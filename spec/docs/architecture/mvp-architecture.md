### Minimum Viable Product Technical Architecture (concise)

- Deterministic, scalable simulations for configurable worlds (Prisoner’s Dilemma first), with seeded randomness where configured.
- Natural‑language strategy authoring compiled to a safe, testable strategy.
- Clear rankings, explainability, and saved Key Performance Indicators.

High‑level components
- Web Application (Single Page Application)
  - Next.js/React (or similar)
  - Views: Discover, World Overview, Strategy Creator, World Creator, Results/Analytics, Profile/Billing
  - Auth: OAuth (GitHub/Google) + email; role flags (free, subscriber, paid world creator)
- Application Programming Interface Gateway (Graph Query Language or Representational State Transfer)
  - Queries: worlds, strategies, leaderboards, KPIs
  - Mutations: create/edit worlds, strategies, runs, tournaments
  - Job submission: enqueue simulations; poll results
- Strategy Compiler
  - Input: natural language prompt + world contract → strategy specification
  - Output: constrained Strategy Domain‑Specific Language or Finite State Machine
  - Validation: schema checks, determinism checks, resource bounds
  - Sandbox: interpret the constrained representation (no user code execution in the Minimum Viable Product)
- World Engine
  - World schema loader (YAML/JSON)
  - Interaction policy (pairing, schedule)
  - Randomization controls (seeded ordering, optional shuffles)
  - Outcome mapper (decision → outcome)
  - Payoff evaluator (matrix/function)
  - State manager (round, histories, totals)
  - Key Performance Indicator runner (registered queries)
- Simulation Service (Workers)
- Deterministic pseudorandom number generator (seeded) for schedule randomization and other permitted randomness
  - Batch scheduler (N rounds × pairings)
  - Aggregation: per‑actor scores, pairwise matrices, Key Performance Indicator snapshots
  - Outputs: run artifacts (summaries, traces if enabled)
  - Queue: Redis/RabbitMQ (BullMQ/Celery)
- Analytics/Explainability
  - Pairwise tables, outcome distributions, pivotal‑event detection
  - Replay sampling (seeded) with event markers
  - Key Performance Indicator snapshots/time series per world
- Storage
  - Postgres (core metadata): users, worlds, strategies, runs, leaderboards, Key Performance Indicators
  - Object storage (artifacts): run summaries, pair matrices, replay samples (JSON/Parquet)
  - Cache: Redis for hot leaderboards and run status
- Billing & Entitlements
  - Plans: free, Creator Basic/Pro; usage packs (actors, world seed, sim runs, private worlds)
  - Meters: simulation runs (with rollover cap), actors/worlds quotas
  - Feature flags: subscriber analytics access

World schema v1 (Prisoner’s Dilemma example)
- metadata: name, description, version, visibility
- decisions.allowed: ["C","D"]
- outcomes.allowed: auto from decisions × decisions (override optional)
- interaction.policy: all_pairs | round_robin | random | swiss (Minimum Viable Product supports random)
- time.iterations: int (Minimum Viable Product fixed)
- determinism.rng_seed: int (Minimum Viable Product required)
- randomness:
  - schedule_randomization: boolean (default true if interaction.policy == "random")
  - noise: object (reserved for future; default disabled)
- payoff:
  - type: matrix | function_ref
  - matrix: {"C":{"C":[3,3],"D":[0,5]},"D":{"C":[5,0],"D":[1,1]}}
- scoring:
  - aggregate: sum | avg | discounted
  - ranking: descending
- state:
  - initial: { round: 0, history: {} }
  - update: builtin.ref (e.g., pd.update_state)
- kpis: list of saved Key Performance Indicator definitions (scope, filters, windows, aggregations)

Strategy model (constrained representation)
- Identity: id, name, description, tags, world_id
- Type: Finite State Machine with named states, transitions on observations (opponent last action, streaks, round)
- Actions: map state → decision in decisions.allowed
- Guards: “since last opponent D, cooperations >= 3” etc.
- Constraints: no randomness in the Minimum Viable Product (unless world explicitly allows)
- Example (Strategy A – ForgiveAfter3Coops):
  - states: START, PUNISH, COOP
  - START: action C → if opponent D → PUNISH
  - PUNISH: action D → if consecutive_opponent_C >= 3 → COOP
  - COOP: action C → if opponent D → PUNISH

Simulation execution (deterministic with seeded randomness)
1. Load world + strategies; verify contracts.
2. Seed pseudorandom number generator (rng_seed).
3. Schedule interactions (policy); when random policy is chosen, use seeded shuffles for reproducibility.
4. For each pairing and round:
   - Query strategies (Finite State Machine step) → decisions
   - Map to outcome; apply payoff
   - Update world/actor state (history, round, totals)
5. Aggregate results:
   - Per‑actor averages, pairwise metrics, Key Performance Indicators
6. Persist artifacts; compute rankings; emit events

Application Programming Interfaces (illustrative)
- GET /worlds, GET /worlds/{id}, GET /worlds/{id}/leaderboard
- POST /strategies (compile+validate), GET /strategies/{id}
- POST /runs (world_id, strategy_ids[], iterations, seed?)
- GET /runs/{id} (status, summary, artifacts)
- GET /analytics/worlds/{id}/pairwise, /kpis, /replays (subscriber)

Security & safety
- No user code execution in the Minimum Viable Product; only the constrained representation interpreted by the engine
- Timeouts and step limits per decision
- Schema validation for worlds and strategies
- Deterministic seeds; signed run manifests; auditable logs

Scalability & cost
- Horizontally scale workers; batch pairings
- Persist large outputs to object storage; summarize in Postgres
- Rollover cap to bound long‑term liabilities
- Backpressure: queued runs with prioritization by plan

Deployment
- Containerized services (Docker)
- Minimal stack:
  - Web: Next.js
  - Application Programming Interface: Node/TypeScript (NestJS/Fastify) or Python (FastAPI)
  - Workers: Node or Python (same language as the Application Programming Interface to share models)
  - Database: Postgres
  - Queue: Redis
  - Object store: Amazon Simple Storage Service compatible

Minimum Viable Product milestones
1. World schema v1 + Prisoner’s Dilemma builtin world + baselines
2. Strategy Domain‑Specific Language / Finite State Machine + compiler (prompt → Finite State Machine) + validator
3. Simulation engine (pairings, payoff, scoring, artifacts)
4. Leaderboard + pairwise metrics + basic Key Performance Indicators
5. Strategy Creator (authoring, static preview; gated quick test)
6. World Creator (paid; full features) + sanity validate
7. Spectator leaderboards; subscriber analytics (filters, pairwise, replay)

Future
- Noise/reseeding; progressive tournament formats
- Marketplace for worlds/strategy packs; sponsored challenges
- Application Programming Interface program; enterprise/private deployments


