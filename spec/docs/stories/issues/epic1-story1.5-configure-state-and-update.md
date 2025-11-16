Title: Epic 1 — Story 1.5: Configure state and state update

User story:
As a world creator, I want to configure state so that the engine can track histories and rounds.

Acceptance criteria:
- Initial state shows round and history defaults; editable JSON with schema validation.
- Built‑in update selector (pd.update_state) selectable; custom updates disabled (future).

Technical notes:
- Update state.initial and state.update in schema_doc; perform JSON schema validation on save.

Links:
- docs/stories/epic-1-world-creator.md#story-15--configure-state-and-state-update
- docs/architecture/world-schema-and-kpis.md

Labels: epic:world-creator, type:feature


