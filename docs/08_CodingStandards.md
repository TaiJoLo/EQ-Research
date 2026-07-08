# Coding Standards

## Data Model

Generic object name:

Level

Never:

ClassicLevel

GapLevel

BOLevel

---

## Function Naming

detectXXX()

createXXX()

updateXXX()

renderXXX()

Never mix responsibilities.

---

## Arrays

Store every Level in

allLevels

Never create one array for each Level Type.

Never store EQ objects inside allLevels.

EQ must have its own data model and storage.

---

## Constants

Always use named constants.

Never use magic numbers.

---

## Level Engine Internal Processing Pipeline

Detect Levels

↓

Create Levels

↓

Update Lifecycle

This processing pipeline belongs only to the Level Engine.

It is independent from the overall system architecture.

## System Architecture

Market Data

↓

Level Engine

↓

EQ Engine

↓

Strategy

↓

Execution

↓

Renderer

## Pine Script Safety Rules

Always check array size before iterating.  

Never call array.get() on an empty array.  

Never modify global variables inside functions.  

Every script must compile and execute without runtime errors.

## Data Model

Fields should be ordered by domain importance.  
Do not reorder existing fields unless absolutely necessary.

1. Core Market Data
2. State
3. Runtime Objects

## Level Identity

A Level never changes its identity after creation.

Classic Levels never become BO Levels.

Gap Levels never become Classic Levels.

New market structures must create new Levels instead of modifying existing ones.

A Level's `levelType` is immutable.

There must not be a LEVEL_EQ.

EQ is not a Level Type.

A Level may change its lifecycle state, but never its identity.

Allowed:
- fresh
- valid
- boCreated
- hnsCreated

Immutable:
- id
- price
- direction
- levelType
- creationBar

## Generic Level Creation

All new Levels must be created through the generic createLevel() function.

createLevel() must receive:

- id
- price
- direction
- levelType
- creationBar

Never create:

- createClassicLevel()
- createBOLevel()
- createGapLevel()
- createHNSLevel()

---

## Generic Creation Interface

New Levels must be created only through createLevel().

Never instantiate Level objects directly.

Never call:

Level.new()

outside the Creation module.

---

## Generic Detector Interface

Every Detector must return:

(
    detected,
    price,
    direction,
    levelType
)

Do not extend the Detector interface.

Additional information belongs to Lifecycle or Strategy.



## Market Structure Independence

Market Structure detection must never depend on lifecycle state.

Examples:

- BO detection must not depend on fresh.
- Gap detection must not depend on fresh.
- HNS detection must not depend on fresh.

Lifecycle state and Market Structure are independent concepts.

## EQ Engine Rules

The EQ Engine consumes Level Engine output only.

The EQ Engine must never modify:

- Levels
- Level Fresh
- Level Valid
- Level lifecycle
- Market structure

EQ creation may only use source Levels where:

- valid == true
- fresh == true
- Same direction
- Same price
- Allowed Level Type
- Allowed Timeframe

EQ price comparison is exact.

No tolerance is used.

EQ price must equal the source Level price.

EQ timeframe combinations must be configurable.

Timeframes must never be hardcoded inside the EQ Engine.

Strategy-specific combinations belong to Strategy configuration.

EQ must be a snapshot.

Existing EQs must never replace or update their source Level IDs, source Level Types, or source Timeframes.

Duplicate EQ filtering is reserved for a future milestone.

Multiple EQs at the same price are allowed for now.

EQ v1 only requires Fresh.

EQ v1 does not require Valid.

EQ must never manage orders, positions, risk, entries, exits, or trade management.

## Trading Research Framework Philosophy

The purpose of this project is not to build a single trading strategy.

The purpose is to build a reusable trading research framework capable of evaluating and improving trading ideas through systematic research.

The Level Engine describes market structure.

The EQ Engine describes market confluence.

The Strategy expresses trading hypotheses.

Execution executes trade plans.

Every trading hypothesis should be represented as an independent configurable module.

Every configurable behavior should be exposed through parameters instead of hardcoded logic whenever practical.

The core Engine should remain stable.

Research should be performed by composing, enabling, disabling, or configuring independent Strategy modules.

New research ideas should not require modification of the Engine whenever possible.

## Research Philosophy

Every research question should be answerable by configuration instead of code modification whenever practical.

Examples:

Instead of changing code to answer:

"Should TP be fixed distance or next H1 Level?"

The framework should allow Target Policy configuration:

- Fixed Distance
- Structure Target

Instead of changing code to answer:

"Should partial TP be used?"

The framework should allow Position Management Policy configuration:

- Full Exit
- Partial Exit

Instead of changing code to answer:

"Should reverse trades be enabled?"

The framework should allow Reverse Policy configuration:

- Enabled
- Disabled

Every trading hypothesis should become an independent Policy with configurable Parameters.

## Modular Strategy Design

The Strategy is composed of independent Policies.

Examples:

- Entry Policy
- Stop Policy
- Target Policy
- Position Management Policy
- Reverse Policy
- Re-entry Policy
- Risk Policy
- Session Policy
- Trend Policy
- News Policy
- Time Policy

Future Policies may be added without modifying existing Policies.

Each Policy should contain:

- Rules
- Parameters

Policies should be independently testable whenever practical.

## Engine vs Trading Hypothesis

Before adding any new feature, determine whether it is:

1. An Engine capability
2. A trading hypothesis

Engine capabilities describe market structure or confluence.

Examples:

- Classic Level
- GAP Level
- BO Level
- HNS Level
- EQ

Trading hypotheses describe how market structure may be traded.

Examples:

- Fixed TP vs Structure TP
- Full Exit vs Partial Exit
- Reverse enabled vs disabled
- Re-entry enabled vs disabled
- Session filter enabled vs disabled

Trading hypotheses must not be hardcoded into the Engine.

Trading hypotheses should be implemented as configurable Strategy Policies whenever practical.

## Design Goals

The framework should maximize:

- Reusability
- Configurability
- Testability
- Extensibility
- Statistical Research

The framework should minimize:

- Hardcoded logic
- Tight coupling
- Duplicate implementations
- Strategy-specific Engine code

## Core Principle

The Engine should remain stable.

Strategies should evolve through research.

Research should evolve through modular Policies.

No research idea should require rewriting the Engine whenever possible.

## Important Boundaries

The Level Engine owns market structure.

The EQ Engine owns confluence.

The Strategy owns trading decisions.

Execution owns order and position lifecycle.

The Renderer owns visualization.

Strategy must not modify Levels.

Strategy must not modify EQs.

Execution must not modify Engine state.

## Strategy Specification v0.1 Rules

The framework data lifecycle is:

Market Data

↓

Level

↓

EQ

↓

Trade Plan

↓

Trade

Strategy consumes EQ output.

Strategy must never modify:

- Levels
- EQs
- Engine state
- Execution state

Strategy produces Trade Plans.

Strategy must not execute orders.

Execution consumes Trade Plans.

Execution owns:

- Pending orders
- Filled orders
- Protective exit orders
- Position lifecycle
- Closed trades
- Trade archive

Execution must never modify:

- Levels
- EQs
- Engine state
- Strategy rules

Trade Plan represents trading intent.

Trade Plan is not an executed trade.

Trade Plan must not contain execution-only data such as:

- Actual fill price
- Actual exit price
- Slippage
- Realized PnL
- MAE
- MFE

Trade represents the actual executed result.

Closed Trades must be archived.

Trades must never be deleted.

Strategy v0.1 must remain modular.

Each Strategy Policy should contain:

- Rules
- Parameters

Policies should be configurable whenever practical.

All qualifying EQs may generate Trade Plans.

There is no single-trade-plan limit in Strategy v0.1.

Recovery rules are optional and configurable.

Reverse and Re-entry must not be hardcoded.

No trendline-based stop logic is implemented in Strategy v0.1.

Trailing is reserved for future versions and is not implemented in Strategy v0.1.

Session Policy is reserved and is not implemented in Strategy v0.1.

Pending orders are managed by Execution.

There is no fixed pending order expiration in Strategy v0.1.

## Detector Ownership

Detectors may read Engine data.

Detectors must never modify Engine data.

Any state update belongs to Lifecycle.

## Lifecycle Rules

Lifecycle is responsible only for updating Level state.

Lifecycle may update:

- fresh
- valid
- boCreated
- hnsCreated

Lifecycle must never:

- create Levels
- delete Levels
- change Level identity
- perform rendering

## State Transition

Lifecycle state changes must be explicit.

Allowed transitions:

fresh:
true → false

valid:
true → false

boCreated:
false → true

hnsCreated:
false → true

Reverse transitions are not allowed unless explicitly defined by a future milestone.

## Regression Rule

A new milestone must never break functionality implemented in previous milestones.

If a regression is introduced, it must be fixed before implementing additional features.

Preserving existing behavior has higher priority than adding new functionality.

## Candle Classification

Bull Candle:
close > open

Bear Candle:
close < open

Doji:
close == open

Doji is neither Bull nor Bear.

Detectors must explicitly exclude Doji unless a future Level definition specifies otherwise.

# Engine Design Rules

These rules are mandatory for every future implementation.

## 1. Single Responsibility Principle

Each function must have one and only one responsibility.

Examples:

- Detector detects new Levels.
- Lifecycle updates existing Levels.
- Renderer draws Levels.
- Strategy generates trading signals.

A function must never perform responsibilities belonging to another module.

---

## 2. One-Way Data Flow

The system architecture follows a strict one-way data flow.

Market Data

↓

Level Engine

↓

EQ Engine

↓

Strategy

↓

Execution

↓

Renderer

Modules must never modify upstream modules.

The internal processing pipeline of the Level Engine is independent from the overall system architecture.

---

## 3. Detector Never Updates Existing Levels

Detector is responsible only for detecting market structure.

Detector must never:

- create Levels
- update existing Levels
- perform rendering

Detector returns only detection results.

Creation belongs to the Creation module.

Lifecycle updates existing Levels.

---

## 4. Lifecycle Never Creates Levels

Lifecycle only updates existing Levels.

Lifecycle must never create or delete Levels.

---

## 5. Renderer Never Changes Market Logic

Renderer is responsible only for visualization.

Renderer may:

- create lines
- update colors
- hide lines

Renderer must never modify:

- price
- fresh
- valid
- levelType

## Renderer Colors

Use the following fixed renderer colors:

SUPPORT_COLOR = color.rgb(0, 136, 122)

RESISTANCE_COLOR = color.rgb(125, 75, 0)

---

## 6. Strategy Never Changes Engine Data

Strategy consumes Engine output only.

Strategy must never modify any Level.

---

## 7. Data Model Stability

The Level data model is considered stable.

Future features should extend behavior through functions rather than changing the Level structure whenever possible.

---

## 8. Add Functions Instead of Modifying Existing Ones

New features should be implemented by adding new functions.

Avoid changing existing function responsibilities.

Examples:

Good:

updateFresh()

updateReject()

updateBreak()

Bad:

Adding Fresh logic into detectClassic()

---

## 9. Main Should Remain Stable

The Main execution flow should remain as stable as possible.

Target architecture:

updateLevels()

↓

detectClassic()

↓

detectGap()

↓

detectBO()

↓

detectHNS()

↓

renderLevels()

Future milestones should only add new modules without restructuring Main.

---

## 10. Architecture Stability

The Engine architecture is considered stable.

Future milestones should extend the existing architecture rather than restructure it.

Prefer adding new:

- Detectors
- Lifecycle logic
- Rendering logic
- Strategy modules

instead of modifying the Engine pipeline or existing module responsibilities.

---

## 11. Python Compatibility

All Engine logic should be designed to allow direct migration to Python with minimal changes.

Business logic must remain independent from TradingView rendering.
