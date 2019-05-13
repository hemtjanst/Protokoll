---
layout: default
title: Features
parent: v1
nav_order: 2
has_children: true
has_toc: false
---

# Features
{: .no_toc }

The feature specification details how capabilities of a device are
represented.
{: .fs-6 .fw-300 }

Features is a map of `string` keys to `map`s representing the feature's
attributes. Most features are a direct mapping of the characteristics in
the HAP specification. Custom features have been created in places where
the HAP specification fell short.

With regards to color management the HAP specificatin MUST be ignored and
instead support for the custom
[Colour feature]({{ site.baseurl }}{% link v1/features/colour.md %})
MUST be implemented.

## Table of contents
{: .no_toc .text-delta }

1. TOC
{:toc}

## Attributes

A feature has a number of possible attributes. The following come directly
from the HAP specification and are all OPTIONAL:

| Key | Read-only | Description|
|---|---|
| `min` || Minimum value |
| `max` || Maximum value |
| `step` || Increment |
| `type` |X| `uint8`, `float64`, `string` etc |
| `unit` |X| Celsius, percentage etc. |

It's important to note that though `min`, `max` and `step` can vary for two
devices with the same feature, `type` and `unit` cannot. The values of
`type` and `unit` are mandated by the HAP specification or for custom
features by this specification.

One additional key is added by us:

| Key | Read-only | Description|
|---|---|
| `readOnly` | X | A boolean, `true` or `false` |

This attribute specifies that this feature cannot be updated and is
important for [Actions]({{ site.baseurl }}{% link v1/actions.md %}).

### Min

This is the minimum support value for this feature. It is the lower, inclusive,
bound of a range of values.

### Max

This is the maximum support value for this feature. It is the upper, inclusive,
bound of a range of values.

### Step

This is primarily used for UI components to generate sliders with a certain
amount of steps.

### Type

The type that will be used to deserialise this value. For example if the type
is specified as a `uint8` the value must be between 0 and 255 or it will
wrap. The `min` and `max` attributes an be used to constrain the range of valid
values to a subset of what is allowed by the type.

### Unit

A unit of measurement. Likely an SI (derived) unit. Can be helpful in UIs.

## Considerations for describing features

When a key is ommitted it MUST be interpreted as if it were explicitly set
to the value defined in the HAP specification.

When publishing a message it is RECOMMENDED that all keys are explicitly
set, even if it is to their defaults. This ensures the receiver of the
message does not need to know what the defaults are in the HAP specification.

It is important to note that when a feature is present you MUST set it to the
empty object, or specify at least one of the keys. If a feature is omitted or
set to `null` it MUST be interpreted as the feature being absent.

## Examples

The "on" feature can be described by an empty map:

```json
"on": {}
```

This means that the `min`, `max`, `step`, `type` and `unit` all default to the
values as defined in the HAP specification.

A more complex feature, like "brightness" can look like this:

```json
"brightness": {
    "min": 10
}
```

This specifies that the brightness is in the range of `10`-`255` because we are
overriding the HAP specification default for the `min` value, which is `0`.

Multiple features are represented through the `features` map:

```json
{
    "features": {
        "on": {},
        "brightness": {}
    }
}
```

## Custom features

| Feature | Description |
|---|---|
{%- assign pages_list = site.html_pages | sort:"nav_order" -%}
{%- for page in pages_list -%}
{%- if page.url contains "/v1/features/" -%}
{%- if page.url != "/v1/features/" %}
| [{{ page.title }}]({{ site.baseurl }}{{ page.url }}) | {{ page.description }} |
{%- endif -%}
{%- endif -%}
{%- endfor -%}
