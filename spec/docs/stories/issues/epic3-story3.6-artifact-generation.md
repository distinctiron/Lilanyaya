Title: Epic 3 — Story 3.6: Artifact generation

User story:
As a user, I want run artifacts so that I can analyze results later.

Acceptance criteria:
- Writes run_summary.json, pairwise_matrix.json (or Parquet), and optional traces.
- Artifacts referenced by a manifest; signed URLs issued on request.

Technical notes:
- Store to object storage under /runs/{run_id}/; record checksums and sizes.

Links:
- docs/stories/epic-3-simulation-engine.md#story-36--artifact-generation

Labels: epic:engine, type:feature, area:storage


