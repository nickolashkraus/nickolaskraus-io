+++
title = 'Using SSL/TLS with Google App Engine'
date = 2018-03-04T00:00:00-06:00
lastmod = 2025-05-01T00:00:00-00:00
draft = false
description = '''
Enabling SSL/TLS for your Google App Engine production environment can be done
trivially. Nevertheless, some circumstances require that your local development
server also use SSL/TLS. Since the local development server provided by the
Google Cloud SDK, `dev_appserver.py`, does not come with SSL/TLS out of the
box, some configuration is required to accomplish this.
'''
tags = ['programming', 'google-app-engine']
+++

Enabling SSL/TLS for your Google App Engine production environment can be done
trivially. Nevertheless, some circumstances require that your local development
server also use SSL/TLS. Since the local development server provided by the
Google Cloud SDK, `dev_appserver.py`, does not come with SSL/TLS out of the
box, some configuration is required to accomplish this.

## In Production

Employing SSL/TLS in production is relatively straightforward. From the Google
Cloud Platform documentation:

> If you want to use native Python SSL, you must enable it by specifying `ssl`
> for the `libraries` configuration in your application's `app.yaml`.

[Source](https://cloud.google.com/appengine/docs/standard/python/sockets/ssl_support)

`app.yaml`

```yaml
libraries:
- name: ssl
  version: latest
```

## For Local Development

Using SSL/TLS with the local development server, `dev_appserver.py`, is
slightly more involved. This solution requires two interventions:

1. Set up a reverse proxy server in front of the local development server to
   proxy SSL traffic to the server.
2. Patching the `requests` Python library so that the `dev_appserver.py` can
   initiate outbound requests over HTTPS.

### Step 1: Set up a reverse proxy server

To solve this, I configured an NGINX server to act as a reverse proxy for SSL
traffic. The walkthrough for accomplishing this on macOS can be found
[here](https://nickolaskraus.io/posts/how-to-create-a-self-signed-certificate-for-nginx-on-macos).

### Step 2: Patch the `requests` Python library

To use requests, you'll need to install both `requests` and
`requests-toolbelt`. Once installed, use the
`requests_toolbelt.adapters.appengine` module to configure requests to use
`URLFetch`:

```python
import requests
import requests_toolbelt.adapters.appengine

# Use the App Engine Requests adapter. This makes sure that Requests uses
# URLFetch.
requests_toolbelt.adapters.appengine.monkeypatch()
```

To issue an HTTPS request, set the `validate_certificate` parameter to true
when calling the `urlfetch.fetch()` method. This is handled transparently in
`requests-toolbelt`
[here](https://github.com/requests/toolbelt/blob/master/requests_toolbelt/adapters/appengine.py#L175)[^1].

[^1]: This file has long since been removed.

[How to Create a Self-Signed Certificate for NGINX on macOS]: http://localhost:1316/posts/how-to-create-a-self-signed-certificate-for-nginx-on-macos/
