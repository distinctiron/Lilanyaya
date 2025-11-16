### Services, Application Programming Interfaces, and Deployment (Minimum Viable Product)

Objectives
- Define service boundaries, Application Programming Interface surfaces, and deployment topology for a reliable, scalable Minimum Viable Product.

Services (logical)
- Web Application
  - Next.js/React Single Page Application served behind a Content Delivery Network.
  - Consumes the Application Programming Interface Gateway; handles auth flows and renders views.
- Application Programming Interface Gateway
  - Choice: REST (Fastify/FastAPI) or Graph Query Language (NestJS/GraphQL).
  - Responsibilities: authentication/authorization, validation, routing to domain services, enqueueing simulation jobs, returning summaries.
- Simulation Worker Service
  - Stateless workers consuming a queue.
  - Executes deterministic simulations with seeded randomness (as configured), writes artifacts to object storage, updates run status.
- Analytics Service (optional initially)
  - Computes heavy aggregations not included in run summaries, precomputes pairwise matrices for hot worlds, maintains caches.
- Billing/Entitlements Service (can live inside the Application Programming Interface Gateway for Minimum Viable Product)
  - Tracks subscriptions and usage packs, enforces quotas/rollover, exposes feature flags to the Application Programming Interface Gateway.

Core REST endpoints (illustrative)
- Worlds
  - GET /worlds?query=...&visibility=...
  - GET /worlds/{world_id}
  - POST /worlds (paid-only; body: world definition)
  - PATCH /worlds/{world_id}
- Strategies
  - GET /worlds/{world_id}/strategies
  - POST /worlds/{world_id}/strategies (compile + validate, returns Finite State Machine spec)
  - GET /strategies/{strategy_id}
- Actors
  - GET /worlds/{world_id}/actors
  - POST /worlds/{world_id}/actors (body: strategy reference + params)
- Runs
  - POST /runs (body: world_id, actor_ids[], iterations, policy?, seed?, options?)
  - GET /runs/{run_id} (status, compact summary, artifact_manifest_url)
  - GET /runs/{run_id}/artifacts/summary (proxy or signed URL)
- Leaderboards and Analytics
  - GET /worlds/{world_id}/leaderboard (latest)
  - GET /worlds/{world_id}/pairwise?run_id=...
  - GET /worlds/{world_id}/kpis?run_id=...
  - GET /worlds/{world_id}/replays?run_id=...&pair=actorA,actorB (if stored)
- Billing/Entitlements
  - GET /me/entitlements
  - POST /subscriptions (create/update via provider webhook)
  - GET /usage (simulation runs remaining, rollover)

Authentication and authorization
- Authentication: OAuth (Google/GitHub) + email magic link fallback.
- Authorization:
  - Role flags and per-resource ownership checks.
  - World visibility: public/private; private requires owner or whitelisted access.
  - Feature flags from entitlements (subscriber analytics, private worlds, limits).

Domain modules (Application Programming Interface Gateway)
- worlds: create/update/publish, validation against world schema, manage actor_strategy_mode and randomness settings.
- strategies: prompt → Finite State Machine compile, validation, versioning.
- actors: add/remove actors, enforce actor_strategy_mode constraints.
- runs: enqueue/poll, quota checks, signed artifact URLs.
- analytics: leaderboards, pairwise, Key Performance Indicator snapshots retrieval.
- billing: subscriptions, packs, quota and rollover accounting.

Queues and workers
- Queue: Redis (BullMQ) or RabbitMQ.
- Job envelope: { run_id, world_ref, actor_refs, iterations, policy, seed, options }.
- Worker scaling: horizontal autoscale based on queue depth and Service Level Objective.

Storage
- Relational database: Postgres (managed).
- Cache: Redis.
- Object storage: Amazon Simple Storage Service compatible (artifacts per run; manifest, summary, matrices, traces).
- Content Delivery Network for static assets and Web Application.

Deployment topology (baseline)
- One Virtual Private Cloud with subnets for Application Programming Interface, workers, database, and cache.
- Managed services where possible (Postgres, Redis, object storage).
- Containers:
  - Web Application: stateless, multiple replicas behind a load balancer.
  - Application Programming Interface: stateless, multiple replicas behind a load balancer.
  - Workers: autoscaled deployment (queue consumer).
- Observability:
  - Centralized logging, metrics (Application Programming Interface latency, worker throughput), traces for key flows.

Security
- All endpoints over Transport Layer Security; cookies HttpOnly/SameSite.
- Signed URLs for artifact access (time-limited).
- Input validation at the Application Programming Interface boundary; schema validation for worlds and strategies.
- Deterministic seeds recorded in manifests; reproducibility checks for audits.

Continuous delivery
- Branch-based deployments to staging; migrations run with gating.
- Blue/green or rolling updates; health checks on Application Programming Interface and worker liveness.
- Seeded “golden run” tests in CI to detect nondeterministic regressions.

Cloud cost and scaling notes
- Scale workers independently from the Application Programming Interface.
- Limit artifact size; prefer columnar format for large pairwise matrices.
- Cache hot leaderboards; expire aggressively.


