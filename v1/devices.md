---
layout: default
title: Devices
parent: v1
nav_order: 1
---

# Devices
{: .no_toc }

The device specification details how a device and its capabilities are
represented.
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Attributes

A device is describe by a number of attributes.

| Key | Required | Type |
|---|---|---|
| `name` | X | `string` |
| `type` | X | `string` |
| `features` | X | `object` |
| `root` | X | `string` |
| `lastWillID` || `UUID` |

### Name

The `name` is a cosmetic attribute and is meant to be used as the name for this
device in UIs.

### Type

The type of the device helps classify devices. The `type` can be something
like `sensor`, or `light` and is meant to be generic, i.e it MUST NOT specify
what kind of sensor or light it is.

### Features

Features are described
[in their own document]({{ site.baseurl }}{% link v1/features.md %}).

### Root

The `root` represents the anchor topic of this device. Anything underneath
the root belongs to this device. It is used to construct topics representing
features and the actions that can be taken on them.

Because providers MUST
[support username/password authentication]({{ site.baseurl }}{% link v1/security.md %})
it is RECOMMENDED that the `root` topic is prefixed with the `username`. This
makes it clear who "owns" the device and enables applying more strict ACLs.

### Last Will ID

The purpose of this attribute is explained in the [Announcement & Discovery
document]({{ site.baseurl }}{% link v1/discovery.md %}). It is OPTIONAL but
MUST be present for any device provided by a bridge, i.e a device that does
not maintain its own connection to the broker.

When this value is present it MUST be unique per bridge and MUST be unique
on the broker. It MUST be the same for all devices belonging to the same
bridge and it SHOULD be a UUIDv4.

## Considerations for describing devices

If a device has no `features`, it MUST NOT be published. There is no point to
having a device without it exposing some capability.

The `name` attribute SHOULD be human friendly and SHOULD be unique for the same
`type`. If five lightbulbs all have the same `name` this will only lead to
confusion. But if a window and a lightbulb have the same `name` a UI can make
the difference apparent through text, hierarchical organisation or grouping of
devices or through the use of different icons.

## Example

A very basic metadata document representing a device can look like this:

```json
{
    "name": "Kitchen",
    "type": "light",
    "features": {
        "on": {},
        "brightness": {}
    },
    "root": "lightbulb/kitchen"
}
```
