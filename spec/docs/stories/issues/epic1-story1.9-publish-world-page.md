Title: Epic 1 — Story 1.9: Publish world page

User story:
As a world creator, I want to publish the world so that others can view and participate.

Acceptance criteria:
- Public overview includes rules, assumptions, Key Performance Indicators, leaderboards, and seeds.
- Submission policy configurable: open/curated/invite-only; enforced on submissions.

Technical notes:
- Public route `/worlds/{id}`; server filters private content; policy stored in schema_doc or separate column.

Links:
- docs/stories/epic-1-world-creator.md#story-19--publish-world-page

Labels: epic:world-creator, type:feature, area:web


