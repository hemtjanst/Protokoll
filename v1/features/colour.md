---
layout: default
title: Colour
parent: Features
description: Features related to colour management
grand_parent: v1
---

# Colour
{: .no_toc}

These custom features relate to colour management. It is aimed at light
devices such as lightbulbs.
{: .fs-6 .fw-300 }

## Table of contents
{: .no_toc .text-delta}

1. TOC
{:toc}

## Why

Though the HAP specification provides us with the `hue` and `saturation`
charactersitics we chose to extend the spec with `colorX` and `colorY`
features instead. `hue` and `saturation` MUST NOT be used.

Hue and saturation are hardware dependent. The same hue and saturation
values on two different lightbulbs can result in distinctly different
colours being displayed. Since we are a vendor agnostic ecosystem it is
both conceivable and likely that people will mix-match hardware from
different vendors. As such we need a hardware agnostic way of specifying
colour so that it becomes possilbe to colour match.

The way to do this is by using the CIE xyY colour space. Many smart light
vendors support CIE xyY. Using `xy` is the preferred method for
configuring colour on both IKEA Trådfri and Philips Hue bulbs. `colorX` and
`colorY` attributes are also documented in the ZigBee specification (hence
just about everyone supports it).

CIE xyY allows us to specify a colour based on `x` and `y` coordinates. `Y` is
the tristimulus value (you can forget most of that). When specifying a
colour in `xy` it is then possible to pick the closest matching colour that
a device can display by overlaying its colour gamut on the CIE xyY
colour space. The colour gamut is specified by a triplet of coordinates, the
`x` and `y` values for Red, Green and Blue. Many vendors document the gamuts
their devices support or you can use a colorimeter or a spectrometer to
attempt to discover them yourself.

Leveraging the colour gamuts of the bulbs a request for a certain colour in
`xy` can be adjusted to a pair of coordinates that fall within the gamut of
all bulbs that you are targetting. A standard formula exists that can convert
CIE xyY to/from RGB facilitating colour input from electronic devices and
accurate colour representation on electronic visual displays.

## colorX
{: .d-inline-block }

Required
{: .label .label-red }

`colorX` specifies the `x` coordinate of a color in CIE xyY.

| Attribute | Value |
|---|---|
| `min` | 0.00 |
| `max` | 1.00 |
| `step` | 1 |
| `type` | float |

## colorY
{: .d-inline-block }

Required
{: .label .label-red }

`colorY` specifies the `y` coordinate of a color in CIE xyY.

| Attribute | Value |
|---|---|
| `min` | 0.00 |
| `max` | 1.00 |
| `step` | 1 |
| `type` | float |

## CIE xy chromaticity diagram

![CIE xy chromaticity](https://user-images.githubusercontent.com/99779/56497878-78823180-6553-11e9-997e-3c35da78654a.png)
