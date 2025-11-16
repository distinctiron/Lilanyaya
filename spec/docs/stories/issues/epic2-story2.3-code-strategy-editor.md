Title: Epic 2 — Story 2.3: Code strategy editor (sandboxed)

User story:
As an advanced creator, I want to implement a constrained function so that I can express behavior as code safely.

Acceptance criteria:
- Editor shows interface contract (input shape and allowed outputs) and language selector.
- Save runs static checks (signature, forbidden globals/imports, size).
- Runtime enforces timeout and memory limits; invalid outputs are rejected with clear errors.

Technical notes:
- Sandboxed runtime (e.g., isolated VM or subprocess) with no I/O; deterministic inputs.

Links:
- docs/stories/epic-2-strategy-creator.md#story-23--code-strategy-editor-sandboxed
- docs/architecture/strategy-dsl-and-sandbox.md#code-strategy-interface-optional-sandboxed

Labels: epic:strategy-creator, type:feature, area:sandbox


