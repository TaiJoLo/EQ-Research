# Glossary

Version: 0.1

---

# Purpose

This document defines all terminology used throughout the EQ Research project.

Every document, Pine Script, Python module and strategy should follow these definitions.

---

# Level

A horizontal price level created by the Engine.

Every level has:

- Price
- Direction
- Type
- State
- Fresh status

---

# Direction

A level always has one direction.

Support

A price level expected to reject downward movement.

Resistance

A price level expected to reject upward movement.

---

# Classic

A level created by two consecutive candles with opposite directions.

Support

Bear Candle

↓

Bull Candle

Level Price = First Candle Close

Resistance

Bull Candle

↓

Bear Candle

Level Price = First Candle Close

---



# GAP

A level created by two consecutive candles with the same direction.

Support

Bull Candle

↓

Bull Candle

Level Price = First Candle Close

Resistance

Bear Candle

↓

Bear Candle

Level Price = First Candle Close

---



# BO (Breakout Level)

A Classic level after a valid Break.

The price never changes.

Only the direction changes.

Support → Resistance

Resistance → Support

Only Classic can become BO.

---



# HNS

A BO level after another valid Break.

The price never changes.

Only the state changes.

If HNS breaks again,

the level becomes Invalid.

---



# Fresh

A level that has never been touched since it was created.

Touch immediately removes Fresh.

Fresh status does not depend on candle close.

---



# Touch

Price touches a level when:

High >= Level >= Low

Any touch counts.

It is not necessary for the candle to close at the level.

---



# Reject

Price touches a level,

but the candle closes back on the original side.

Support

Touch

Close above

Resistance

Touch

Close below

Reject removes Fresh.

Reject does not change level type.

---



# Break

A candle closes beyond the level.

Support

Close below Support

Resistance

Close above Resistance

Break changes the level state.

---



# Invalid

A level ignored by the Engine.

Example:

HNS breaks again.

---



# State

The lifecycle of a level.

Classic

↓

BO

↓

HNS

↓

Invalid

The price never changes during the lifecycle.

Only the state changes.

---



# EQ (Equilibrium)

A multi-timeframe confluence level.

An EQ exists when:

- The selected timeframe contains an allowed level type.
- The lower timeframe contains an allowed level type.
- Both levels have exactly the same price.
- Both levels are Fresh.

The Engine detects every valid EQ.

The Strategy decides whether an EQ should be traded.

---



# Timeframe Pair

Examples:

H4 + H1

H1 + M30

M30 + M15

The Engine allows different timeframe pairs.

The Strategy chooses which pair to trade.

---



# Driver

(Research Concept)

A higher-timeframe EQ that appears to initiate price movement.

Currently not part of the Engine.

Currently not part of the Strategy.

This concept will be researched in future versions.

---



# Strategy

A set of trading rules.

Examples:

- Entry
- Stop Loss
- Take Profit
- Re-entry
- Pending Orders

Strategy is independent from the Engine.

---



# Engine

The Engine identifies market structure.

The Engine never makes trading decisions.

The Engine only outputs objective market information.

---



# Research Parameter

A configurable option used for testing.

Examples:

- Allowed Level Types
- Timeframe Pairs
- Stop Loss Size
- Take Profit Size
- Pending Order Expiration
- Trading Sessions

Research Parameters must never modify the Engine definitions.