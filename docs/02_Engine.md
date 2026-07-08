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

# 13. EQ Engine

The EQ Engine consumes Level Engine output.

The EQ Engine never modifies the Level Engine.

The EQ Engine is read-only with respect to Levels.

The EQ Engine produces EQ output for Strategy and Renderer modules.

---

## EQ Definition

EQ is not a Level Type.

EQ is a Confluence.

EQ is created from existing Levels.

EQ never becomes part of the Level Engine.

EQ must have its own data model.

There must not be a LEVEL_EQ.

EQ must not be stored inside allLevels.

---

## EQ Creation Rules

An EQ may be created only when every participating source Level satisfies:

- Valid == true
- Fresh == true
- Same Direction
- Same Price
- Allowed Level Type
- Allowed Timeframe

Price comparison is exact.

No tolerance is used.

EQ price = source Level price.

Since all source Levels must have the same price, no averaging is required.

---

## Timeframe Design

The EQ Engine must support configurable timeframe combinations.

Timeframes must never be hardcoded.

Future examples include:

- H1 + M30
- H4 + H1
- H4 + H1 + M30

The architecture must support additional timeframes in future milestones.

---

## Current Strategy Configuration

The current trading strategy uses:

Primary Timeframe

- H1 Classic

Secondary Timeframe

- M30 Classic
- M30 GAP

This is a Strategy configuration only.

It must not be hardcoded into the EQ Engine.

---

## EQ Fresh

Every newly created EQ starts as Fresh.

EQ Fresh means the EQ price has not been touched after EQ creation.

Touch is defined as:

High >= EQ Price >= Low

If price touches the EQ price after EQ creation:

Fresh = false

A non-Fresh EQ is no longer eligible for new trade setup generation.

A non-Fresh EQ may still be rendered as historical context or order reference.

Rendering of non-Fresh EQs is a Renderer policy, not EQ Engine logic.

---

## EQ Source Snapshot

EQ is a snapshot of market confluence.

Once an EQ is created:

- Source Level IDs never change.
- Source Level Types never change.
- Source Timeframes never change.

EQ never replaces one source Level with another.

Example:

H1 Classic + M30 Classic

never becomes

H1 Classic + M30 BO

If later market structure creates:

H1 BO + M30 BO

that is a completely new EQ.

The original EQ remains an independent snapshot.

---

## EQ Future Evolution

If source Levels later evolve into new Level Types:

Classic -> BO

BO -> HNS

those new Levels may participate in creating future EQs.

Existing EQs never update their source Levels.

---

## Multiple EQs

Multiple EQs are allowed.

Duplicate EQ filtering is reserved for a future milestone.

If multiple valid confluences exist, every valid EQ may be created.

Multiple EQs at the same price are allowed for now.

This may indicate stronger confluence and will be studied later.

---

## EQ Boundaries

The EQ Engine never manages:

- Orders
- Pending Orders
- Positions
- Risk
- Trade Management

The Strategy decides:

- Whether to trade an EQ
- Whether to request orders
- Whether to request order cancellation
- Whether to request trade management actions
- Whether to use Fresh EQ only
- Whether to reference historical non-Fresh EQs

Execution owns order and position lifecycle.

Execution executes trade plans produced by Strategy.

Execution must not modify Engine state.

Renderer behavior will be defined in a later milestone.

Fresh EQs may be rendered normally.

Non-Fresh EQs may optionally be rendered differently as historical context or order reference.

This is Renderer policy, not EQ Engine logic.

---

# 14. Design Principles

Fresh and Valid are independent lifecycle states.

A Level may remain Valid after it is no longer Fresh.

When a Level evolves into a new market structure, the source Level becomes invalid.

Existing Levels never change identity or Level Type.

Allowed lifecycle state changes:

- Fresh
- Valid

Fresh, Valid, boCreated, and hnsCreated do not change a Level's identity.

The Level Engine owns market structure.

The EQ Engine owns confluence.

The Strategy owns trading decisions.

Execution owns order and position lifecycle.

The Renderer owns visualization.

The EQ Engine is read-only.

The EQ Engine never modifies:

- Levels
- Level Fresh
- Level Valid
- Level lifecycle
- Market structure

The EQ Engine only consumes Level Engine output and produces EQ output.

EQ represents a snapshot of market confluence.

Existing EQs never modify their source Levels.

Existing EQs never replace their source Levels.
