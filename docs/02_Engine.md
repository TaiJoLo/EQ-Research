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

When market structure evolves, the Engine creates a new Level instead of modifying an existing Level's identity or Level Type.

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

Close of the first Bear candle.

Direction

Support

The Level is confirmed only after the second Bull candle closes.

---

## Resistance

Pattern

Bull Candle

↓

Bear Candle

Level Price

Close of the first Bull candle.

Direction

Resistance

The Level is confirmed only after the second Bear candle closes.

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

Close of the first Bull candle.

Direction

Support

The Level is confirmed only after the second Bull candle closes.

---

## Resistance

Pattern

Bear Candle

↓

Bear Candle

Level Price

Close of the first Bear candle.

Direction

Resistance

The Level is confirmed only after the second Bear candle closes.

---

# 6. Fresh

Every newly created Level starts as Fresh.

Fresh means a Level has not been touched after its creation candle.

The creation candle of every newly created Level is ignored.

Any touch after the creation candle immediately removes Fresh.

Fresh applies to every Level Type.

Fresh does not control evolution.

A non-Fresh Classic Level may still create a BO Level.

A non-Fresh BO Level may still create an HNS Level.

---

# 7. Level Events

Fresh, Valid, and Evolution are separate concepts.

Touch controls Fresh.

Break controls structural evolution.

Valid controls whether the source Level is still a current market structure.

Renderer displays only Levels where:

valid && fresh

## Touch

Touch is an intrabar event.

Touch is defined as:

High >= Level >= Low

If any Level is touched after its creation candle:

fresh = false

## Break

Break is a close event.

Support Break

Close < Level Price

Resistance Break

Close > Level Price

Break is evaluated only after the candle closes.

Break creates new market structure when an evolution rule exists.

Break does not mutate Level identity or Level Type.

## Reject

Reject is a close event.

Reject occurs when:

- touched == true
- broken == false

If a Level is rejected:

- No new Level is created.
- The source Level remains valid.
- Fresh is false because the Level was touched.

## State Effects

If any Level is touched after its creation candle:

fresh = false

If a Classic Level is broken:

- Create a BO Level.
- Set the source Classic Level valid = false.
- The source Classic keeps its original ID, price, direction, and Level Type.

If a BO Level is broken:

- Create an HNS Level.
- Set the source BO Level valid = false.
- The source BO keeps its original ID, price, direction, and Level Type.

HNS is terminal.

HNS does not create another Level Type.

If an HNS Level is touched after its creation candle:

fresh = false

---

# 8. Break

A Break occurs when price closes beyond a Level.

Support

Close below Support.

Resistance

Close above Resistance.

Break is evaluated only after the candle closes.

Only a confirmed candle close may create a new market structure.

Break creates new market structure when an evolution rule exists.

Break does not mutate Level identity or Level Type.

---

# 9. BO Level

A BO Level is created when a Classic Level is broken.

The source Classic Level does not need to be Fresh to create a BO Level.

The source Classic Level becomes valid = false.

The source Classic keeps its original ID, price, direction, and Level Type.

The BO Level is a newly created Level.

BO Level properties

- Same Price
- Opposite Direction
- Level Type = LEVEL_BO

Every Classic Level may create at most one BO Level.

Every BO Level starts as Fresh.

---

# 10. HNS Level

An HNS Level is created when a BO Level is broken.

The source BO Level does not need to be Fresh to create an HNS Level.

The source BO Level becomes valid = false.

The source BO keeps its original ID, price, direction, and Level Type.

The HNS Level is a newly created Level.

Every BO Level may create at most one HNS Level.

Every HNS Level starts as Fresh.

Break is confirmed only after the candle closes.

HNS is terminal.

HNS does not create any further Level Type.

---

## Support HNS

Source Level

Resistance BO

Break

Close above the BO Level.

HNS Level properties

- Same Price
- Direction = Support
- Level Type = LEVEL_HNS

---

## Resistance HNS

Source Level

Support BO

Break

Close below the BO Level.

HNS Level properties

- Same Price
- Direction = Resistance
- Level Type = LEVEL_HNS

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
- Valid
- boCreated
- hnsCreated

boCreated and hnsCreated are internal Engine state.

---

# 13. Design Principles

Fresh and Valid are independent lifecycle states.

A Level may remain Valid after it is no longer Fresh.

When a Level evolves into a new market structure, the source Level becomes invalid.

Existing Levels never change identity or Level Type.

Allowed lifecycle state changes:

- Fresh
- Valid

Fresh, Valid, boCreated, and hnsCreated do not change a Level's identity.
