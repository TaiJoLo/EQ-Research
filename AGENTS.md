# EQ Engine

This repository implements a production-quality TradingView Pine Script engine.

## Source of Truth

Always read the following documents before writing code.

docs/02_[Engine.md](http://Engine.md)

docs/04_[DataModel.md](http://DataModel.md)

docs/07_[SystemArchitecture.md](http://SystemArchitecture.md)

docs/08_[CodingStandards.md](http://CodingStandards.md)

docs/09_[DevelopmentRoadmap.md](http://DevelopmentRoadmap.md)

The documentation is the source of truth.

Never invent architecture.

Never contradict documentation.

---

## Coding Rules

- Pine Script v6 only.

- Production-quality code.

- Single Responsibility Principle.

- Never use magic numbers.

- Never use magic strings.

- Renderer never changes market logic.

- Detector only creates Levels.

- Lifecycle only updates Levels.

- Level Manager coordinates updates.

- Strategy never modifies Engine.

---

## Development Rules

Always implement only the requested milestone.

Do not implement future milestones.

Do not leave TODO placeholders.

Every implementation must compile successfully.

