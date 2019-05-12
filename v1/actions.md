---
layout: default
title: Actions
parent: v1
nav_order: 5
---

# Actions
{: .no_toc }

The actions specification details how a device's features can be acted
on.
{: .fs-6 .fw-300 }

Many features support being changed. For example the brightness of a lightbulb
can be adjusted, or the colour, or the volume of a speaker. To do so topics can
be constructed by combining the `root` attribute of a device with the feature
name to allow toggling its value.

Two topics need to be available in order to be able to complete a request-response
flow.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Current state

The `current` topic represents the `current` value or state of this feature.

Actors MUST NOT publish to a `current` topic. An ACL MAY be put in place to
enforce this. The `current` topic is read-only for an actor. Actors SHOULD
subscribe to the `current` topic if they wish to know the current state
and/or be notified of completed state changes. They SHOULD do so before
initiating a state change in order to be able to determine if the state change
happened.

Providers MUST publish to the `current` topic whenever the state of one of
their features changes. Providers MUST ignore any changes to the `current`
topic and therefore SHOULD NOT subscribe to it. The `current` topic is
write-only for a provider. An ACL MAY be put in place to enforce this.

## Desired state

The `desired` topic is used to initiate a transition to a new state.

Actors MAY subscribe to the `desired` topic if they wish to become aware of
other actors initiating state transitions. The `desired` topic is considered
read-write for an actor though in most cases an actor SHOULD limit itself
to treating the `desired` topic as write-only. By subscribing to the `current`
topic it will already be aware of succesful state changes.

Providers MUST subscribe to the `desired` topics of all their features unless
the feature is read-only. Providers MUST NOT publish to the `desired` topics.
The `desired topic is read-only for a provider and an ACL MAY be put in place
to enforce this.

## Changing state

When publishing to a `desired` topic the value MUST be encoded as a `string`,
so `"255"` not `255` as a byte. Anyone consuming the value from the `desired`
topic MUST use the `type` specified for this feature when converting it to
a native type.

The boolean value `true` is encoded as `"1"`, `false` as `"0"`. Any other value
is considered invalid and MUST not trigger a state change.

## Read-only features

When a feature has the `readOnly` attribute set to `true` only the `current`
topic will be available.

Publishing to the `desired` topic has no effect. It is like screaming into a
void. The actor SHOULD take note of the `readOnly` attribute of the device
and act accordingly.

An ACL MAY be put in place preventing the act of publishing to the `desired`
topic for a read-only feature. The actor MUST be able to handle this
gracefully.

## Building the topic name

In order to make applying ACLs simpler `current` and `desired` are **prefixed**
to the `root` attribute, not appended to the feature name.

* Let the device `root` attribute be: `<owner>/lightbulb/1`
* Let the feature we want to control be: `on`

This means that:

* `current`: `current/<owner>/lightbulb/1/on`
* `desired`: `desired/<owner>/lightbulb/1/on`

See the [Devices specification]({{ site.baseurl }}{% link v1/devices.md %}#root)
for what the `<owner>` value can be.
