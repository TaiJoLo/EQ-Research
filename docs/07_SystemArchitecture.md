# System Architecture

Version: 0.4

---

# Design Philosophy

The project follows a layered architecture.

Each module has exactly one responsibility.

Modules communicate in one direction only.

No module may depend on a higher-level module.

---

# Overall Architecture

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

# Architecture vs Implementation

The Level Engine Processing Pipeline is an internal implementation detail of the Level Engine.

It is independent from the overall system architecture.

The system architecture defines the responsibilities and communication between modules.

The internal processing pipeline defines how a single module performs its work.

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

# Module Responsibilities

## 1. Detector

Purpose

Detect raw market structure.

Responsibilities

• Detect Classic Levels

• Detect GAP Levels

• Detect BO creation

• Detect HNS creation

Output

New Levels only.

The Detector never updates existing Levels.

---



## 2. Level Manager

Purpose

Manage the lifecycle of every Level.

Responsibilities

• Fresh

• Reject

• Break

• BO conversion

• HNS conversion

• Duplicate handling

• Valid status

Output

Current market structure.

The Level Manager is the only module allowed to modify existing Levels.

---



## 3. EQ Engine

Purpose

Generate EQ confluence.

Responsibilities

Consume Level Engine output.

Compare Levels across configurable timeframes.

Create EQ snapshots from valid Fresh source Levels.

Examples

Classic + Classic

Classic + Gap

Gap + Gap

Classic + BO

Gap + BO

HNS + Classic

The EQ Engine never modifies Levels.

The EQ Engine is read-only with respect to the Level Engine.

The EQ Engine must not hardcode Strategy timeframe combinations.

The EQ Engine must support configurable timeframe combinations.

EQ is not a Level Type and must not be stored inside allLevels.

---



## 4. Strategy

Purpose

Generate trading decisions.

Responsibilities

Choose which EQ combinations are tradable.

Produce Trade Plans.

Express trading hypotheses through configurable Policies.

Define planned entry, stop, target, position management, recovery, and risk parameters.

The Strategy consumes EQ output.

The Strategy does not execute orders.

The Strategy does not modify Levels.

The Strategy does not modify EQs.

The Strategy does not modify Engine state.

The Strategy does not modify Execution state.

---



## 5. Execution

Purpose

Execute trade plans produced by Strategy.

Responsibilities

Manage order lifecycle.

Manage position lifecycle.

Create Trades from executed Trade Plans.

Archive closed Trades.

Execution must not modify Engine state.

Execution must not modify Levels.

Execution must not modify EQs.

Execution must not modify Strategy rules.

---



## 6. Renderer

Purpose

Display information on TradingView.

Responsibilities

Draw Levels.

Delete non-renderable Levels.

Display EQ.

Display Labels.

The Renderer never modifies market structure.

It only visualizes Engine output.

---



# Data Flow

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



# Dependency Rules

Level Engine

↓

EQ Engine

↓

Strategy

↓

Execution

↓

Renderer

Dependencies are one-way only.

No module may reference a higher-level module.

---



# Core Principles

The Engine owns market structure.

The EQ Engine owns confluence.

The Strategy owns trading decisions.

Execution owns order and position lifecycle.

The Renderer owns visualization.

Every module has exactly one responsibility.
