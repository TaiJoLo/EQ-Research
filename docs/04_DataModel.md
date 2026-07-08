# Data Model

Version: 0.4

---

# Philosophy

The Data Model defines the core project data objects.

Each data object has one responsibility.

Level represents market structure.

EQ represents confluence.

Trade Plan represents Strategy intent.

Trade represents executed trade results and research records.

Data objects must never contain TradingView drawing objects.

Rendering belongs to the Renderer.

Trading decisions belong to Strategy.

Order and position lifecycle belongs to Execution.

---

# Level

| Field | Type | Description |

|---------|------|-------------|

| price | float | Level price |

| direction | int | SUPPORT / RESISTANCE |

| creationBar | int | Bar index where the Level is confirmed |

| levelType | int | Classic / Gap / BO / HNS |

| fresh | bool | Whether the Level has never been tested |

| valid | bool | Whether the Level is still structurally valid |

| boCreated | bool | Whether the Level has already created a BO Level |

| hnsCreated | bool | Whether the Level has already created an HNS Level |

| id | int | Unique identifier |

## Level Type Enumeration

Level Type is stored as an integer.

Values:

LEVEL_CLASSIC = 1

LEVEL_GAP = 2

LEVEL_BO = 3

LEVEL_HNS = 4

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

valid

boCreated

hnsCreated

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

The Level data model represents market structure only.

---

# EQ

EQ is not a Level Type.

EQ is a Confluence created from existing Levels.

EQ must not be stored inside allLevels.

There must not be a LEVEL_EQ.

EQ must have its own data model.

## EQ Fields

| Field | Type | Description |

|---------|------|-------------|

| id | int | Unique EQ identifier |

| creationTime | int | Time where the EQ is created |

| creationBar | int | Bar index where the EQ is created |

| price | float | EQ price |

| direction | int | SUPPORT / RESISTANCE |

| fresh | bool | Whether the EQ has not been touched after creation |

| sourceCount | int | Number of source Levels in the EQ |

| sourceTimeframes | array<string> | Timeframe for each source Level |

| sourceLevelIds | array<int> | Level ID for each source Level |

| sourceLevelTypes | array<int> | Level Type for each source Level |

## EQ State

EQ v1 only needs Fresh.

EQ does not need Valid in v1.

Reason:

- Level valid represents market structure evolution.
- EQ is not market structure.
- EQ does not evolve into another type.
- EQ is a snapshot confluence.

## EQ Source Snapshot

Once an EQ is created:

- Source Level IDs never change.
- Source Level Types never change.
- Source Timeframes never change.

EQ never replaces one source Level with another.

The data model must support future EQs containing more than two source Levels.

---

# EQ Important Principles

An EQ never stores TradingView objects.

An EQ never stores Strategy orders, positions, risk, entries, or exits.

An EQ is read-only with respect to source Levels.

An EQ never modifies source Levels.

An EQ represents a snapshot of market confluence.

---

# Trade Plan

Trade Plan represents trading intent.

Trade Plan is not an executed trade.

Trade Plan is produced by Strategy and consumed by Execution.

Trade Plan must not contain execution-only result data.

Execution-only result data belongs to Trade.

## Trade Plan Fields

Identity:

- Trade Plan ID
- Strategy Version

Source:

- EQ ID
- Source Level IDs
- Source Level Types
- Source Timeframes

Entry:

- Direction
- Entry Price
- Entry Order Type
- Original Entry Price

Stop:

- Stop Policy
- Stop Price

Target:

- Target Policy
- Target Price
- Target Mode

Position Management:

- Exit Mode
- Partial Exit %
- Break Even Enabled
- Break Even Offset
- Trailing Reserved

Recovery:

- Recovery Policy Enabled
- Reverse Enabled
- Re-entry Enabled
- Max Reverse Count
- Max Re-entry Count
- Recovery Entry Price

Risk:

- Risk %
- Position Size

State:

- Pending
- Filled
- Closed

## Trade Plan Important Principles

A Trade Plan never modifies Levels.

A Trade Plan never modifies EQs.

A Trade Plan never executes orders.

A Trade Plan records Strategy intent only.

---

# Trade

Trade represents the actual executed result.

Trade is created from a Trade Plan after execution.

Trade must be archived and never deleted.

## Trade Fields

Identity:

- Trade ID
- Trade Plan ID
- Strategy Version

Source:

- EQ ID
- Source Level IDs
- Source Level Types
- Source Timeframes

Execution:

- Planned Entry Price
- Actual Entry Price
- Planned Stop Price
- Actual Exit Price
- Entry Time
- Exit Time
- Order Type
- Position Size

Result:

- Profit / Loss
- R Multiple
- Win / Loss
- Duration
- MAE
- MFE
- Exit Reason

Archive:

- Closed trades are archived.
- Trades must never be deleted.
- Archived trades are used for research and statistics.

## Trade Important Principles

Trade contains execution result data.

Trade is a research record.

Trade does not modify Levels.

Trade does not modify EQs.
