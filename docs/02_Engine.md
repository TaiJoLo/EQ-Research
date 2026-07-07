# EQ Engine Specification

Version: 1.0

---

# 1. Purpose

The Engine is responsible for identifying every valid market Level.

The Engine never decides whether a Level should be traded.

Trading decisions belong to the Strategy module.

---

# 2. Level Types

The Engine defines four Level Types.

- Classic
- GAP
- BO
- HNS

Each Level Type is independent.

A Level never changes its identity after creation.

When market structure evolves, the Engine creates a new Level instead of modifying an existing one.

---

# 3. Candle Direction

Bull Candle

Close > Open

Bear Candle

Close < Open

Doji

Close == Open

A Doji is neither Bull nor Bear.

---

# 4. Classic Level

A Classic Level is formed by two adjacent candles of opposite direction.

A Doji cannot form a Classic Level.

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

# 5. GAP Level

A GAP Level is formed by two adjacent candles of the same direction.

A Doji cannot form a GAP Level.

## Support

Pattern

Bull Candle

↓

Bull Candle

Level Price

Close of the first (Bull) candle.

Direction

Support

The Level is confirmed only after the second (Bull) candle closes.

---

## Resistance

Pattern

Bear Candle

↓

Bear Candle

Level Price

Close of the first (Bear) candle.

Direction

Resistance

The Level is confirmed only after the second (Bear) candle closes.

---

# 6. Fresh

Every newly created Level starts as Fresh.

Fresh remains true until the first touch after the creation candle.

Touch is defined as:

High >= Level >= Low

Any touch removes Fresh.

The creation candle of every newly created Level is ignored.

Fresh applies to every Level Type.

---

# 7. Break

A Break occurs when price closes beyond a Level.

Support

Close below Support.

Resistance

Close above Resistance.

Break is evaluated only after the candle closes.

Only a confirmed candle close may create a new market structure.

---

# 8. BO Level

A BO Level is created when a Classic Level is broken.

The original Classic Level remains unchanged.

The BO Level is a new Level.

BO Level properties

- Same Price
- Opposite Direction
- Level Type = LEVEL_BO

Every Classic Level may create at most one BO Level.

Every BO Level starts as Fresh.

---

# 9. HNS Level

An HNS Level is created when a BO Level is broken.

The original BO Level remains unchanged.

The HNS Level is a new Level.

HNS Level properties

- Same Price
- Opposite Direction
- Level Type = LEVEL_HNS

Every BO Level may create at most one HNS Level.

Every HNS Level starts as Fresh.

---

# 10. Invalid Levels

The invalidation rules for HNS and future Level Types will be defined in a later version.

---

# 11. Duplicate Levels

Duplicate Level handling is reserved for a future milestone.

The current Engine allows multiple Levels to exist at the same price.

---

# 12. Engine Output

Every Level contains:

- ID
- Creation Time
- Timeframe
- Price
- Direction
- Level Type
- Fresh
- Active
- Valid
- boCreated

Additional lifecycle fields may be introduced in future versions without changing the identity of a Level.

---

# 13. Processing Order

For every confirmed candle close, the Engine executes in the following order:

1. Update Lifecycle

2. Detect Market Levels

   - Classic
   - GAP
   - BO
   - HNS

3. Render Levels

Future milestones may extend the detection stage without restructuring the execution pipeline.

---

# 14. Engine Principles

The Engine only describes market structure.

The Engine never generates trading signals.

Every valid Level exists independently of any Strategy.

The Engine owns market structure.

The Strategy consumes Engine output.

The Renderer visualizes Engine output only.

The Engine never consumes Strategy output.

---

# 15. Design Principles

Every new market structure is represented by creating a new Level.

Existing Levels never change their identity.

Only lifecycle state may change.

Allowed lifecycle state changes:

- Fresh
- Active
- Valid

Lifecycle state changes never modify the identity of a Level.

Immutable Level properties:

- ID
- Creation Time
- Timeframe
- Price
- Direction
- Level Type