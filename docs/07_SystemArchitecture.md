# System Architecture

Version: 0.2

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
  
Detector  
  
↓  
  
Level Manager  
  
↓  
  
Renderer  
  
↓  
  
Strategy

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



## 3. Research Engine

Purpose

Generate EQ candidates.

Responsibilities

Compare Levels across multiple timeframes.

Research different EQ combinations.

Examples

Classic + Classic

Classic + Gap

Gap + Gap

Classic + BO

Gap + BO

HNS + Classic

The Research Engine never modifies Levels.

---



## 4. Strategy

Purpose

Generate trading decisions.

Responsibilities

Choose which EQ combinations are tradable.

Manage TP.

Manage SL.

Manage position sizing.

Perform performance analysis.

The Strategy never changes market structure.

---



## 5. Renderer

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

Detector

↓

Level Manager

↓

Research Engine

↓

Strategy

↓

Renderer

---



# Dependency Rules

Detector

↓

Level Manager

↓

Research Engine

↓

Strategy

↓

Renderer

Dependencies are one-way only.

No module may reference a higher-level module.

---



# Core Principles

The Engine owns market structure.

The Research Engine owns EQ discovery.

The Strategy owns trading decisions.

The Renderer owns visualization.

Every module has exactly one responsibility.
