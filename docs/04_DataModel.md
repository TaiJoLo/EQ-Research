# Data Model

Version: 0.2

---

# Philosophy

The Data Model represents market structure only.

It must never contain TradingView drawing objects or Strategy information.

Rendering and trading decisions belong to separate modules.

---

# Level

| Field | Type | Description |

|---------|------|-------------|

| price | float | Level price |

| direction | int | SUPPORT / RESISTANCE |

| creationBar | int | Bar index where the Level is confirmed |

| levelType | string | Classic / Gap / BO / HNS |

| fresh | bool | Whether the Level has never been tested |

| active | bool | Whether the Level should currently be used by downstream modules |

| valid | bool | Whether the Level is still structurally valid |

| id | int | Unique identifier |

---

# Field Groups

## Core Market Data

price

direction

creationBar

These fields define the existence of a Level.

Without them, a Level does not exist.

---

## State

levelType

fresh

active

valid

These fields describe the current lifecycle of the Level.

---

## Runtime Metadata

id

Used only for tracking the Level internally.

---

# Important Principles

A Level never stores TradingView objects.

A Level never stores Strategy information.

A Level never stores Research results.

The Data Model represents market structure only.