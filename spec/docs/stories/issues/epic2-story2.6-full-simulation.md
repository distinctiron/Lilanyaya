Title: Epic 2 — Story 2.6: Full simulation

User story:
As a creator, I want to run full simulations so that I can get ranked, reproducible results.

Acceptance criteria:
- User selects iterations, seed, and policy (respecting world settings); quotas enforced.
- Artifacts (summary, pairwise, traces if enabled) are persisted and linked.

Technical notes:
- POST /runs enqueues; GET /runs/{id} returns status and summary; artifacts via signed URLs.

Links:
- docs/stories/epic-2-strategy-creator.md#story-26--full-simulation
- docs/architecture/simulation-engine-and-scheduling.md

Labels: epic:strategy-creator, type:feature, area:engine


