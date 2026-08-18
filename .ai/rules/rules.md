---
paths:
  - '.ai/rules/**'
---

# Rules

## Do not silently invent material business rules
Do not silently choose scoring weights, liquidity meanings, better-alternative thresholds, renovation coefficients, confidence thresholds, or other decisions that change product meaning. Surface the missing decision and propose an option before fixing it in code. Record only newly settled durable architecture/domain decisions through Laravel Boost `record-rule`, never temporary TODOs.
