Title: Epic 1 — Story 1.1: Create world from template

User story:
As a paid user, I want to create a world from a template so that I can start quickly with sensible defaults.

Acceptance criteria:
- Template picker offers Prisoner’s Dilemma v1, Blank, Custom Matrix Game.
- Form captures name, description, visibility (public/private) and persists.
- Redirects to world detail page after save; shows next-step guidance.

Technical notes:
- Persist to worlds table (name, description, visibility, owner_user_id, version).
- Initialize schema_doc with template defaults.

Links:
- docs/stories/epic-1-world-creator.md#story-11--create-world-from-template
- docs/architecture/world-schema-and-kpis.md

Labels: epic:world-creator, type:feature, priority:high


