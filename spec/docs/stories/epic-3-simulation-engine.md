# Epic 3 — Simulation Engine & Scheduling

Goal: Deterministic engine with seeded randomness where configured, scalable execution, and rich artifacts.

## Story 3.1 — Seeded pseudorandom initialization
As a system, I need to initialize a single seed so that all randomization is reproducible.

Acceptance criteria:
- Run manifest records the seed; all schedule randomness derives from it.
- Re‑running with same inputs and seed yields identical outputs.

## Story 3.2 — All‑pairs scheduler
As a system, I need a stable all‑pairs schedule so that every unique pair is simulated predictably.

Acceptance criteria:
- Generates unordered unique pairs for M actors; complexity O(M² × R).
- Ordering is stable; artifacts reflect the schedule deterministically.

## Story 3.3 — Random schedule (seeded)
As a system, I need a random scheduler so that worlds can diversify interaction orders.

Acceptance criteria:
- Supports shuffle‑all and sample‑per‑round modes; both seeded.
- Switching modes is recorded in the run manifest; outputs remain reproducible.

## Story 3.4 — Pairwise execution loop
As a system, I need to execute pairwise rounds so that strategies can interact and score.

Acceptance criteria:
- Supports Finite State Machine and Code strategies; validates emitted decisions.
- Applies payoff, updates state, and accumulates per‑actor totals each round.

## Story 3.5 — Limits and timeouts
As an operator, I need guardrails so that runs finish and resources are protected.

Acceptance criteria:
- Per‑decision step timeout and overall run timeout enforced; clear error codes.
- Maximum steps bound prevents pathological runs; failures recorded in summary.

## Story 3.6 — Artifact generation
As a user, I want run artifacts so that I can analyze results later.

Acceptance criteria:
- Writes run_summary.json, pairwise_matrix.json (or Parquet), and optional traces.
- Artifacts referenced by a manifest; signed URLs issued on request.

## Story 3.7 — Golden‑run tests
As a developer, I want golden tests so that determinism regressions are caught in continuous integration.

Acceptance criteria:
- Fixed inputs produce byte‑identical summaries and matrices; test fails on drift.


