---
layout: default
title: Multitenancy
parent: v1
nav_order: 30
---

# Multitenancy
{: .no_toc }

This document sets forth some requirements in order to enable multitenancy.
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Tenant prefix

In order to achieve multitenancy each tenant MUST be assigned its own
unique identifier. This identifier MUST be stable for the lifetime of
the tenant.

All topics as set forth in these documents MUST be prefixed with the
tenant identifier. This would result in the following topic layout:

* `tenant 1/`
  * `current/`
  * `desired/`
  * `device/`
  * `provider/`
* `tenant 2/`
  * `current/`
  * `desired/`
  * `device/`
  * `provider/`

## Authentication

If multitenancy is implemented anonymous connectivity to the broker MUST NOT
be allowed. All connections to the broker MUST require authentication.

## Authorization

In order to ensure two tenants cannot gain access to each other's data or
control devices in the other's home, ACLs MUST be applied to ensure `tenant 1`
cannot publish or subscribe to topics of `tenant 2` and vice-versa.
