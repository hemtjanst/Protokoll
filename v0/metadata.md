---
layout: default
title: Metadata
parent: v0
nav_order: 1
---

# Metadata
{: .no_toc }

Metadata specifies how a device and its capabilities are represented. It also
provides a mapping to MQTT topics in order to be able to control a device
through MQTT.
{: .fs-6 .fw-300 }

|---|---|
| **iOS/HomeKit** | 12.1 |
| **Last Updated** | 2019-05-10 |

## Table of contents
{: .no_toc }

1. TOC
{:toc}

## Device

A device is represented by a JSON document. When published onto MQTT
it is serialised as a string. This document is often refered to as the
*meta* document, since it represents device metadata.

The meta document consists of a number of required and optional keys.
The naming of the keys follows [Google's JSON style guide][json-style] and as
such are in *camelCase*. However, `ID` is always fully uppercase and any
chemical formula expressed in chemical symbols follows its relevant casing, so
`CO2`, not `Co2` for carbon dioxide.

| Key | Required | Type |
|---|---|---|
| `name` | X | `string` |
| `type` | X | `string` |
| `feature` | X | `object` |
| `topic` || `string` |
| `lastWillID` || `UUID` |

### `name`

A human friendly device name. Can be the same thing as the topic name or
something else entirely. This will be the name of the accessory as HomeKit sees
it so do pick something that makes sense and allows you to relatively easily
identify the accessory.

### `type`

The type of device, for example `light` or `CO2Sensor`. These map directly onto
HomeKit services and are considered the "primary" service. There is currently no
support for hidden, secondary or linked services.

You can find the supported devices [here][types] and how they map to HomeKit
services.

### `feature`

This is [discussed in its own section](#feature-1).

### `topic`

The "root" topic of this device, for example `lightbulb/kitchen`. This may also
be the name that you publish to underneath announce, so `announce/lightbulb/kitchen`
is entirely valid and will be used as the root topic unless the `topic` key is present.

If you publish as `announce/lightbulb/kitchen` but the topic is set to `light/kitchen`
the `topic` in meta takes precedence.

### `lastWillID`

The `lastWillID` only has to be set for bridged devices, so in cases where each
device doesn't itself maintain a connection to the MQTT broker. When that is
the case the Last Will And Testament should be set instead to publish the
"root" topic name to the `leave` topic.

The `lastWillID` can be anything but needs to be unique. As such it's recommended
to use a UUIDv4 for this.

## Feature

Every device needs at least one feature to be defined for it, the usual
required characteristic (it can be more than one). Similarly to device the
[characteristics][characteristics] mapping contains a list of which feature
names map to what HomeKit characteristics.

Each key in the feature object represents a feature. A feature has 5 optional
keys. When `min`, `max` and `step` are not defined the defaults from the HAP
spec apply. Though you can specify different values for `min`, `max` and `step`
you cannot change the type. If HAP specifies the type as `uint8` it will be
deserialised as such. Putting in a `float64` would be an error.

| Key | Type |
|---|---|
| `min` | As specified in HAP |
| `max` | Idem |
| `step` | Idem |
| `getTopic` | `string` |
| `setTopic` | `string` |

We expect that in order to get and set the value of a feature a
`"root topic"/<feature>/get` and `set` topic exist that we can use. If those
topics are named differently you have to specify a `getTopic` and a `setTopic`
key that have the full path to a topic (so not necessarily nested under the
"root" topic) that should be used instead.

[json-style]: https://google.github.io/styleguide/jsoncstyleguide.xml
[types]: https://github.com/hemtjanst/hemtjanst/blob/master/homekit/util/service.go
[characteristics]: https://github.com/hemtjanst/hemtjanst/blob/master/homekit/util/characteristic.go
