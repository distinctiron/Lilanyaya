### Product design spec: Core user flows (concise)

- Vision
  - Platform for exploring strategies across configurable worlds; deterministic simulations; clear rankings and explainability.
  - Personas: Strategy Creator, World Creator (paid), Spectator/Researcher.

- Core model
  - Decision space (per actor): choices an actor can make each interaction (Prisoner’s Dilemma: ["C","D"]).
  - Outcome space (per interaction): joint result of two actors’ decisions (Prisoner’s Dilemma: ["C/C","C/D","D/C","D/D"]).
  - Interaction policy: all_pairs | round_robin | random | swiss; Prisoner’s Dilemma defaults to all_pairs.
  - Time: iterations (fixed in the Minimum Viable Product).
  - Determinism: fixed random number generator seed, no noise in the Minimum Viable Product; re‑seed/noise later version.
  - World state: canonical data (round, histories, totals).
  - Key Performance Indicators (derived): queries over state/outcomes (e.g., percentage of defectors, exploitation rate C/D, mutual cooperation rate, punish duration).

- Access model
  - World creation: paid-only (subscription or usage pack) with full creation/validation/analytics features included.
  - Strategy creation: free for authoring + static preview; subscriber-only quick tests and advanced explanations.
  - Spectator: full leaderboards for all; advanced analytics/replays for subscribers.

### Strategy Creator (happy path)
1) Select world
   - View rules, assumptions, seed actors, sample leaderboard.
2) Create strategy (natural language prompt)
   - Authoring mode toggle: Natural Language | Code (choose one at creation time)
   - Name, description, tags; constraints visible.
   - If Code mode: show interface contract (input/return), language selection (for example, JavaScript/TypeScript or Python), and sandbox limits.
3) Preview behavior (no compute)
   - Auto summary (state machine), static example traces, validation.
4) Quick Test (subscriber)
   - 10 simulation runs vs baselines; result card per opponent.
   - Free users see an upgrade teaser.
5) Explanations (subscriber)
   - Filters, outcome mix (C/C, C/D, D/C, D/D), pivotal rounds, saved views, export aggregates.
6) Iterate or save
   - Versioning; notes.
7) Full simulation / tournament entry
   - Consumes simulation runs; show quota and rollover; deterministic seed.
8) Publish/share
   - Public readme; link to pairwise performance summary.

### World Creator (paid-only; full features)
1) Start new world
   - Template (Prisoner’s Dilemma v1, Blank, Custom Matrix Game); metadata (name, description, visibility).
2) Define rules
   - Decision space (per actor); Outcome space (auto-derived or override).
   - Interaction policy; iterations; termination (fixed in the Minimum Viable Product).
   - Determinism: fixed seed by default (no noise in the Minimum Viable Product).
   - Actor–Strategy relationship: actor_strategy_mode = one_to_one | many_to_one
     - one_to_one (default for Prisoner’s Dilemma): each actor defines its own strategy instance
     - many_to_one: multiple actors can reference the same strategy (shared baselines/entries)
3) Scoring and assumptions
   - Payoff: matrix or function; ranking (sum | avg | discounted).
   - Public assumptions panel.
4) World state & Key Performance Indicators
   - Initial state config; built-in state update.
   - Key Performance Indicator builder: scope (world/actor/pair), filters, windows, aggregations; save indicators.
5) Seed actors
   - Baselines (AlwaysC, AlwaysD, TitForTat, Win-Stay/Lose-Shift).
   - Submission policy (open/curated/invite-only).
6) Preview (static)
   - Auto summary of rules; outcome map; sample trace (static).
7) Validate (simulation)
   - Sanity run (e.g., 10 runs) vs seeds; pairwise averages; extremes; invalid checks.
8) Publish
   - Public page; schedule tournaments; submission moderation.
9) Operate world
   - Run/auto-run simulations; analytics dashboard; Key Performance Indicator snapshots; share.

### Spectator/Researcher
1) Discover worlds
   - Featured/trending/new; filters; world cards with Key Performance Indicators.
2) World overview
   - Rules, assumptions, seeds, leaderboard summary; call to action to try a strategy.
3) Results browsing
   - Leaderboards; pairwise table; trends.
   - Free: full leaderboards; limited drilldown teaser.
4) Analytics & replays (subscriber)
   - Filters, Key Performance Indicators, pairwise drilldowns; replay explorer with event markers.
   - Export aggregated stats; save analyses.
5) Share and follow
   - Share links; follow worlds/actors/creators; bookmarks.

### Gating summary
- Strategy Creator: Steps 4–5 subscriber-only; Step 7 uses simulation runs quota (higher for subscribers).
- World Creator: paid users get all features within creation flow (no gating).
- Spectator: leaderboards free; analytics/replays subscriber-only.

### Quotas and rollover (Minimum Viable Product)
- Free: 100 simulation runs/month; rollover enabled (cap for example, three times the monthly allotment).
- Subscriptions: higher monthly runs with rollover; usage packs for add-ons.
- Private worlds: included in subs and purchasable packs.

### Trust & reproducibility
- Deterministic seeds, public world rules, reproducible logs, anti-cheat checks.

### Upgrade prompts (lightweight)
- After preview: “Run a 10‑run quick test” (subscriber call to action).
- On explanations: “Advanced analytics” (subscriber call to action).
- When exceeding sim runs: “Add runs” (usage pack) or “Upgrade plan.”


