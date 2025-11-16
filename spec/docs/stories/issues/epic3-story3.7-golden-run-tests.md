Title: Epic 3 — Story 3.7: Golden‑run tests

User story:
As a developer, I want golden tests so that determinism regressions are caught in continuous integration.

Acceptance criteria:
- Fixed inputs produce byte‑identical summaries and matrices; test fails on drift.

Technical notes:
- Store golden artifacts in test fixtures; add CI step to compare outputs.

Links:
- docs/stories/epic-3-simulation-engine.md#story-37--goldenrun-tests

Labels: epic:engine, type:test


