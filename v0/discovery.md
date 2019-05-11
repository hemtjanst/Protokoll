---
layout: default
title: Service Discovery
parent: v0
nav_order: 2
---

# Service Discovery
{: .no_toc }

In order to not have to continuously scan for new devices, or to subscribe
to every event, a discovery mechanism has to be implemented.
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc }

1. TOC
{:toc}

## Discover

A `discover` topic with retain set must exist. Any device that joins the
network must subscribe to `discover`. Since the retain bit is set they will
receive the discovery request and must now announce themselves to the rest
of the network. If someone then wants to initiate a full discovery all they
need to do is publish again to discover (with the retain bit set).

## Announce

An announce is done by publishing to the `announce/YOUR NAME` topic with persistence.
The body of the message must be the meta content for the entity that joined.
The topic layout is entirely arbitrary and so is the naming of the device.
Anyone interested in knowing about devices now simply subscribes to `announce/#`
and gets to know any device that joins.

**Please note**, the topic of the device is used to generate a unique identifier for
this device, so if you change it it'll be like you removed the existing device and
added a new one.

Whenever a device leaves it publishes its "root" topic to the `leave` topic.
Similarly to announce this allows other clients to cancel their subscriptions
if they were explicitly wathcing that device or take any other ation.

## Last Will and Testament

MQTT includes a Last Will And Testament feature that will cause the broker to
publish a message on a specific topic when the client is gone, gracefully or
not.

If every device sets up its own MQTT client it can hence specify that as a
last will and testament the broker should publish to `leave` with its original
topic. This ensures that we can always properly clean up, even if the device
falls of a cliff.

However, for bridged clients, lets say IKEA Trådfri lights that are published
through a trådfri-to-mqtt broker, the bridge is the only MQTT client. Since we
can't specify multiple actions in a last will and testament the bridge can
still tell the broker to publish to a message to the topic, the content being
a string representing a "leave ID". Every device that the bridge used to bridge
must specify that same "leave ID" in its metadata so that a mapping can be
maintained between "leave ID"s and devices for any cleanup purposes, such as no
longer announcing this accessory to HomeKit.
