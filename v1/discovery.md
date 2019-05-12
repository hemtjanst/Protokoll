---
layout: default
title: Announce & Discover
parent: v1
nav_order: 10
---

# Device Announcement and Discovery
{: .no_toc }

This document details how to discover currently available devices on the
broker and how to register new devices.
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## ID values

In order to be able to keep track of devices and providers a set of IDs will
be necessary. There are a few rules to follow with regards to generating these
IDs:

* Device IDs MUST be unique on the broker and MUST be stable for the
  lifetime of the device
* Provider IDs are considered ephemeral but MUST NOT change while the
  provider is connected to the broker. Provider IDs MUST be unique on the
  broker
* Device IDs and provider IDs MUST NOT collide

It is RECOMMENDED that UUIDv4 is used to generate provider IDs. Device IDs
MAY use a UUIDv4 and store this or use another method to generate a device
ID.

## Joining

When a provider joins the broker it MUST complete certain actions. Which
actions depend on whether the provider is a single device or if it's a
bridge.

### Single device

This implies that the device is its own provider.

When connecting to the broker the Last Will and Testament MUST be configured
in the following manner:

* Last Will Topic: `current/<root topic>/reachable`
* Last Will Messsage: `"0"`
* Last Will Retain: `true`
* Last Will QoS: `2`

Once the connection with the broker is established the device must announce
itself:

* Publish with retain to `device/<id>` the device metadata
* Publish with retain to `current/<root topic>/reachable` the value `"1"`

### Bridge

This implies that the provider is a piece of software or hardware that can
expose one or multiple devices.

When connecting to the broker the Last Will and Testament MUST be configured
in the following manner:

* Last Will Topic: `provider/<id>`
* Last Will Messsage: `` (empty payload)
* Last Will Retain: `true`
* Last Will QoS: `2`

Once the connection with the broker is established the bridge must announce
itself and all its devices:

* Publish with retain to `provider/<id>` the provider metadata
* Publish with retain to `device/<id>` the device metadata
  * The bridge MUST set the `providerID` field in every device metadata to
    `<id>` it used when publishing to `provider/<id>`
* Publish with retain to `current/<root topic>/reachable` the value `"1"` for
  any device that it bridges that is available or `"0"` for any device that it
  bridges that is unavailable

Device and provider `<id>` values MUST NOT collide. It is RECOMMENDED to
use a UUIDv4 for these values.

The device `<id>` MUST be stable for the lifetime of the device. The provider
`<id>` is considered ephemeral and MAY change but MUST NOT change while a
provider is connected to the broker.

When joining a provider must set its Last Will And Testament. If a provider is
a single device the Last Will And Testament must be set to:

## Leaving

### Gracefully

When devices want to leave the network they MUST execute a number of steps in
order to ensure all actors become aware of the fact that the device has left.

#### Single device

* Publish with retain to `current/<root topic>/reachable` the value of`"0"`

#### Bridge

For all of its bridged devices:

* Publish with retain to `current/<root topic>/reachable` the value of `"0"`

And finally:

* Publish with retain the empty payload to `provider/<id>`

### Ungracefully

When a provider looses its connection to the broker due to some error
condition one of two things will happen. It is important to note that his
will **only** happen when we disconnect from the broker ungracefully. If
we properly send an MQTT disconnect and then close the connection the
following events will not take place.

#### Single device

The broker will take care of publishing the value `"0"` to
`current/<root topic>/reachable` as that's what we specified in the Last Will
and Testament. No further action is required.

#### Bridge

The broker will publish with retain the empty payload to `provider/<id>` as
that's what we specified in the Last Will and Testament.

A janitor service must now find all devices with the matching `<id>` and for
each of them publish with retain to `current/<root topic>/reachable` the value
of `"0"`.
