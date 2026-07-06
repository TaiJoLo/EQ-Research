# EQ Research

## Project Goal

Develop a professional quantitative research platform based on the EQ (Equilibrium) price level model.

This repository is designed to separate:

- Market Structure Engine
- Trading Strategy
- Research & Statistics

The Engine should detect every valid market level without considering whether it should be traded.

Trading rules are implemented separately so different ideas can be tested without modifying the Engine.

---

## Project Structure

docs/

```
Specifications
```

pine/

```
TradingView indicators & strategies
```

python/

```
Research platform
```

tests/

```
Validation
```

data/

```
Exported datasets
```

The Engine produces market structure.

Visualization is handled by the TradingView Renderer.

Trading decisions are handled by the Strategy module.

---



## Development Roadmap

Phase 1

- Classic
- GAP
- BO
- HNS
- Fresh
- EQ

Phase 2

Trading Strategy

Phase 3

Python Research Platform

Phase 4

Optimization