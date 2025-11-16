Title: Epic 3 — Story 3.2: All‑pairs scheduler

User story:
As a system, I need a stable all‑pairs schedule so that every unique pair is simulated predictably.

Acceptance criteria:
- Generates unordered unique pairs for M actors; complexity O(M² × R).
- Ordering is stable; artifacts reflect the schedule deterministically.

Technical notes:
- Deterministic ordering by actor id; avoid duplicates (A,B) vs (B,A).

Links:
- docs/stories/epic-3-simulation-engine.md#story-32--allpairs-scheduler

Labels: epic:engine, type:feature


