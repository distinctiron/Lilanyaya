### Simulation Engine and Scheduling (Minimum Viable Product)

Objectives
- Deterministic, reproducible simulations for configurable worlds (beginning with Prisoner’s Dilemma), with seeded randomness where configured.
- Efficient scheduling of pairings and rounds with clear aggregation outputs.
- Strict safety: no user code execution; bounded resources per run.

Core responsibilities
1) Load validated world definition and strategy specifications.
2) Construct the schedule of interactions (pairings × rounds).
3) Execute steps deterministically using a seeded pseudorandom number generator only where the world allows randomness (disabled in the Minimum Viable Product).
4) Aggregate results: per‑actor scores, pairwise metrics, saved Key Performance Indicators, and optional traces.
5) Persist artifacts and emit events for downstream consumers (leaderboards, analytics).

Determinism and randomness model
- Seeded pseudorandom number generator: numeric seed recorded in the run manifest; all randomization derived from this seed to ensure reproducibility.
- Pure functional decision path: strategies are Finite State Machine based; given the same inputs they produce the same outputs (strategies remain deterministic).
- Schedule randomization: when the world’s policy requests randomness, pair order (and per-round matching if applicable) is produced by seeded shuffles.
- Evaluation order within a pairing remains fixed (Actor A then Actor B).
- Optional noise hooks are reserved (disabled by default in the Minimum Viable Product).

Scheduling policies (world interaction policy)
- All pairs (default example):
  - For M actors, construct all unordered unique pairs (A,B) where A ≠ B.
  - Each pair plays R rounds (iterations).
  - Complexity: O(M² × R).
- Round robin:
  - Organize actors into rounds so each actor plays once per round.
  - Each pairing repeats across many rounds until R is met.
- Random schedule (supported in the Minimum Viable Product):
  - Sample or shuffle pairs per round using the seeded pseudorandom number generator.
  - Two modes (configurable):
    - Shuffle‑all: build all pairs then apply a seeded shuffle to order of execution.
    - Sample‑per‑round: for each round, draw pairs without replacement using the seeded generator.
- Swiss or tournament variants (future).

Execution loop (per run)
1) Initialize run context
   - Load world, strategies, and seed.
   - Build schedule: list of pairings with R iterations each.
   - Initialize world state and per‑actor accumulators.
2) For each pairing (Actor A, Actor B)
   - Initialize pairing state (last decisions, counters).
   - Repeat for R iterations:
     a) Query Actor A strategy with observations → decision_A.
     b) Query Actor B strategy with observations → decision_B.
     c) Compute outcome from decisions (for example, “C/D”).
     d) Apply payoff evaluator to outcome → rewards [r_A, r_B].
     e) Update world state, pairing state, and accumulators.
     f) If trace logging is enabled, append compact round trace.
3) After all pairings complete
   - Aggregate per‑actor totals and averages.
   - Compute pairwise tables (average payoff, outcome mix, first defection round, punish duration).
   - Evaluate configured Key Performance Indicators over outcomes and state.
4) Persist and publish
   - Store summary document (JSON) with seed, parameters, aggregates.
   - Store pairwise matrix and any trace samples.
   - Emit run‑completed event with references to artifacts.

Data produced per run
- Run summary (JSON):
  - run_id, world_id, strategy_ids[], seed, iterations, policy, created_at
  - per_actor: { actor_id: { total, average, variance, pairwise_refs[] } }
  - ranking: sorted list of actor_id by average (descending)
  - kpi_snapshots: { indicator_id: value, … }
- Pairwise matrix (JSON or columnar):
  - rows: actor A, columns: actor B
  - values: { average_payoff_A, outcome_counts: { "C/C": n, "C/D": n, "D/C": n, "D/D": n }, first_defection_round, punish_duration_average }
- Optional traces (sampled):
  - For selected pairs: list of rounds with decisions, state transitions, and events.

Resource limits and fairness
- Hard bounds per run:
  - Maximum actors per run (configurable by plan).
  - Maximum iterations per pairing.
  - Maximum total steps = number_of_pairs × iterations, enforced at worker level.
- Timeouts:
  - Per decision step timeout (to avoid pathological guard evaluation).
  - Overall run timeout with graceful abort and partial artifacts.
- Quotas:
  - Simulation run quotas per plan with rollover cap; validated before enqueueing.

Worker implementation
- Interface
  - Input envelope: { run_id, world_ref, strategy_refs[], policy, iterations, seed, options }
  - Output: artifacts persisted to object storage, run status updated in database.
- Language
  - Same language as the Application Programming Interface to share models (for example, TypeScript or Python).
- Parallelization
  - Outer loop over pairings: chunked into batches (configurable batch size).
  - Inner loop (rounds): tight numeric loop; avoid allocations; reuse buffers.
- Error handling
  - Validation failures → fail fast with clear error code.
  - Non‑fatal anomalies (for example, invalid outcome code) → log and mark run invalid.

Artifact storage
- Object storage (for example, Amazon Simple Storage Service compatible) for:
  - run_summary.json
  - pairwise_matrix.json (or Parquet for larger matrices)
  - traces/*.json (if enabled)
- Database (Postgres) stores:
  - run metadata (foreign keys to world and strategies)
  - compact aggregates for fast leaderboard queries

Scalability
- Horizontal workers with a queue (for example, Redis backed).
- Backpressure and prioritization:
  - Paid plans and sponsored tournaments receive higher priority lanes.
  - Large runs are split into batches with progress checkpoints.
- Caching:
  - Hot leaderboards and recent runs cached in Redis with short time to live.

Testing strategy
- Golden runs:
  - For a fixed world, strategies, seed, and iterations, expected aggregates are stored.
  - Worker changes must reproduce the same outputs bit‑for‑bit.
- Property tests:
  - Payoff matrix symmetry where applicable; bounds on averages.
- Performance tests:
  - Measure throughput with increasing actor counts and iterations.

Operational metrics
- Queue depth, average wait time, runs per minute.
- Worker utilization, error rates, timeouts.
- Average time per pair and per decision step.
- Storage growth for artifacts.

Future extensions
- Randomized schedules and noise models with explicit parameterization.
- Progressive tournament formats (Swiss, elimination).
- Checkpointing and resume for very large runs.
- Hardware acceleration (for example, vectorized inner loops) if profiling warrants.


