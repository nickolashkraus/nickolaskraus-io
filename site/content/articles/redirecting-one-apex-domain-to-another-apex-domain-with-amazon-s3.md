---
title: "Redirecting One Apex Domain to Another Apex Domain with Amazon S3"
date: 2022-05-12T00:00:00-06:00
draft: false
description: How to redirect one apex domain to another apex domain with Amazon S3.
tags: ["aws", "route53", "s3"]
---

I decided to migrate my personal website from NickolasKraus.org to NickolasKraus.io. As I've written in the past,

>It is generally considered best practice to never change the URL of a resource on the internet. This is because other resources may reference your resource and if the URL to your resource has changed these references will break.

Additionally, I did not want to nullify the PageRank I'd developed over the years.

To this effect, I needed to redirect all requests destined for NickolasKraus.org to NickolasKraus.io.

Naively, I assumed I could simply use a [CNAME](https://en.wikipedia.org/wiki/CNAME_record) to map NickolasKraus.org to NickolasKraus.io. However, when testing this in the AWS Management Console, I received the following error:

```
Bad request.
(InvalidChangeBatch 400: RRSet of type CNAME with DNS name nickolaskraus.org. is not permitted at apex in zone nickolaskraus.org.)
```

After thinking through some possible solutions, I landed on the following:

>You can redirect all requests to a website endpoint for a bucket to another bucket or domain. If you redirect all requests, any request made to the website endpoint is redirected to the specified bucket or domain.

[Source](https://docs.aws.amazon.com/AmazonS3/latest/userguide/how-to-page-redirect.html#redirect-endpoint-host)

This feature of static website hosting with Amazon S3 facilitates redirection of requests from one domain to another. Notably, from one *apex* domain to another *apex* domain.

To accomplished this, I extended the Terraform module that I use for hosting static websites with the following:

```
resource "aws_s3_bucket_website_configuration" "s3_root" {
  count  = var.redirect_domain_name == "" ? 1 : 0
  bucket = aws_s3_bucket.s3_root.bucket

  index_document {
    suffix = "index.html"
  }

  error_document {
    key = "404.html"
  }
}

resource "aws_s3_bucket_website_configuration" "s3_root_redirect" {
  count  = var.redirect_domain_name != "" ? 1 : 0
  bucket = aws_s3_bucket.s3_root.bucket

  redirect_all_requests_to {
    host_name = var.redirect_domain_name
    protocol  = "https"
  }
}
```

When the `redirect_domain_name` is provided, the S3 bucket is configured to redirect requests to another domain (i.e. NickolasKraus.io).

**NOTE**: The code for this Terraform module can be found here:
* [infrable-io/terraform-aws-static-website](https://github.com/infrable-io/terraform-aws-static-website)

After deploying NickolasKraus.io and configuring NickolasKraus.org for redirection, requests were dutifully redirected.

One small annoyance is that requests to https://nickolaskraus.org are routed to https://nickolaskraus.io/index.html. I think I can live with this small imperfection...
