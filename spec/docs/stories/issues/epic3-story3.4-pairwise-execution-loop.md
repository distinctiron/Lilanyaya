Title: Epic 3 — Story 3.4: Pairwise execution loop

User story:
As a system, I need to execute pairwise rounds so that strategies can interact and score.

Acceptance criteria:
- Supports Finite State Machine and Code strategies; validates emitted decisions.
- Applies payoff, updates state, and accumulates per‑actor totals each round.

Technical notes:
- Tight inner loop; reuse buffers; per‑decision timeout enforced.

Links:
- docs/stories/epic-3-simulation-engine.md#story-34--pairwise-execution-loop

Labels: epic:engine, type:feature


