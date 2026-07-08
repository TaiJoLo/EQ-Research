# Development Roadmap

Version: 2.0

---

# Vision

Build a professional, research-driven trading framework.

The project evolves through independent milestones.

Each milestone represents a complete deliverable.

A deliverable may be:

- Specification
- Data Model
- Implementation
- Validation
- Research

Future milestones may be added, removed, merged, split, or reordered as research evolves.

Milestone IDs are permanent historical identifiers.

They do not imply a fixed implementation order.

The Roadmap is a living document.

---

# System Architecture

Market Data

↓

Level Engine

↓

EQ Engine

↓

Strategy

↓

Trade Plan

↓

Execution

↓

Renderer

---

# Completed Milestones

## M001 — Classic Detection

Status

Completed

Deliverables

- Classic Level detection
- Support / Resistance detection
- Generic Level data model
- TradingView visualization
- Initial Engine prototype

---

## M002 — Engine Foundation

Status

Completed

Deliverables

- Stable Engine architecture
- Detector module
- Level Manager
- Renderer framework
- Generic createLevel() API
- Main Engine execution pipeline
- Production-quality project structure

---

## M003 — Fresh Lifecycle

Status

Completed

Deliverables

- Fresh lifecycle
- First-touch detection
- Creation bar exclusion
- Support / Resistance touch detection
- Fresh state management
- Renderer integration for Fresh Levels

---

## M004 — BO Levels

Status

Completed

Deliverables

- Break detection
- BO Level creation
- Generic createLevel() reuse
- One BO per Classic Level
- BO lifecycle integration
- Break confirmation on candle close

---

## M005 — Engine Refactor

Status

Completed

Deliverables

- Stable Level Manager API
- Queue-based architecture
- Candidate queues
- Dirty Renderer
- Stable Renderer API
- Engine cleanup
- Production-ready Engine infrastructure

---

## M006 — GAP Levels

Status

Completed

Deliverables

- GAP Level detection
- GAP rendering
- GAP lifecycle integration
- GAP support / resistance logic
- Engine integration

---

## M007 — HNS Levels

Status

Completed

Deliverables

- HNS detection
- HNS lifecycle
- BO → HNS evolution
- HNS freshness rules
- HNS rendering
- Engine lifecycle refinement
- Market structure validation

---

## M008 — EQ Engine Specification

Status

Completed

Deliverables

- EQ Engine architecture
- EQ Data Model
- EQ creation rules
- Snapshot design
- Source immutability
- Multi-timeframe architecture
- Strategy boundary definition
- Execution boundary definition
- Renderer boundary definition
- Trading Research Framework philosophy
- Modular Strategy Policy architecture
- Engine vs Strategy responsibility definition

---

# Current Milestone

## M009 — Strategy Specification

Status

In Progress

Goal

Design a modular, research-driven Strategy framework.

Current scope

- Strategy Specification v0.1
- Trade Plan Data Model
- Trade Data Model
- Core Data Lifecycle
- Entry Policy
- Stop Policy
- Target Policy
- Position Management Policy
- Reverse Policy
- Re-entry Policy
- Risk Policy
- Session Policy (reserved)
- Pending Order Lifecycle
- Trade Archive
- Future Policy extension design

The Strategy expresses trading hypotheses.

Every trading hypothesis should be represented as an independent configurable Policy whenever practical.

The Strategy produces Trade Plans.

Execution consumes Trade Plans.

Trades are archived for research and statistics.

---

# Planned Milestones

The following milestones are planned but their implementation order is intentionally flexible.

Possible future milestones include:

- Trade Plan Data Model
- EQ Engine Implementation
- Strategy Implementation
- Execution
- TradingView Backtesting
- Python Research Framework
- Research Statistics
- Optimization Framework
- Walk Forward Testing
- Monte Carlo Analysis
- Live Trading Integration
- Broker Integration
- Multi-Account Execution
- Dashboard
- Additional Strategy Policies

Future milestones may be:

- Added
- Removed
- Split
- Merged
- Deferred
- Reordered

according to research priorities.

---

# Development Principles

Complete lower layers before higher layers whenever practical.

The Engine owns market structure.

The EQ Engine owns confluence.

The Strategy owns trading hypotheses.

Trade Plan owns trading intent.

Execution owns order and position lifecycle.

Renderer owns visualization.

Research should extend configurable Strategy Policies instead of modifying the Engine whenever practical.

Every new trading idea should first be evaluated as a Strategy Policy before introducing new Engine functionality.

---

# Validation Policy

Every completed milestone must include the validations appropriate to its type.

Specification Milestones

- Documentation Review
- Architecture Review
- Internal Consistency Review

Implementation Milestones

- Code Review
- Unit Validation
- TradingView Validation (when applicable)

Research Milestones

- Statistical Validation
- Backtesting
- Performance Comparison

Only after all required validation passes may a milestone be considered complete.

---

# Current Status

Completed

- ✅ M001 — Classic Detection
- ✅ M002 — Engine Foundation
- ✅ M003 — Fresh Lifecycle
- ✅ M004 — BO Levels
- ✅ M005 — Engine Refactor
- ✅ M006 — GAP Levels
- ✅ M007 — HNS Levels
- ✅ M008 — EQ Engine Specification

In Progress

- 🚧 M009 — Strategy Specification

Next

To be determined after M009 is completed and reviewed.

Future milestones are selected based on research priorities rather than a fixed development sequence.
