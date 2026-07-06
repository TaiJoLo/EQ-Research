# EQ Engine Specification

Version: 0.1

---

# 1. Purpose

The Engine is responsible for identifying every valid market Level.

The Engine never decides whether a Level should be traded.

Trading decisions belong to the Strategy module.

---

# 2. Level Types

The Engine currently supports four Level Types.

- Classic
- GAP
- BO
- HNS

Classic Levels evolve through the following lifecycle:

Classic

↓

BO

↓

HNS

↓

Invalid

GAP Levels currently do not evolve.

---

# 3. Classic Level

A Classic Level is formed by **two adjacent candles of opposite direction**.

A Doji (Close == Open) is ignored and cannot form a Classic Level.

## Candle Direction

Bull Candle

Close > Open

Bear Candle

Close < Open

Doji

Close == Open

Doji is ignored.

## Support

Pattern

Bear Candle

↓

Bull Candle

Level Price

Close of the first (Bear) candle.

Direction

Support

The Level is confirmed only after the second (Bull) candle closes.

---



## Resistance

Pattern

Bull Candle

↓

Bear Candle

Level Price

Close of the first (Bull) candle.

Direction

Resistance

The Level is confirmed only after the second (Bear) candle closes.

---



# 4. GAP Level

Reserved.

The GAP algorithm will be defined in a future version.

---



# 5. Fresh

A Level is Fresh immediately after creation.

Only candles after the Level is created can remove Fresh.

Touch means

High >= Level >= Low

Any touch removes Fresh.

The candle that creates the Level is ignored.

The candle that creates a BO is also ignored.

---



# 6. Reject

Reject means

Price touches the Level

AND

The candle closes back on the original side.

Example

Support

Low touches Support

Close above Support

Result

Reject

Reject removes Fresh.

Reject does not change the Level Type.

---



# 7. Break

Break means

Price closes beyond the Level.

Support

Close below Support.

Resistance

Close above Resistance.

Break is evaluated only after the candle closes.

Only a confirmed candle close can create a Break.

---



# 8. BO

Only Classic Levels can become BO.

When a Classic Level is Broken

Classic

↓

BO

The Level price never changes.

After a Break, the Level changes its market role.

Support becomes Resistance.

Resistance becomes Support.

BO starts as Fresh.

---



# 9. HNS

HNS is formed after a BO Level is Broken again.

Example

Classic Support

↓

BO Resistance

↓

Break Again

↓

HNS Support

The Level price never changes.

The Level Type changes to HNS.

HNS starts as Fresh.

---



# 10. HNS Expiration

If an HNS Level is Broken again

↓

The Level becomes Invalid.

Invalid Levels are ignored permanently.

---



# 11. Duplicate Levels

If multiple Levels of the same Level Type exist at exactly the same price,

only the newest Level remains Active.

Older Levels become Inactive and are ignored by the Engine.

Historical information may still be retained internally for future research.

---



# 12. Engine Output

Each Level contains

- ID
- Creation Time
- Timeframe
- Price
- Direction
- Level Type
- Fresh
- Valid

---



# 13. Processing Order

For every confirmed candle close, the Engine executes in the following order:

1. Detect newly created Levels.
2. Update existing Levels.
3. Process Reject events.
4. Process Break events.
5. Update Level Types.
6. Remove Invalid Levels.

---



# 14. Engine Principles

The Engine only describes market structure.

The Engine never creates trading signals.

Every valid Level must exist independently of any Strategy.

Every valid EQ must exist independently of any Strategy.

The Strategy Engine consumes Engine output.

The Engine never consumes Strategy output.