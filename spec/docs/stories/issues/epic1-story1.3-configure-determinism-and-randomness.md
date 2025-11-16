Title: Epic 1 — Story 1.3: Configure determinism and randomness

User story:
As a world creator, I want deterministic seeds and optional schedule randomization so that runs are reproducible yet flexible.

Acceptance criteria:
- rng_seed is required and persisted.
- schedule_randomization toggle appears when policy is random; uses seeded shuffles.
- World schema preview reflects chosen settings.

Technical notes:
- Update determinism.rng_seed and randomness.schedule_randomization in schema_doc.
- Simulation manifest records seed and randomization mode.

Links:
- docs/stories/epic-1-world-creator.md#story-13--configure-determinism-and-randomness
- docs/architecture/simulation-engine-and-scheduling.md

Labels: epic:world-creator, type:feature


