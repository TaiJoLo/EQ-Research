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

---

## Constants

Always use named constants.

Never use magic numbers.

---

## Engine Pipeline

Detection

↓

Creation

↓

Lifecycle Update

↓

Rendering

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

The system follows a strict one-way data flow.

Market Data

↓

Detector

↓

Level Manager

↓

Renderer

↓

Strategy

Modules must never modify upstream modules.

---

## 3. Detector Never Updates Existing Levels

Detector is only responsible for creating new Levels.

Detector must never:

- update Fresh
- update Reject
- update Break
- update BO
- update HNS

These belong to Lifecycle.

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
- active
- valid
- levelType

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

## 10. Python Compatibility

All Engine logic should be designed to allow direct migration to Python with minimal changes.

Business logic must remain independent from TradingView rendering.
