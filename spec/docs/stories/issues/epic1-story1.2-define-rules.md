Title: Epic 1 — Story 1.2: Define rules (decisions, outcomes, policy, iterations)

User story:
As a world creator, I want to define rules so that simulations behave as specified.

Acceptance criteria:
- Decision space is an array; outcome space auto-derived with manual override.
- Interaction policy options: all_pairs, round_robin, random; validation on change.
- Iterations must be a positive integer; invalid inputs are blocked with inline error.

Technical notes:
- Update schema_doc fields decisions.allowed, outcomes.allowed, interaction.policy, time.iterations.
- Client derives outcomes from decisions × decisions; allow override.

Links:
- docs/stories/epic-1-world-creator.md#story-12--define-rules-decisions-outcomes-policy-iterations
- docs/architecture/world-schema-and-kpis.md

Labels: epic:world-creator, type:feature


