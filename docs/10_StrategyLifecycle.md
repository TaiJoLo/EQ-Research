# Strategy Lifecycle

Version: 0.1

---

# Purpose

This document defines Strategy Specification v0.1.

It defines the Strategy role, Trade Plan lifecycle, Trade lifecycle, and the boundary between Strategy and Execution.

This document does not implement Strategy.

This document does not implement Execution.

This document does not modify Engine behavior.

---

# Core Data Lifecycle

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

Each object has a single responsibility.

Level:

Market structure.

EQ:

Confluence.

Trade Plan:

Strategy decision / trading intent.

Trade:

Executed trade result and research record.

---

# Strategy Role

Strategy consumes EQ output.

Strategy does not modify:

- Levels
- EQs
- Engine state
- Execution state

Strategy produces Trade Plans.

Strategy does not execute orders.

Execution consumes Trade Plans.

---

# Execution Role

Execution owns:

- Pending orders
- Filled orders
- Protective exit orders
- Position lifecycle
- Closed trades
- Trade archive

Execution does not modify:

- Levels
- EQs
- Engine state
- Strategy rules

---

# Trade Plan Definition

Trade Plan represents trading intent.

Trade Plan is not an executed trade.

Trade Plan must not contain execution-only data such as:

- Actual fill price
- Actual exit price
- Slippage
- Realized PnL
- MAE
- MFE

Those belong to Trade.

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

The state names are intentionally simple for v0.1.

---

# Trade Definition

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

---

# Strategy v0.1 Policies

Strategy v0.1 is modular.

It is composed of independent Policies.

Each Policy should contain:

- Rules
- Parameters

Policies should be configurable whenever practical.

---

## Entry Policy

Fresh Support EQ:

Create Buy Limit Trade Plan at EQ price.

Fresh Resistance EQ:

Create Sell Limit Trade Plan at EQ price.

All qualifying EQs may generate Trade Plans.

There is no single-trade-plan limit in v0.1.

---

## Stop Policy

Stop values are configurable inputs.

No trendline-based stop logic is implemented in v0.1.

Protective stop orders are placed after entry is filled.

Order type details are handled by Execution.

---

## Target Policy

Target Policy must support multiple target modes.

Initial target modes:

- Fixed Distance
- Structure Target

Fixed Distance:

Uses configurable target distance inputs.

Structure Target:

Uses configurable structure source.

Current structure candidates include:

- H1 Classic Level
- H1 EQ

Future structure candidates may be added.

Target Policy should not be hardcoded to one target type.

---

## Position Management Policy

Position management must support:

- Full Exit
- Partial Exit

Partial Exit:

If enabled, exit a configurable percentage at the first target.

Break Even:

Break Even is only applicable when Partial Exit is enabled.

If Exit Mode is Full Exit, Break Even is not used.

Trailing:

Reserved for future versions.

Not implemented in v0.1.

---

## Recovery Policy

Recovery Policy includes:

- Reverse
- Re-entry

Recovery rules are optional and configurable.

Reverse and Re-entry must not be hardcoded.

---

## Reverse Rule

If enabled:

When a trade fails according to the Direct SL or SL Zone rule, a reverse Trade Plan may be created.

Reverse Entry Price:

Original Entry Price.

Max Reverse Count:

Configurable input.

Default:

1

Reverse after Reverse:

Not allowed by default.

---

## Re-entry Rule

If enabled:

When a trade is stopped by wick behavior and the Primary Timeframe closes outside the SL zone, a same-direction Re-entry Trade Plan may be created.

Re-entry Price:

Original Entry Price.

Max Re-entry Count:

Configurable input.

Default:

1

Repeated Re-entry:

Not allowed by default.

---

## SL Zone Rule

SL Zone is the price area between Entry Price and Stop Price.

Long:

SL <= Close <= Entry

Short:

Entry <= Close <= SL

If price closes inside the SL Zone before stop is hit:

- Close the active position.
- If Reverse is enabled, create a reverse Trade Plan at the Original Entry Price.

---

## Direct SL Rule

If an H1 candle directly hits Stop Loss after EQ entry:

- The original trade is closed.
- If Reverse is enabled, create a reverse Trade Plan at the Original Entry Price.

The candle timeframe is Primary Timeframe.

Current Primary Timeframe:

H1

---

## Wick Stop Re-entry Rule

If price hits Stop Loss by wick, and the Primary Timeframe closes outside the SL Zone:

- If Re-entry is enabled, create a same-direction Re-entry Trade Plan at the Original Entry Price.

The candle timeframe is Primary Timeframe.

Current Primary Timeframe:

H1

---

## Risk Policy

Risk % is configurable.

Position size is derived from Risk %, Entry Price, and Stop Price.

Advanced risk logic is reserved for future versions.

---

## Session Policy

Session Policy is reserved.

No session filter is implemented in v0.1.

Future versions may add:

- Enable Session Filter
- Session Start
- Session End

---

# Pending Order Lifecycle

Pending orders are managed by Execution.

A pending order may remain active as long as its Trade Plan remains valid under current Strategy rules.

There is no fixed expiration in v0.1.

Future filters may cancel pending orders if the Trade Plan no longer qualifies.

Examples:

- Session Filter
- Trend Filter
- News Filter
- Risk Filter

---

# Trade Archive

Closed Trades must be archived.

Trades are never deleted.

Archived Trades are required for:

- Backtesting
- Research
- Statistics
- Performance review
- Optimization

---

# Research Principles

Strategy rules should be modular.

Research ideas should become configurable Policies or Parameters.

The Engine must remain stable during Strategy research.

Do not modify Engine logic to test Strategy hypotheses.
