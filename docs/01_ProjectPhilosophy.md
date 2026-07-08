# Project Philosophy

Version: 1.0

---

# Trading Research Framework Philosophy

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

---

# Research Philosophy

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

---

# Modular Strategy Design

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

---

# Engine vs Trading Hypothesis

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

---

# Design Goals

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

---

# Core Principle

The Engine should remain stable.

Strategies should evolve through research.

Research should evolve through modular Policies.

No research idea should require rewriting the Engine whenever possible.

---

# Important Boundaries

The Level Engine owns market structure.

The EQ Engine owns confluence.

The Strategy owns trading decisions.

Execution owns order and position lifecycle.

The Renderer owns visualization.

Strategy must not modify Levels.

Strategy must not modify EQs.

Execution must not modify Engine state.
