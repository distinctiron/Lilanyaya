Title: Epic 4 — Story 4.2: Key Performance Indicator snapshots

User story:
As a world creator, I want Key Performance Indicator snapshots so that I can track trends across runs.

Acceptance criteria:
- Key Performance Indicators evaluated per run, stored and listed chronologically.
- Compare view shows last N runs with deltas.

Technical notes:
- Compute after run completion; persist to kpi_snapshots; expose via GET /worlds/{id}/kpis?run_id=...

Links:
- docs/stories/epic-4-analytics.md#story-42--key-performance-indicator-snapshots

Labels: epic:analytics, type:feature


