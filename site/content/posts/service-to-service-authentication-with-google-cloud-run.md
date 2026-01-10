+++
title = 'Service-to-Service Authentication with Google Cloud Run'
date = 2026-01-07T00:00:00-00:00
lastmod = 2026-01-07T00:00:00-00:00
draft = false
description = '''
The documentation that should exist for service-to-service authentication with
Google Cloud Run.
'''
tags = ['programming', 'google-cloud', 'cloud-run']
+++

I'll be frank: Google's documentation is not good. This post serves as the
documentation that *should* exist for service-to-service authentication with
Google Cloud Run.

The repository which accompanies this post can be found here:
- [nickolashkraus/gcp-oidc][nickolashkraus/gcp-oidc]

## Overview

First, let's get some definitions out of the way:

- **Google Cloud Run**: A fully managed, serverless platform on Google Cloud
  for running containerized applications. Cloud Run automatically handles
  scaling, routing, HTTPS, security, etc.
- **Google-signed OIDC token**: A JSON Web Token (JWT) issued by Google's
  OpenID Connect provider and signed with Google's private keys. It proves the
  identity of the caller and the intended audience. Google-signed OIDC tokens
  are used for service-to-service authentication with Google Cloud Run.

## Official Documentation

Google provides official documentation on service-to-service authentication
[here][Authenticating service-to-service]. I'll provide a brief synopsis with
examples and clarify where the documentation is lacking technical depth.

For synchronous communication (i.e., HTTP-based request/response
communication), a service calls another service directly using its endpoint
URL. For this use case, Google recommends making the receiving service only
callable by the requesting service. This is accomplished using IAM and
a service identity[^1] that has been granted permissions to allow another
service to call it.

So, for example, if Service A (calling service) needs to make authenticated
requests to Service B (receiving service), Service B must grant that permission
to Service A[^2].

To set up a service account, you configure the receiving service to accept
requests from the calling service by making the calling service's service
account a *principal* on the receiving service. Then you grant that service
account the Cloud Run Invoker (`roles/run.invoker`) role.

**Example: Service Account**

```terraform
resource "google_cloud_run_v2_service_iam_binding" "service_b" {
  name    = google_cloud_run_v2_service.service_b.name
  members = ["serviceAccount:${google_service_account.service_a.email}"]
  role    = "roles/run.invoker"

  # NOTE: `allUsers` makes the service publicly accessible.
  # members = ["allUsers"]
}
```

Then, to make authenticated requests, the calling service must present proof of
the calling service's identity. To do this, the calling service must add
a Google-signed OpenID Connect ID token to the request.

**Example: Service A**

To generate a Google-signed OpenID Connect ID token, use Google's
authentication library, [google-auth][google-auth]:

1. Fetch a token.
2. Set the audience claim (`aud`).

    **NOTE**: The audience claim should be the URL of the receiving service.

3. Add the token to the request.

    **Example**

    ```
    Authorization: Bearer <TOKEN>
    ```

    ```
    X-Serverless-Authorization: Bearer <TOKEN>
    ```

    The token can be added to either the `Authorization` or
    `X-Serverless-Authorization` header. If your application already uses the
    `Authorization` header for custom authorization, you can provide the token
    using `X-Serverless-Authorization` instead. Keep in mind that Cloud Run may
    modify the `Authorization` header, which is why the alternative header
    exists. If the token is provided using `X-Serverless-Authorization`, the
    signature is removed before passing the token to the receiving service[^3].
    If you provide both headers, only the `X-Serverless-Authorization` header
    is checked.

```python
import requests
from google.auth.transport.requests import Request as GoogleAuthRequest
from google.oauth2.id_token import fetch_id_token


def make_authorized_request(endpoint: str, audience: str) -> requests.Response:
    """Make an authenticated request to a Cloud Run service.

    The request is authenticated using the ID token obtained from the
    google-auth client library using the specified audience value.

    NOTE: Cloud Run's authenticating proxy uses the `aud` claim to validate the
    token. Therefore, you must set the audience to the URL of the receiving
    service. The endpoint is the API endpoint (base URL + path) that will
    receive/handle the request.

    See:
      - https://github.com/googleapis/google-auth-library-python

    Args:
        endpoint: URL of the request (e.g., https://service.a.run.app/api/v1/).
        audience: Cloud Run service URL used for token validation (typically
            the same as endpoint's base URL unless using a custom audience).

    Returns:
        HTTP response for the request.

    Raises:
        requests.HTTPError: If the request returns an error status code.
        google.auth.exceptions.GoogleAuthError: If token retrieval fails.

    Example:
        response = make_authorized_request(
            endpoint='https://service-b.a.run.app/api/v1/',
            audience='https://service-b.a.run.app'
        )
    """
    token = fetch_id_token(GoogleAuthRequest(), audience)

    headers = {"Authorization": f"Bearer {token}"}

    resp = requests.get(endpoint, headers=headers)
    resp.raise_for_status()

    return resp
```

[Source][request.py]

**Example: Service B**

Within the receiving service, the Google-signed OpenID Connect ID token is
extracted from the request headers and can be verified by checking the
signature against Google's public keys and validating standard OIDC claims
(`iss`, `aud`, `exp`, etc.).

**Example: Google-signed OpenID Connect ID Token**

```json
{
  "aud": "https://service-b.a.run.app",
  "azp": "000000000000000000000",
  "email": "000000000000-compute@developer.gserviceaccount.com",
  "email_verified": true,
  "exp": 1234567890,
  "iat": 1234567890,
  "iss": "https://accounts.google.com",
  "sub": "000000000000000000000"
}
```

1. Extract the token from the request headers.
2. Verify the token and validate its claims.

```python
import http

import jwt
from fastapi import HTTPException, Request
from google.auth.exceptions import GoogleAuthError
from google.auth.transport.requests import Request as GoogleAuthRequest
from google.oauth2.id_token import verify_oauth2_token
from jwt import PyJWTError


def verify_authorized_request(request: Request, expected_audience: str) -> str:
    """Verify an authenticated request from a Cloud Run service.

    1. Extracts the token from the request headers.
    2. Decodes the token and validates its claims (`iss`, `aud`).

       NOTE: The signature is not validated when the token is provided using
       the `X-Serverless-Authorization` header.

    3. Returns the `email` claim.

    WARNING: Google automatically removes the signature of a JWT sent with the
    `X-Serverless-Authorization` header. Therefore, it is impossible to verify
    the token in the application code. For this reason, we must rely on Cloud
    Run's authenticating proxy and IAM for authentication if the token is
    provided using this header instead of the standard `Authorization` header.

    Furthermore, even if a service is publicly accessible, Google will still
    remove the signature of the JWT.

    Args:
        request: FastAPI Request object.
        expected_audience: Audience value the token must contain.

    Returns:
        Service account email from the validated token.

    Raises:
        HTTPException: If the token is missing, invalid, or malformed.

    Example:
        @app.get("/api/v1")
        def handle_request(request: Request):
            email = verify_authorized_request(
                request, expected_audience="https://my-service.a.run.app"
            )
            return email
    """
    auth_header = request.headers.get("Authorization")
    serverless_auth_header = request.headers.get("X-Serverless-Authorization")

    if not (auth_header or serverless_auth_header):
        raise HTTPException(
            status_code=http.HTTPStatus.UNAUTHORIZED,
            detail="Missing authorization header",
        )

    try:
        # If both headers are provided, only check the
        # `X-Serverless-Authorization` header.
        if serverless_auth_header:
            auth_type, token = serverless_auth_header.split(" ", 1)
        else:
            auth_type, token = auth_header.split(" ", 1)
    except ValueError:
        raise HTTPException(
            status_code=http.HTTPStatus.UNAUTHORIZED,
            detail="Malformed authorization header",
        )

    if auth_type.lower() != "bearer":
        raise HTTPException(
            status_code=http.HTTPStatus.UNAUTHORIZED,
            detail="Unsupported authentication type: %s" % auth_type,
        )

    try:
        # WARNING: The following will always produce a 'Could not verify token
        # signature' error if the token is provided using the
        # `X-Serverless-Authorization` header:
        #
        #   claims = verify_oauth2_token(
        #       token, GoogleAuthRequest(), expected_audience
        #   )
        #
        # Therefore, the JWT must be decoded without verifying the signature.
        # If the service is publicly accessible, you can basically use whatever
        # JWT you want as long as it is added to the
        # `X-Serverless-Authorization` header and decodes properly.
        #
        # In order to verify the token via code, it must be provided using the
        # `Authorization` header. If both headers are provided, only the
        # `X-Serverless-Authorization` header is checked by the Google Cloud
        # Run platform.
        if serverless_auth_header:
            claims = jwt.decode(
                token,
                options={
                    "verify_signature": False,
                    "verify_aud": True,
                    "verify_iss": True,
                },
                audience=expected_audience,
                issuer=["https://accounts.google.com", "accounts.google.com"],
            )
        else:
            claims = verify_oauth2_token(
                token, GoogleAuthRequest(), expected_audience
            )
        email = claims.get("email")
        if not email:
            raise HTTPException(
                status_code=http.HTTPStatus.UNAUTHORIZED,
                detail="Token missing `email` claim",
            )
        return email
    except GoogleAuthError as exc:
        raise HTTPException(
            status_code=http.HTTPStatus.UNAUTHORIZED,
            detail="Invalid token: %s" % exc,
        ) from exc
    except PyJWTError as exc:
        raise HTTPException(
            status_code=http.HTTPStatus.UNAUTHORIZED,
            detail="Invalid token: %s" % exc,
        ) from exc
```

[Source][receive.py]

This part of the documentation would benefit from greater technical depth,
since if you attempt to use the `X-Serverless-Authorization` header to pass the
Google-signed OIDC token, you cannot verify the token via code.
Application-level token validation is not possible because Google removes the
signature of the JWT before passing the token to the service. The signature of
the JWT is replaced with `SIGNATURE_REMOVED_BY_GOOGLE`, which causes token
verification to fail.

## `SIGNATURE_REMOVED_BY_GOOGLE`

Ideally, the authentication of service-to-service communication is facilitated
transparently via [Identity-Aware Proxy (IAP)][Identity-Aware Proxy (IAP)].

A receiving service is configured to accept requests from a calling service by
making the calling service's service account a *principal* on the receiving
service. Then you grant that service account the Cloud Run Invoker
(`roles/run.invoker`) role.

A calling service then includes a Google-signed OIDC token in the request to
the receiving service and the token is verified automatically. You do not need
specific code within the receiving service to verify the Google-signed OIDC
token, since the Google Cloud Run platform (specifically the Identity-Aware
Proxy/IAP layer that sits in front of the service) automatically performs the
token validation for you.

In the process of verifying the OIDC token, Google (specifically IAP) removes
the signature of the token before passing the request to the receiving service
**only** if the token is provided using the `X-Serverless-Authorization`
header. This is done as a security measure in order to prevent identity token
reuse[^4]. The signature segment of the JWT is replaced with
`SIGNATURE_REMOVED_BY_GOOGLE`, rendering the token invalid (though the claims
are still retrievable).

**Example**

```
<header>.<payload>.SIGNATURE_REMOVED_BY_GOOGLE
```

When IAM authentication is enforced, Google validates the ID token's signature
internally before the application code runs. If the token is valid, Google
authenticates the request. The signature is then stripped to ensure the token
cannot be used elsewhere. When the application receives a token with this
message (or an empty signature), it does **not** mean Google has already
verified the token's authenticity. If the service is publicly accessible,
Google does not authenticate requests, but still removes the token's signature.
Therefore, an application cannot trust the claims within the token's payload,
since token verification cannot be performed. In such cases, the token can
still be decoded and its claims extracted, but the receiving service has no
way of verifying those claims.

Google advises against using a custom authentication scheme via the
standard `Authorization` header because it is managed and modified by the
platform. However, if an application already uses secret/API keys for
authentication, the OIDC token authentication method **cannot** exist alongside
this method unless the `Authorization` is used for authentication (API key) and
the alternative header, `X-Serverless-Authorization`, is used for
quasi-authorization using the token's claims.

In short, `SIGNATURE_REMOVED_BY_GOOGLE` does not necessarily mean the token is
legitimate and has already been verified by the GCP infrastructure, since IAM
dictates if the receiving service is reachable by the calling service[^5].

## Summary

In practice, Google-signed OIDC tokens should be used to authenticate
service-to-service communication. There are, however, subtle nuances in how
this should be implemented. I found this out the hard way when I attempted to
use custom authentication (API keys) using the `Authorization` in addition to
`X-Serverless-Authorization` with tokens. This approach fundamentally does not
work, since, in order to allow requests using API keys, the receiving service
must be publicly accessible (`allUsers`). I incorrectly assumed that Google
would:
  1. Allow tokens passed using `X-Serverless-Authorization` to still be
     verified via code.
  2. Verify a token at the platform-layer before removing the signature and
     passing the token to the service.

Both assumptions were incorrect. The only viable solution would be to:
  1. Configure the service account of the receiving service to only be
     accessible by the calling service.
  2. Provide a Google-signed OIDC token using the request headers.

Suffice it to say, API keys and OIDC tokens do not mix on Google Cloud Run.

## The Landlord and the Tenant

Let's say you have the following problem:

>The authentication scheme of your microservice architecture uses API keys
>provided by the `Authorization` header. You want to transition to OIDC tokens,
>but, as stated above, you cannot simultaneously provide an API key and OIDC
>token using the `Authorization` and `X-Serverless-Authorization` header,
>respectively. This is due to the fact that in order to allow requests to be
>authenticated via API keys, the service must be publicly accessible, but, this
>prevents Google from verifying the OIDC token, and furthermore, removes the
>signature from the token, so the token cannot be verified via code.

This problem can be restated using the following analogy:

>A tenant is provided a key to his apartment by a landlord. The landlord wishes
>to change the lock on the apartment, but due to their busy schedules, they can
>never synchronize being at the apartment at the same time. If the landlord
>changes the lock, the tenant will not be able to unlock his apartment with the
>old key. If the landlord first exchanges the keys, the tenant will only be
>able to get into the apartment when the new lock is installed.
>
>What are they to do?
>
>The solution is for the landlord to provide the tenant with both keys, so that
>he has the ability to unlock his apartment both before and after the new lock
>is installed by simply trying both. The first key unlocks the apartment until
>the new lock is installed then the second key unlocks the apartment.
>Thereafter, the first key can be destroyed.

The key synchronization problem is due to the fact that both the calling and
receiving service must be modified at the same time, which is not possible
without downtime. The two keys analogize to the API key and OIDC token, which
can be tried in succession: while the receiving service is publicly accessible,
the API key provides authentication, but once the service account is updated to
only allow the calling service using an OIDC token, the API key will fail and
only the OIDC token will authenticate the request.

[nickolashkraus/gcp-oidc]: https://github.com/nickolashkraus/gcp-oidc
[Authenticating service-to-service]: https://docs.cloud.google.com/run/docs/authenticating/service-to-service
[google-auth]: https://github.com/googleapis/google-auth-library-python
[request.py]: https://github.com/nickolashkraus/gcp-oidc/blob/master/examples/request.py
[receive.py]: https://github.com/nickolashkraus/gcp-oidc/blob/master/examples/receive.py
[Identity-Aware Proxy (IAP)]: https://cloud.google.com/security/products/iap

[^1]: The identity of a Cloud Run service is referred to as the *Cloud Run
service identity*. When a Cloud Run service calls another Cloud Run
service, the Cloud Run service identity is used for authentication.

[^2]: Conceptually, this is easier to understand than, say, IAM with AWS,
where both the caller and receiver need bidirectional permissions: the caller
to call the receiver, and the receiver to receive calls from the caller.

[^3]: Had I only internalized this sentence, I would have avoided hours of
debugging. If a token is provided using the `X-Serverless-Authorization`
header, the signature of the JWT is replaced with
`SIGNATURE_REMOVED_BY_GOOGLE`. This is not the case if using the
`Authorization` header, in which case the token is passed to the receiving
service unaltered.

[^4]: Impersonation via token reuse (also known as the *confused deputy
problem*), is a security risk whereby an entity that doesn't have
permission to perform an action can coerce a more-privileged entity to
perform the action. If an identity token with a valid signature can be read
by an application, that application (or a malicious actor who compromised
it) could then use the same token to impersonate the original user or
service account when calling other services.

[^5]: Even though Google removes the token's signature, if the service allows
public access, Google does not validate the token. It simply lets it pass
through, while still removing the token's signature.
