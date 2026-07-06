# Development Roadmap

Version: 1.0

---

# Vision

Build a professional, research-driven EQ trading framework.

The project is divided into independent milestones.

Each milestone must be fully tested before moving to the next.

No feature should be implemented before its underlying layer is complete.

---

# Architecture

Market Data

↓

Level Engine

↓

EQ Engine

↓

Strategy

↓

Renderer

---

# Milestone 1

## Classic Level Detection

Status

In Progress

Objectives

- Detect Classic Support
- Detect Classic Resistance
- Ignore Doji
- Require adjacent candles only
- Confirm only after the second candle closes
- Store every detected Level
- Draw detected Levels

Deliverables

- Classic detection
- Level data structure
- Initial Renderer

---



# Milestone 2



## Level Lifecycle

Objectives

Implement complete lifecycle management.

Features

- Fresh
- Reject
- Break
- Active
- Valid

Deliverables

- Level Manager
- Active Level filtering
- Automatic state updates

---



# Milestone 3



## BO Levels

Objectives

Implement Break-Out Levels.

Features

- Classic → BO
- Direction reversal
- Fresh BO
- BO lifecycle

Deliverables

- BO detection
- BO state management

---



# Milestone 4



## HNS Levels

Objectives

Implement HNS Levels.

Features

- BO → HNS
- HNS lifecycle
- HNS expiration

Deliverables

- HNS detection
- HNS state management

---



# Milestone 5



## GAP Levels

Objectives

Implement GAP Levels.

Features

- GAP Support
- GAP Resistance
- GAP lifecycle

Deliverables

- GAP detection
- GAP management

---



# Milestone 6



## Multi-Timeframe Engine

Objectives

Synchronize Levels across multiple timeframes.

Features

- HTF Level collection
- LTF Level collection
- Price matching
- Level synchronization

Deliverables

- Multi-timeframe database
- Level matching engine

---



# Milestone 7



## EQ Engine

Objectives

Generate EQ candidates.

Features

- Classic + Classic
- Classic + BO
- Classic + GAP
- GAP + GAP
- BO + BO
- HNS combinations

Deliverables

- EQ detection
- EQ database

---



# Milestone 8



## Strategy Engine

Objectives

Convert EQ into trading signals.

Features

- Entry rules
- Stop Loss
- Take Profit
- Risk Management

Deliverables

- Tradable strategy
- Signal generation

---



# Milestone 9



## Research Framework

Objectives

Evaluate strategy performance.

Features

- Parameter optimization
- EQ combination comparison
- SL optimization
- TP optimization
- Multi-timeframe comparison

Deliverables

- Research reports
- Performance statistics

---



# Milestone 10



## Production Release

Objectives

Prepare a stable production version.

Features

- Performance optimization
- Documentation review
- Code cleanup
- Version tagging

Deliverables

- Version 1.0
- Stable release

---



# Development Principles

Always complete lower layers before higher layers.

Never allow Strategy to modify market structure.

Never allow Renderer to modify market structure.

The Engine owns market structure.

The EQ Engine owns EQ generation.

The Strategy owns trading decisions.

The Renderer owns visualization.

---



# Version Policy

Every completed milestone must include:

- Git Commit
- CHANGELOG update
- Documentation update
- TradingView validation
- Code review

Only after all checks pass may development continue to the next milestone.