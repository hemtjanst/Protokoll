---
layout: default
title: v1
nav_order: 3
permalink: /v1
has_children: true
has_toc: false
search_exclude: true
---

# v1
{: .d-inline-block .no_toc }

Alpha
{: .label .label-yellow }

This is the documentation for upcoming v1 version.
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Introduction

Hemtjänst is a home automation ecosystem. It defines a vendor agnostic way
to describe devices and their capabilities and act on them. It leverages MQTT
and the pub/sub-pattern for all interactions in order to be able to create
user experiences that feel real-time. By using an MQTT broker as the central
message system, devices can join and leave the network at their leisure. Other
systems can leverage the data available on the broker to extend the system with
new capabilities.

Since many existing IoT solutions cannot natively interact with MQTT, a special
class of devices exist called bridges. The job of bridges is to expose devices
locked behind a vendor-specific protocol and make them available on the broker
using our vendor-agnostic representation. This ensures only the bridge needs to
hold any vendor-specific logic, essentially acting as a translation layer or
a proxy.

Though the MQTT broker acts as a central nervous system enabling communication
between all participants, there is no single system that "owns" devices. This
allows for an ecosystem of microservices that all interact through the broker
and avoids the need for some central monolith that must encapsulate all
logic. By agreeing on a device representation all components can act
independently of each other.

## Document Organisation

This specification is broken up in a number of different documents. A first
pair of documents focusses on metadata such as how to describe a device, its
capabilities and how to act on them:

* [Devices]({{ site.baseurl }}{% link v1/devices.md %})
* [Features]({{ site.baseurl }}{% link v1/features.md %})
* [Actions]({{ site.baseurl }}{% link v1/actions.md %})
* Groups
* Scenes
* Configuration

The second set of document(s) specify how providers can join the network
and make their devices available:

* [Announcement and Discovery]({{ site.baseurl }}{% link v1/discovery.md %})

The third pair of documents set forth a number of operational requirements:

* [Security and Privacy]({{ site.baseurl }}{% link v1/security.md %})
* [Multitenancy]({{ site.baseurl }}{% link v1/multitenancy.md %})

## Conventions and Terminology

The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT",
"SHOULD", "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in all
documents are to be interpreted as described in
[RFC 2119](https://tools.ietf.org/html/rfc2119).

Instead of inveting our own IoT metadata specification, Hemtjanst
leverages the Apple HomeKit Accessory Protocol specification. We borrow the
concepts of Profiles and Characteristics from it. When something is not
specified or is ambiguously specified by us, the HAP specification MUST be
considered leading/authoritative. Access to the HAP specification requires
an Apple ID but does not require a paid Apple Developer subscription.

Feature names are the *camelCased* version of the associated HAP
characteristic. For example, the "Current Relative Humidity" is represented
as `currentRelativeHumidity`.

JSON is used as the data exchange format. The naming of the keys in JSON
documents follows [Google's JSON style guide][json-style] and as
such are in *camelCase*. However, `ID` is always fully uppercase and any
chemical formula expressed in chemical symbols follows its relevant casing, so
`CO2`, not `Co2` for carbon dioxide.

The following terms are used:

**actor**
:   An entity connected to the broker for the purpose of interacting with
    existing devices. It can do so passively, only observing things,
    actively or both. It does not publish devices and is therefore not
    a provider

**bridge**
:   A piece of software or hardware that extends the system with new
    capabilities. A bridge can be an actor, a provider, or both

**broker**
:   A server implementing the MQTT protocol for asynchronous communication.
    The broker MUST implement MQTT version 3.1.1 and MAY support newer
    versions like MQTT 5 though its features are not used

**device**
:   An entity on the broker that represents a physical device in the real
    world. For example a lightbulb

**feature**
:   A capability of a device, like being able to turn it on/off. It is called
    a Characteristic in the HAP specification

**metadata**
:   A JSON document describing an entity on the broker. This will usually
    be a device description

**provider**
:   An entity connected to the broker that publishes devices. It does not
    act on other devices and is therefore not an actor

**publish**
:   The act of publishing a message to the broker

**subscribe**
:   The act of subscribing to a topic on the broker

**topic**
:   A location on the broker to publish the message to. It takes the form of
    a path, like `my/special/topic`

[json-style]: https://google.github.io/styleguide/jsoncstyleguide.xml
