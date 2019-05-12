---
layout: default
title: Power
parent: Features
description: Features related to power usage
grand_parent: v1
---

# Power
{: .no_toc}

These custom features relate to power and power management. They are meant
for devices such as smart sockets/plugs.
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta}

1. TOC
{:toc}

## currentAmpere
{: .d-inline-block }

Read-only
{: .label .label-blue }

The current power draw in Amperes.

| Attribute | Value |
|---|---|
| `min` | 0 |
| `max` | 255 |
| `step` | 1 |
| `type` | float |
| `unit` | Ampere |

## currentVoltage
{: .d-inline-block }

Read-only
{: .label .label-blue }

The current power draw in Volts.

| Attribute | Value |
|---|---|
| `min` | 0 |
| `max` | 10000 |
| `step` | 1 |
| `type` | float |
| `unit` | Volt |

## currentPower
{: .d-inline-block }

Required
{: .label .label-red }

Read-only
{: .label .label-blue }

The current power draw in Watts.

| Attribute | Value |
|---|---|
| `min` | 0 |
| `max` | 10000 |
| `step` | 1 |
| `type` | float |
| `unit` | Watt |

## totalEnergy
{: .d-inline-block }

Read-only
{: .label .label-blue }

Total energy usage so far, in kilowatt hour. This value can reset to zero if
the device is reset but otherwise must only increment.

| Attribute | Value |
|---|---|
| `min` | 0 |
| `max` | 1.7976931348623157 × e^308 |
| `step` | 1 |
| `type` | float |
| `unit` | Kilowatt hour |
