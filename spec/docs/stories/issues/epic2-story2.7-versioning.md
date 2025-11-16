Title: Epic 2 — Story 2.7: Versioning

User story:
As a creator, I want to version my strategy so that I can iterate safely.

Acceptance criteria:
- Save with notes creates a new immutable version; list and revert supported.
- Published link points to latest by default; specific versions linkable.

Technical notes:
- strategies.version increments; run_actors.snapshot stores version used.

Links:
- docs/stories/epic-2-strategy-creator.md#story-27--versioning

Labels: epic:strategy-creator, type:feature


