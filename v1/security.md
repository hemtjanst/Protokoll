---
layout: default
title: Security and Privacy
parent: v1
nav_order: 20
---

# Security and Privacy
{: .no_toc }

This document sets forward a number of requirements and recommendations with
regards to securing this environment and ensuring confidentiality of the
data.
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## TLS

The broker MUST require TLS for all client connections. At least TLS
version 1.2 MUST be supported. Connections using earlier versions of
TLS older than 1.2 MUST be denied.

Clients MUST NOT be able to establish a connection to the broker in
plain text.

It is RECOMMENDED to apply
[RFC 7525](https://tools.ietf.org/html/rfc7525), "Recommendations
for Secure Use of Transport Layer Security (TLS) and Datagram Transport Layer
Security (DTLS)", especially the section on Cipher Suites.

## Authentication

The broker and all actors and providers MUST support username/password
authentication. They MAY support client certificate based authentication.

It is NOT RECOMMENDED to allow anonymous connections to the broker.

## Authorization

It is RECOMMENDED to apply ACLs to the different topics in order to enforce
proper behaviour by all actors and providers.

If anonymous connectivity to the broker is allowed it is RECOMMENDED to apply
an ACL that ensures anonymous clients are unable to publish to the
[`desired` topics]({{ site.baseurl }}{% link v1/actions.md %}#desired-state).

## Privacy

By enforcing TLS we ensure that nobody can eaves-drop on the communication
between the broker and the actors or providers. It also ensures a malicious
actor is unable to get valid credentials by observing network traffic.

By mandating authentication we can ensure only trusted components can
provide devices or gain access to their state and act on them. This puts us
in control of who has access to what data and when. It should be noted that
certain bridges can expose some of the data on the broker in a way that does
not require authentication.

By leveraging ACLs we can further lock down the amount of access each part
of the system has. This can be used to limit access to specific devices and
ensure components cannot do something malicious or against the specification.
