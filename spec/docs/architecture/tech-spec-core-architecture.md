### Technical Specification — Core Architecture (MVP)

Scope
- Focus: Core service boundaries, data flows, interfaces, and operational concerns for Minimum Viable Product.
- Inputs: PRD, user flows, architecture docs already in repo.

System overview
- Web Application (Next.js/React) calls Application Programming Interface Gateway.
- Application Programming Interface Gateway validates/authz, persists metadata (Postgres), enqueues simulation jobs (Redis/RabbitMQ).
- Simulation Workers execute deterministic runs with seeded randomness, emit artifacts to object storage (Amazon Simple Storage Service compatible), update run status/aggregates (Postgres).
- Optional Analytics Service can precompute heavy views (later).

Core components
1) Web Application
  - Pages: Worlds, Strategies, Results/Analytics, Profile/Billing.
  - Auth: OAuth + email; secure cookies.
  - Caching: Client‑side + Content Delivery Network for static assets.
2) Application Programming Interface Gateway (REST)
  - Modules: auth, worlds, strategies, actors, runs, analytics, billing.
  - Enforcement: entitlements (subscriptions, packs), quotas, visibility.
  - Integration: payment provider webhooks for subscriptions/packs.
3) Simulation Workers
  - Deterministic pseudorandom number generator seeded per run.
  - Schedulers: all_pairs; seeded random (shuffle‑all, sample‑per‑round).
  - Executors: Finite State Machine interpreter and sandboxed code strategies.
  - Artifacts: run_summary.json, pairwise_matrix.json(.parquet), traces/.
  - Guardrails: per‑decision timeout, run timeout, max steps.
4) Storage
  - Postgres: users, worlds, strategies, actors, runs, leaderboards, Key Performance Indicators, subscriptions, purchases.
  - Redis: queue + hot caches (leaderboards).
  - Object storage: artifacts per run; signed URL delivery.

Key data flows
1) Create world
  - POST /worlds → validate schema → persist → return world_id.
  - Optional: 10‑run sanity → enqueue → artifacts → status update.
2) Create strategy
  - POST /worlds/{id}/strategies (mode=Natural Language|Code).
  - Natural Language: prompt → Finite State Machine compile → preview JSON.
  - Code: static checks → store source → preview metadata (no execute).
3) Quick Test (subscriber)
  - POST /runs (small iterations vs baselines; seeded) → status → summary.
4) Full simulation
  - POST /runs → enqueue → workers produce artifacts → update leaderboards, Key Performance Indicator snapshots.
5) Analytics
  - GET /worlds/{id}/leaderboard (cached), /pairwise, /kpis, /replays (signed artifacts).

Interfaces (representative)
- POST /worlds
  - body: { name, description, visibility, schema_doc }
  - 201: { world_id }
- POST /worlds/{world_id}/strategies
  - body: { name, mode: "Finite State Machine"|"Code", source|prompt }
  - 201: { strategy_id, preview }
- POST /runs
  - body: { world_id, actor_ids[], iterations, policy?, seed?, options? }
  - 202: { run_id, status: "queued" }
- GET /runs/{run_id}
  - 200: { status, summary?, artifact_manifest_url? }

Domain models (high level)
- World: schema_doc { decisions, outcomes, interaction, time, determinism, randomness, payoff, scoring, state, actor_strategy_mode, kpis }.
- Strategy: { type: "Finite State Machine"|"Code", spec|source, version, visibility }.
- Run: { seed, iterations, policy, status, summary, artifacts }.

Determinism and randomness
- Single seed recorded in run manifest; all schedule randomness derived from it.
- Strategies must be deterministic given inputs; sandboxed code has no non‑deterministic sources.

Security and safety
- No network/file access in worker execution sandboxes.
- Signed URLs for artifacts; resource‑level authorization on Application Programming Interface.
- Webhooks verified and idempotent; audit logs for artifact access.

Operational concerns
- Autoscale workers on queue depth; separate worker tiers for large jobs.
- Observability: logs, metrics (Application Programming Interface latency, worker throughput), traces for run lifecycle.
- Golden‑run CI tests ensure reproducibility across changes.

Risks and mitigations
- Cost spikes: quotas + rollover caps; batch scheduling; cache hot views.
- Nondeterminism: enforce seeds everywhere; block non‑deterministic APIs; golden tests.
- Security: strict sandbox; input validation; least‑privilege storage access.

Phased implementation plan
1) Application Programming Interface + DB scaffolding (users/worlds/strategies/runs).
2) Finite State Machine pipeline (compile, validate, interpret) + all_pairs scheduler.
3) Run artifacts + leaderboards + Key Performance Indicator snapshots.
4) Seeded random scheduler modes; subscriber quick test and analytics gating.
5) Code strategy sandbox (single language) with strict limits.

