---
layout: default
title: Home
nav_order: 1
permalink: /
search_exclude: true
---

# Protokoll

This is an attempt to document the Hemtjänst protocol. You'll find the
specification of how devices and features are encoded as well as the
documentation for the service discovery mechanism.
{: .fs-6 .fw-300 }

Contrary to many other projects Hemtjänst reuses an existing specification,
the Apple HomeKit Accessory Protocol. We use Apple's Accessories, Services
and Characteristic definitions but have extended it with some of our own
in places where Apple's specification falls short. Access to the HAP
specification requires an Apple ID but is otherwise free (you
don't have to be a subscriber of the Apple Developer program).

## Versions

The current version of the protocol is v0. An update to it,
v1, is currently under discussion.

## Specifications

Each version contains two specifications. One is for how to represent
a device and its features. This is what we call Metadata. The metadata
specification also provides the mapping for device features to MQTT
topics.

The second specification is the Service Discovery part. This documents
how devices can discover each other and how to make yourself discoverable.
