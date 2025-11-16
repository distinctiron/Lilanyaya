Title: Epic 1 — Story 1.8: Validate world (10‑run sanity)

User story:
As a world creator, I want a sanity check so that obvious issues are caught early.

Acceptance criteria:
- Triggers a 10‑run deterministic job; artifacts are stored.
- Summary view shows pairwise averages and any errors/outliers.

Technical notes:
- Enqueue a run with iterations=as configured, repeat=10; seed fixed.
- Save run_summary and pairwise_matrix; surface status.

Links:
- docs/stories/epic-1-world-creator.md#story-18--validate-world-10run-sanity
- docs/architecture/simulation-engine-and-scheduling.md

Labels: epic:world-creator, type:feature, area:engine


