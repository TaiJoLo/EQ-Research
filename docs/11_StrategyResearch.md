# Strategy Research Log

Version: 1.0

---

# Document Purpose

This document is not a specification.

It does not define system behavior.

It is a research log used to track trading hypotheses, test configurations, results, and decisions.

The goal is to prevent repeated testing, preserve reasoning, and build an evidence-based Strategy over time.

---

# Research Entry Format

Each research item should use the following repeatable structure.

## Research ID

Example:

RQ-001

## Title

Short name of the research question.

## Category

Examples:

- Entry Policy
- Stop Policy
- Target Policy
- Position Management Policy
- Reverse Policy
- Re-entry Policy
- Risk Policy
- Session Policy
- EQ Source Type
- Timeframe Combination

## Hypothesis

What is being tested and why.

## Configuration

The policy settings or parameters used in the test.

Example:

Target Policy:

- Mode: Fixed Distance
- GC Target: 100
- XAU Target: 1000

## Comparison

What this test is compared against.

Example:

Fixed Distance TP vs H1 Structure TP

## Metrics

The metrics that should be collected.

Examples:

- Profit Factor
- Win Rate
- Expectancy
- Average Win
- Average Loss
- Max Drawdown
- Number of Trades
- Day Win Rate
- Average R
- Median R

## Result

Backtest result summary.

## Status

Allowed values:

- Pending
- Testing
- Accepted
- Rejected
- Deferred

## Decision

Final decision and reason.

## Notes

Additional observations.

---

# Initial Research Backlog

## RQ-001

Title

Fixed Distance TP vs H1 Structure Target

Category

Target Policy

Hypothesis

Structure-based targets may improve expectancy compared with fixed-distance targets.

Configuration

Pending.

Comparison

Fixed Distance TP vs H1 Structure Target.

Metrics

Pending.

Result

Pending.

Status

Pending

Decision

Pending.

Notes

Pending.

---

## RQ-002

Title

Full Exit vs Partial Exit

Category

Position Management Policy

Hypothesis

Partial exit with BE may reduce drawdown but may also reduce average win.

Configuration

Pending.

Comparison

Full Exit vs Partial Exit.

Metrics

Pending.

Result

Pending.

Status

Pending

Decision

Pending.

Notes

Pending.

---

## RQ-003

Title

Reverse Enabled vs Reverse Disabled

Category

Reverse Policy

Hypothesis

Reverse setups after failed EQ reactions may improve expectancy.

Configuration

Pending.

Comparison

Reverse Enabled vs Reverse Disabled.

Metrics

Pending.

Result

Pending.

Status

Pending

Decision

Pending.

Notes

Pending.

---

## RQ-004

Title

Re-entry 0 vs Re-entry 1

Category

Re-entry Policy

Hypothesis

One same-direction re-entry after wick stop may recover valid EQ reactions, but additional re-entries may increase churn.

Configuration

Pending.

Comparison

Re-entry 0 vs Re-entry 1.

Metrics

Pending.

Result

Pending.

Status

Pending

Decision

Pending.

Notes

Pending.

---

## RQ-005

Title

Classic + Classic EQ vs Classic + GAP EQ

Category

EQ Source Type

Hypothesis

Different EQ source combinations may have different win rates and expectancy.

Configuration

Pending.

Comparison

Classic + Classic EQ vs Classic + GAP EQ.

Metrics

Pending.

Result

Pending.

Status

Pending

Decision

Pending.

Notes

Pending.

---

## RQ-006

Title

Long Only vs Short Only

Category

Direction Filter

Hypothesis

Long and short trades may have different performance profiles.

Configuration

Pending.

Comparison

Long Only vs Short Only.

Metrics

Pending.

Result

Pending.

Status

Pending

Decision

Pending.

Notes

Pending.

---

## RQ-007

Title

Session Filter Impact

Category

Session Policy

Hypothesis

Restricting trades to specific sessions may improve expectancy or reduce drawdown.

Configuration

Pending.

Comparison

Session Filter enabled vs Session Filter disabled.

Metrics

Pending.

Result

Pending.

Status

Pending

Decision

Pending.

Notes

Pending.

---

# Research Principles

- Change one major variable at a time whenever practical.
- Keep Engine logic stable during Strategy research.
- Prefer configurable Policies over code changes.
- Record both accepted and rejected hypotheses.
- A rejected test is still useful evidence.
- Do not promote a Strategy rule without statistical evidence.
- Every research result should be traceable to a specific configuration.
