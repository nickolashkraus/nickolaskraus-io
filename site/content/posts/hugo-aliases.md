+++
title = 'Hugo Aliases'
date = 2019-11-10T00:00:00-00:00
lastmod = 2025-08-22T00:00:00-00:00
draft = false
description = '''
Use Hugo aliases to create redirects to your page from other URLs.
'''
tags = ['hugo']
aliases = ['/articles/hugo-aliases/']
+++

## Overview

It is generally considered best practice to never change the URL of a resource
on the internet. This is because other resources may reference your resource
and if the URL to your resource has changed these references will break.

If the URL of a resource needs to be changed, the original URL should become
a redirect to the new URL. This is a fundamental feature of the internet and is
addressed at length in the [Hypertext Transfer Protocol specification (RFC
2616)][Hypertext Transfer Protocol specification (RFC 2616)].

If that were not cause enough to adhere to redirection, Google penalizes
indexed content that has moved (e.g., is now returning a 404), which will hurt
SEO.

## Hugo Aliases

[Hugo aliases][Hugo aliases] allow you to create redirects to your page from
other URLs. This is useful if you change the title of an article and want the
URL slug to match.

To add an alias to content, simply add an `aliases` key to your content's
frontmatter:

`content/posts/new-file-name.md`

**TOML**

```toml
+++
aliases = ['/posts/previous-file-name']
+++
```

**YAML**

```yaml
---
aliases: ['/posts/previous-file-name']
---
```

- An alias with a leading slash is relative to the `BaseURL`.
- An alias without a leading slash is relative to the current directory.

The alias from the previous URL to the new URL is a client-side redirect, as
opposed to a server-side redirect, and will work for content that is not
retrieved from a server (e.g., Amazon CloudFront or Amazon S3).

`posts/previous-file-name/index.html`

```html
<!doctype html>
<html lang="en-us">
  <head>
    <title>https://example.org/posts/new-file-name/</title>
    <link rel="canonical" href="https://example.org/posts/new-file-name/" />
    <meta name="robots" content="noindex" />
    <meta charset="utf-8" />
    <meta http-equiv="refresh" content="0; url=https://example.org/posts/new-file-name/" />
  </head>
</html>
```

Collectively, the elements in the head section:

- Tell search engines that the new URL is canonical.
- Tell search engines not to index the previous URL.
- Tell the browser to redirect to the new URL.

[Hugo aliases]: https://gohugo.io/content-management/urls/#aliases
[Hypertext Transfer Protocol specification (RFC 2616)]: https://tools.ietf.org/html/rfc2616#section-10.3
