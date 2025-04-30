+++
title = 'How to Create a Self-Signed Certificate for NGINX on macOS'
date = 2018-02-25T00:00:00-00:00
lastmod = 2025-04-30T00:00:00-00:00
draft = false
description = '''
This guide provides a walkthrough for how to create a self-signed SSL/TLS
certificate for an NGINX web server on macOS.
'''
tags = ['programming', 'nginx', 'macos']
+++

This guide provides a walkthrough for how to create a self-signed SSL/TLS
certificate for an NGINX web server on macOS[^1]. Additionally, I created a
GitHub repository,
[Self-Signed](https://github.com/nickolashkraus/self-signed), which provides a
working example.

## Overview

TLS, or Transport Layer Security, and its predecessor SSL, which stands for
Secure Sockets Layer, are web protocols used to wrap normal traffic in an
encrypted wrapper.

Using this technology, servers can send traffic safely between servers and
clients without the possibility of the messages being intercepted by outside
parties. The certificate system also helps users verify the identity of the
sites they are connecting to.

While a self-signed certificate will encrypt communication between your server
and any clients, it cannot be used to verify the authenticity of the
connection. This is because a self-signed certificate is not signed by a
trusted certificate authority (CA) included in the browser's root store.

A CA-signed certificate is preferred in *all* cases where the web interface is
user-facing, however there are instances where creating a self-signed
certificate is necessary. For example, when setting up a reverse proxy server
in front of a local development server to proxy SSL traffic to the server.

## Understanding SSL/TLS

SSL/TLS works by using the combination of a public certificate and a private
key. The SSL key is kept secret on the server. It is used to encrypt content
sent to clients. The SSL certificate is publicly shared with anyone requesting
the content. It helps verify that the content was encrypted by the legitimate
server holding the private key.

## Step 0 [Update]: Address `net::ERR_CERT_COMMON_NAME_INVALID` in Chrome

The following recommendations were made by [Victor
Combal-Weiss](https://www.linkedin.com/in/victorcombalweiss/)[^2]:

To fix the following error:
>This site is missing a valid, trusted certificate
>(`net::ERR_CERT_COMMON_NAME_INVALID`).

Copy `openssl.cnf`:

```bash
cp /System/Library/OpenSSL/openssl.cnf openssl.cnf
```

Add the following to `openssl.cnf`:

```bash
[v3_ca]
subjectAltName = DNS:localhost
```

## Step 1: Create the SSL Certificate

1. Create the required directories:

    ```bash
    mkdir -p /usr/local/etc/ssl/private
    mkdir -p /usr/local/etc/ssl/certs
    ```

2. Create a key and certificate pair:

    ```bash
    sudo openssl req \
      -x509 -nodes -days 365 -newkey rsa:2048 \
      -subj "/CN=localhost" \
      -config nginx/openssl.cnf \
      -keyout /usr/local/etc/ssl/private/self-signed.key \
      -out /usr/local/etc/ssl/certs/self-signed.crt
    ```

    **NOTE**: This will create both a key file and a certificate.

## Step 2: Create a Diffie-Hellman Key Pair

```bash
sudo openssl dhparam -out /usr/local/etc/ssl/certs/dhparam.pem 128
```

**NOTE**: This will take a *long* time depending on how many bits are
specified. For demonstration purposes, 128 bits is sufficient.

## Step 3: Create *snippets* to use with NGINX

1. Create the required directory:

    ```bash
    mkdir -p /usr/local/etc/nginx/snippets
    ```

2. Create the `self-signed.conf` snippet file:

    ```bash
    touch /usr/local/etc/nginx/snippets/self-signed.conf
    ```

3. Set `ssl_certificate` and `ssl_certificate_key`:

    ```bash
    sudo vim /usr/local/etc/nginx/snippets/self-signed.conf
    ```

    `self-signed.conf`

    ```bash
    ssl_certificate /usr/local/etc/ssl/certs/self-signed.crt;
    ssl_certificate_key /usr/local/etc/ssl/private/self-signed.key;
    ```

4. Create the `ssl-params.conf` snippet:

    ```bash
    sudo vim /usr/local/etc/nginx/snippets/ssl-params.conf
    ```

    `ssl-params.conf`[^3]

    ```bash
    ssl_protocols TLSv1.1 TLSv1.2;
    ssl_prefer_server_ciphers on;
    ssl_ciphers "EECDH+AESGCM:EDH+AESGCM:AES256+EECDH:AES256+EDH";
    ssl_ecdh_curve secp384r1;
    ssl_session_cache shared:SSL:10m;
    ssl_session_tickets off;
    ssl_stapling on;
    ssl_stapling_verify on;
    resolver 8.8.8.8 8.8.4.4 valid=300s;
    resolver_timeout 5s;
    add_header Strict-Transport-Security "max-age=63072000; includeSubdomains";
    add_header X-Frame-Options DENY;
    add_header X-Content-Type-Options nosniff;
    ssl_dhparam /usr/local/etc/ssl/certs/dhparam.pem;
    ```

## Step 4: Configure NGINX to use SSL/TLS

1. Configure `nginx.conf`:

    ```bash
    sudo vim /usr/local/etc/nginx/nginx.conf
    ```

    `nginx.conf`

    ```conf
    worker_processes  1;
    
    events {
      worker_connections  1024;
    }
    
    http {
      include       mime.types;
      default_type  application/octet-stream;
    
      sendfile           on;
      keepalive_timeout  65;
      proxy_http_version 1.1;
    
      # configure nginx server to redirect to HTTPS
      server {
        listen       80;
        server_name  localhost;
        return 302 https://$server_name:443;
      }
    
      # configure nginx server with ssl
      server {
        listen       443 ssl http2;
        server_name  localhost;
        include self-signed.conf;
        include ssl-params.conf;
    
        # route requests to the local development server
        location / {
          proxy_pass http://localhost:1313/;
        }
      }
    
      include servers/*;
    }
    ```

    **NOTE**: Any hard-coded references using *http* must be changed to use
    *https*.

## Step 5: Restart the NGINX server

1. Check for syntax errors in `nginx.conf`:

    ```bash
    sudo nginx -t
    ```

2. Restart the NGINX server:

    ```bash
    sudo nginx -s stop && sudo nginx
    ```

## Step 6: Test your encryption

Navigate to [https://localhost](https://localhost).

Because the certificate we created isn't signed by one of the system's trusted
certificate authorities, you should be greeted with a big warning sign and the
admonition **Your connection is not private**. To resolve this, add the
self-signed certificate to the system's trusted root store.

## Step 7: Add the self-signed certificate to the trusted root store

```bash
sudo security add-trusted-cert \
  -d -r trustRoot \
  -k /Library/Keychains/System.keychain /usr/local/etc/ssl/certs/self-signed.crt
```

[^1]: This guide is based on [How To Create a Self-Signed SSL Certificate for
Nginx in Ubuntu 16.04][How To Create a Self-Signed SSL Certificate for
Nginx in Ubuntu 16.04].
[^2]: I would like to thank [Victor
Combal-Weiss](https://www.linkedin.com/in/victorcombalweiss/) for informing
me of Chrome's updated trusted certificate policy.
[^3]: TLSv1.1 is deprecated and should not be used in production environments.

[How To Create a Self-Signed SSL Certificate for Nginx in Ubuntu 16.04]: https://www.digitalocean.com/community/tutorials/how-to-create-a-self-signed-ssl-certificate-for-nginx-in-ubuntu-16-04
