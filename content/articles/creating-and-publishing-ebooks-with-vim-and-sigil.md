---
title: "Creating and Publishing eBooks with Vim and Sigil"
date: 2023-02-17T00:00:00-06:00
draft: true
description: Use "Copy as cURL" to send an HTTP request with the proper headers.
tags: ["vim", "sigil"]
---

I love editing text in Vim, however I recently ran into a use case that required third-party software: creating and publishing eBooks (EPUB files).

My naive workflow would be:

Extract the compressed files of the EPUB file (ZIP archive):

```bash
$ unzip casino-royale.epub -d casino-royale
```

Do some editing

Then repackage and compress the files back into an EPUB:

```bash
$ zip -rX "../$(basename "$(realpath .)").epub" mimetype $(ls|xargs echo|sed 's/mimetype//g')
```

I would then needs to import the EPUB into a viewer to see the changes.


