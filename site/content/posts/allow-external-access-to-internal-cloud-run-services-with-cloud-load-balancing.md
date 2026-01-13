+++
title = 'Allow External Access to Internal Cloud Run Services with Cloud Load Balancing'
date = 2026-01-13T00:00:00-00:00
lastmod = 2026-01-13T00:00:00-00:00
draft = false
description = '''
How to allow external access to internal Cloud Run services with Cloud Load
Balancing.
'''
tags = ['programming', 'google-cloud', 'cloud-load-balancing']
+++

The following provides an architecture design for allowing external entities to
access internal Cloud Run services in a secure, scalable way.

## Problem

A typical microservice architecture consists of serverless applications (Cloud
Run, Cloud Functions, etc.). Most of these services are only accessible by
other allowed internal Google producer traffic, such as other Cloud Run
services. They are not directly accessible from the public internet unless
specifically configured (see [Restrict network ingress for Cloud Run][Restrict
network ingress for Cloud Run]).

This means that services cannot easily allow public traffic directly from the
internet. In the case of a microservice that needs to be publicly accessible,
this makes the communication between an external entity and the service
impossible without opening the service to all external requests.

## Solution

A potential solution is to create an Application Load Balancer (ALB) that
proxies traffic from the Internet to backend services (like Cloud Run) while
still restricting network ingress. This allows Cloud Run services to only be
reachable from Cloud Load Balancing and other allowed internal Google producer
traffic (i.e., Cloud Run, Cloud Functions, etc.), but not directly from the
public internet:

![Cloud Load Balancing](/images/cloud-load-balancing.svg)

This approach scales cleanly as more services are added to the system, since
the ALB can route to multiple backends using host- and path-based routing.
Additionally, the implementation can layer on [Cloud Armor][Cloud Armor] (WAF,
DDoS protection, rate limiting, etc.) to provide a robust, centralized ingress
into the microservice architecture.

[Restrict network ingress for Cloud Run]: https://docs.cloud.google.com/run/docs/securing/ingress
[Cloud Armor]: https://cloud.google.com/security/products/armor
