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

## Milestone 1

Classic Detection

Deliverables

- Classic Level detection
- Classic Level rendering
- Generic Level data model
- TradingView visualization

Status

Completed

---

## Milestone 2

Engine Foundation

Deliverables

- Stable Engine architecture
- Detector module
- Lifecycle module (framework only)
- Level Manager
- Renderer
- Generic Level data model
- Main execution pipeline
- Production-quality code structure

Status

In Progress

---

## Milestone 3

Fresh Lifecycle

Goal

Implement the first lifecycle logic for Levels.

Definition of Done

□ Detect first touch for Support Levels.

□ Detect first touch for Resistance Levels.

□ Ignore the creation bar.

□ Update fresh from true to false after first touch.

□ Renderer only displays Levels where fresh == true.

□ Main execution flow remains unchanged.

□ No changes to Detector, Creation, or Data Model.

---

## Milestone 4


Goal

Implement Break Detection.

Create BO Levels using the generic createLevel() function.

Do not create a dedicated createBOLevel().

A Level may generate at most one BO Level.

Once a Level has generated a BO Level,
it must never generate another BO Level.

A BO Level must be created only once on the first confirmed break. 

---

## Milestone 5


Engine Refactor

Goal

Freeze Engine Architecture.

Introduce stable queue-based engine infrastructure.

No new trading functionality.

Deliverables

- Stable Level Manager API
- Stable Queue API
- Stable Renderer API
- Cleaner candidate queues
- No changes to market logic

---

Milestone 6 — GAP Levels
Milestone 7 — HNS Levels
Milestone 8 — EQ Engine v1.0
Milestone 9 — Strategy Engine
Milestone 10 — TradingView Backtesting
Milestone 11 — Python Research Engine


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


## Current Status

Completed

- ✅ Milestone 1 — Engine Skeleton
- ✅ Milestone 2 — Classic Levels
- ✅ Milestone 3 — Fresh Lifecycle
- ✅ Milestone 4 — BO Levels
- ✅ Milestone 5 — Queue API + Dirty Renderer + Renderer Text

In Progress

- 🚧 Milestone 6 — GAP Levels

Next

- HNS
- EQ
- Strategy