# Product Requirements Document (PRD) — lilanyaya

Status: Draft  
Owner: Sujeevan (Product)  
Last updated: {{date}}

## 1. Product vision and context

Prana is a platform to explore strategies across configurable worlds using deterministic (seeded) simulations, clear rankings, and explainability. It lets:
- World Creators define “worlds” (rules, decision and outcome spaces, interaction policy, payoff, schedule, state, Key Performance Indicators).
- Strategy Creators author strategies in natural language that compile to a safe Finite State Machine model, preview them, and run simulations or enter tournaments.
- Spectators/Researchers browse leaderboards, analytics, and (for subscribers) replays and pairwise analyses.

Initial flagship world: Prisoner’s Dilemma (repeated). The platform is world‑agnostic and will support new game types and parameterized variants.

Magic moment: seeing a strategy’s behavior explained through pairwise outcomes, pivotal rounds, and KPIs, then iterating instantly to improve it.

References:  
- docs/design/user-flows-spec.md  
- docs/design/wireframes.md  
- docs/architecture/mvp-architecture.md  
- docs/architecture/world-schema-and-kpis.md  
- docs/architecture/strategy-dsl-and-sandbox.md  
- docs/architecture/simulation-engine-and-scheduling.md  
- docs/architecture/services-apis-and-deployment.md

## 2. Goals and success criteria

Success is learning/product validation first, growth second:
- Activation: time‑to‑first‑result (user creates one actor and views results) < 5 minutes.
- Engagement: median of 2+ strategy iterations per active creator session.
- Robustness: identical results for identical seeds and inputs (deterministic).
- Conversion (later): baseline free→paid conversion driven by analytics/replays gating.

Business‑model guardrails (pricing to be finalized later):
- Free tier emphasizes acquisition and virality (one actor per world; 100 monthly simulation runs; rollover cap).
- World creation is paid; all creation/validation/analytics are included for paid creators.
- Seeded randomness available in Minimum Viable Product for schedule randomization; strategies remain deterministic.

## 3. Users and key jobs‑to‑be‑done

1) World Creator (paid)
- Define world rules and assumptions.
- Seed baseline actors.
- Validate world behavior with sanity runs.
- Publish and operate worlds; see analytics and KPIs.

2) Strategy Creator
- Author strategy in natural language.
- Preview behavior statically; (subscriber) quick test and explanations.
- Run full simulations; enter tournaments; iterate versions.

3) Spectator/Researcher
- Discover worlds and results.
- View leaderboards; (subscriber) drilldowns, KPIs, and replays.

## 4. Scope (Minimum Viable Product)

Must‑have
- World Creator (paid): create, validate (10‑run sanity), seed baselines, publish; KPI builder and analytics dashboard.
- Strategy Creator: NL authoring → FSM compile; static preview; (subscriber) quick test and explanations; run full simulations; publish strategy.
- Spectator: discovery, world overview, leaderboards (free); analytics and replays (subscriber).
- Simulation engine: deterministic with seeded randomness for schedule randomization; artifacts (summary, pairwise matrix, optional traces).
- World schema v1: decision/outcome spaces, interaction policy (all_pairs, round_robin, random), iterations, determinism, randomness, payoff, scoring, state, KPIs, actor_strategy_mode.

Nice‑to‑have (stretch)
- Replay explorer with event markers for more worlds beyond Prisoner’s Dilemma.
- Sponsored tournaments scaffolding.
- Saved analyses sharing.

Out‑of‑scope (now)
- Complex marketplace, enterprise deployments, or custom noise models beyond seeded schedule randomization.

## 5. Functional requirements

5.1 World creation
- Define rules: decision space, outcome space (auto‑derived with override), interaction policy, iterations, determinism seed, randomness settings, payoff (matrix/function), scoring, state update, assumptions.
- Actor–strategy relationship: actor_strategy_mode = one_to_one (default for Prisoner’s Dilemma) or many_to_one.
- Seed baseline actors (Always Cooperate, Always Defect, Tit For Tat, Win‑Stay/Lose‑Shift).
- Validate (10‑run sanity) and publish world page.

5.2 KPI builder and analytics
- Define KPIs (scope, filters, windows, aggregations); save definitions per world.
- Display KPI snapshots for each run; compare recent runs.
- Pairwise matrix: average payoff, outcome distribution, first defection, punish duration.

5.3 Strategy authoring and simulation
- Authoring modes (choose at creation time):
  - Natural language → compile to Finite State Machine (no user code)
  - Code → implement constrained interface in a sandboxed runtime (strict limits; must return allowed decisions)
- Static preview: behavior summary and validation (no compute).
- Subscriber quick test: 10 simulation runs vs baselines; mini‑cards per opponent.
- Full simulation: choose iterations/seed/policy per world; persist artifacts and update leaderboards.
- Versioning: save strategy versions with notes; publish.

5.4 Spectator experience
- Discover: featured/trending/new worlds with KPI snapshots.
- World overview: rules, assumptions, seed actors, leaderboards.
- Analytics & replays (subscriber): filters, pairwise drilldowns, KPI panels, replay samples (seeded).

## 6. Non‑functional requirements

Determinism and reproducibility
- All randomization is derived from recorded seeds; identical inputs → identical outputs.
- Strategies are deterministic; seeded randomness applies to scheduling where configured.

Performance and scalability
- Workers process simulations in batches of pairings; results stored to object storage.
- Hot leaderboards cached; pairwise matrices may use columnar storage for scale.

Security and safety
- Strategy execution modes:
  - Constrained Finite State Machine interpreter (default, safe, fully explainable)
  - Optional user code strategies via a constrained interface executed in a secure sandbox with strict time/memory limits, deterministic inputs, and required return types (must emit only allowed decisions). Sandboxed execution is audited and can be disabled per world.
- Signed URLs for artifacts; private worlds/strategies enforce access control.

Reliability
- Golden‑run tests in continuous integration to detect nondeterministic regressions.
- Timeouts per decision and per run; bounded steps.

## 7. KPI definitions (examples)

- Mutual cooperation rate: percentage of rounds that are C/C.
- Percentage of defectors: actors with >50% D decisions.
- Exploitation rate: count of C/D outcomes in last N rounds.
- Punish duration: average rounds spent in D/D after first D.

## 8. Release plan (phased)

Phase 1 (Foundations)
- World schema v1 with Prisoner’s Dilemma; baseline actors; KPI builder.
- Strategy compiler and interpreter; static preview; quick test (subscriber).
- Deterministic engine with seeded schedule randomization; artifacts; leaderboards.
- Gating and quotas (free vs subscriber; paid‑only world creation).

Phase 2 (Depth)
- Replays with event markers; saved analyses; additional worlds.
- Sponsored tournaments scaffold; basic moderation.

## 9. Risks and mitigations

- Cold start (few worlds/strategies): ship high‑quality built‑ins and featured tournaments.
- Cost spikes: quotas, rollover caps, batching; prioritize subscribers.
- Trust: determinism, public rules, reproducible logs, anti‑cheat checks.
- Scope creep: keep Minimum Viable Product guardrails; defer marketplace and complex noise.

## 10. Open decisions

- Final subscription tiers and quotas (settle post‑MVP usage data).
- Additional default KPIs per world type.
- Tournament formats beyond “all pairs” and “round robin.”

---  
End of PRD (Draft). Please comment inline and mark sections for refinement. 


