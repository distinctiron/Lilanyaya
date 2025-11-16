Title: Epic 3 — Story 3.1: Seeded pseudorandom initialization

User story:
As a system, I need to initialize a single seed so that all randomization is reproducible.

Acceptance criteria:
- Run manifest records the seed; all schedule randomness derives from it.
- Re‑running with same inputs and seed yields identical outputs.

Technical notes:
- Deterministic pseudorandom number generator seeded once per run; passed to scheduler.

Links:
- docs/stories/epic-3-simulation-engine.md#story-31--seeded-pseudorandom-initialization

Labels: epic:engine, type:feature


