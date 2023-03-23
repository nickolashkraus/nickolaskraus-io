---
title: "Copy as cURL"
date: 2023-02-17T00:00:00-06:00
draft: false
description: Use "Copy as cURL" to send an HTTP request with the proper headers.
tags: ["programming", "shell"]
---

While writing a short script to automate the download of a collection of EPUB files, I noticed that the content downloaded using cURL and the content download via the webpage were different. Notably, the content downloaded using cURL contained what appeared to be a redirection to the original HTML webpage.

```bash
URL='https://www.fadedpage.com/link.php?file=20151102.epub'
$ curl -v -X GET -L "${URL}" -o ebook.epub
...
< HTTP/2 200
< content-type: text/html; charset=UTF-8
...
```

I started diagosing the issue by examining the request made via the webpage using the Firefox Developer Tools. I noticed the request made via the webpage added several headers, which I had not added to the request made using cURL. Additionally, during my debugging I learned that you can copy the request as a cURL command by right-clicking the request in the **Network** tab and selecting **Copy Value** > **Copy as cURL**. The cURL request looked like this:

```bash
$ curl -v -X GET -L "${URL}" -o ebook.epub \
-H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10.15; rv:109.0) Gecko/20100101 Firefox/110.0' \
-H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,*/*;q=0.8' \
-H 'Accept-Language: en-US,en;q=0.5' \
-H 'Accept-Encoding: gzip, deflate, br' \
-H 'Referer: https://www.fadedpage.com/showbook.php?pid=20151102' \
-H 'DNT: 1' \
-H 'Connection: keep-alive' \
-H 'Cookie: PHPSESSID=pkde44k2eqp4mvvvmueaqdscu6' \
-H 'Upgrade-Insecure-Requests: 1' \
-H 'Sec-Fetch-Dest: document' \
-H 'Sec-Fetch-Mode: navigate' \
-H 'Sec-Fetch-Site: same-origin'
...
< HTTP/2 200
< content-type: application/octet-stream
< content-length: 885413
< content-description: File Transfer
< content-disposition: attachment; filename=20151102.epub
< content-transfer-encoding: binary
```

That worked! I then removed one header at a time to determine which header allowed for the file download. I narrowed it down to the `Cookie: PHPSESSID=pkde44k2eqp4mvvvmueaqdscu6` header. `PHPSESSID` is a session cookie that is used to identify a user's session on a website. Instead of navigating to the website to get a valid session cookie, I could retrieve a fresh one via cURL and use it in subsequent requests to download content:

```bash
while IFS=':' read -r key value; do
  case "${key}" in
    "set-cookie") SET_COOKIE="${value}";;
  esac
done < <(curl --silent --head --location "${url}")
PHPSESSID="${SET_COOKIE#*PHPSESSID=}";PHPSESSID="${PHPSESSID%;*}"
curl --silent -X GET -L "${url}" -o "${filename}" \
  -H "Cookie: PHPSESSID=${PHPSESSID}"
```

```bash
$ curl -v -X GET -L "${URL}" -o ebook.epub \
-H "Cookie: PHPSESSID=${PHPSESSID}"
...
< HTTP/2 200
< content-type: application/octet-stream
< content-length: 885413
< content-description: File Transfer
< content-disposition: attachment; filename=20151102.epub
< content-transfer-encoding: binary
```

Viola! With just a few hours of initial time investment a task that would have taken 5 minutes of clicking can know be completed in 5 seconds 😉.
