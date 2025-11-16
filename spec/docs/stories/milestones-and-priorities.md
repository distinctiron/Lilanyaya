### Milestones, Priorities, Owners, and Order (Minimum Viable Product)

Assumptions
- Owners are placeholders; adjust to your team. Suggested roles: FE (frontend), BE (backend/Application Programming Interface), ENG (engine/worker), INF (infra/ops).
- Timeboxes are indicative, assuming a small team working in parallel.

Milestone M0 — Platform Bootstrap (Week 0–1)
- Priority: P0
- Goals: Sign in, basic entitlements (stubbed), private visibility, quota check scaffold.
- Stories
  - Epic 5 (Platform): 5.1 (Sign in & profile), 5.2 (Entitlements — stub flags, no billing), 5.5 (Access control private content), 5.4 (Quota check — readout only)
    - Owners: BE+INF
  - Enablers: Project scaffolding, environments, CI, golden-run test harness skeleton
    - Owners: INF+ENG

Milestone M1 — Foundations (Weeks 1–2)
- Priority: P0
- Goals: Create worlds and strategies, run deterministic sims, basic leaderboard.
- Stories
  - Epic 1 (World Creator): 1.1, 1.2, 1.3, 1.4, 1.5
    - Owners: FE+BE
  - Epic 2 (Strategy Creator): 2.1, 2.2, 2.4
    - Owners: FE+BE
  - Epic 3 (Engine): 3.1, 3.2, 3.4
    - Owners: ENG
  - Epic 4 (Analytics): 4.1
    - Owners: BE
  - Platform (enablers): minimal auth (login only), basic quotas readout
    - Owners: BE

Milestone M2 — Simulation & Analytics Core (Weeks 3–4)
- Priority: P0
- Goals: Seeded random scheduling, artifacts, KPIs, quick test (gated).
- Stories
  - Epic 3 (Engine): 3.3, 3.6
    - Owners: ENG
  - Epic 4 (Analytics): 4.2, 4.3
    - Owners: BE
  - Epic 2 (Strategy Creator): 2.5, 2.6
    - Owners: FE+BE+ENG
  - Epic 1 (World Creator): 1.6, 1.8
    - Owners: FE+BE+ENG

Milestone M3 — Gating, Replays, and World Ops (Weeks 5–6)
- Priority: P1
- Goals: Subscriber analytics/replays, sanity flows for world operations.
- Stories
  - Epic 4 (Analytics): 4.4, 4.5
    - Owners: FE+BE
  - Epic 1 (World Creator): 1.7, 1.9
    - Owners: FE+BE
  - Epic 3 (Engine): 3.5
    - Owners: ENG

Milestone M4 — Platform & Entitlements (Weeks 7–8)
- Priority: P1
- Goals: Subscriptions, usage packs, quotas with rollover, access control.
- Stories
  - Epic 5 (Platform): 5.1, 5.2, 5.3, 5.4, 5.5
    - Owners: BE+INF
  - Epic 3 (Engine): 3.7 (Golden‑run CI)
    - Owners: ENG+INF

Milestone M5 — Advanced Strategy (Code) + Hardening (Weeks 9–10)
- Priority: P2
- Goals: Code strategies sandbox, stabilization, docs, and polish.
- Stories
  - Epic 2 (Strategy Creator): 2.3, 2.7, 2.8
    - Owners: FE+BE+ENG
  - Epic 5 (Platform): 5.6, 5.7
    - Owners: BE+INF
  - Cross‑cutting: docs, telemetry, error budgets, incident playbooks
    - Owners: ALL

Backlog (Post‑MVP candidates)
- Sponsored tournaments, team/org access, marketplace, additional worlds, API program.

Scheduling order (flat list, highest first)
1) 5.1, 5.2 (stub), 5.5, 5.4 (readout), CI scaffolds
2) 1.1, 1.2, 3.1, 3.2, 2.1, 2.2, 2.4, 4.1
3) 3.4, 1.3, 1.4, 1.5
4) 3.3, 3.6, 4.2, 4.3, 2.5, 2.6, 1.6, 1.8
5) 4.4, 4.5, 1.7, 1.9, 3.5
6) 5.2 (full billing), 5.3, 5.4 (rollover), 3.7
7) 2.3, 2.7, 2.8, 5.6, 5.7

Notes
- Revisit priorities after the first integrated demo at the end of M2.
- Keep seeds and determinism checks as non‑negotiable guardrails during all milestones.

