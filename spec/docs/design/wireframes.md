### Lightweight wireframes – core screens

Note: ASCII/layout outline to guide design. Primary calls to action and gating states included.

Home / Discover
- Header: Logo | Worlds | Strategies | Learn | Profile
- Hero: “Explore strategy tournaments across worlds” [Browse worlds] [Create strategy]
- Sections:
  - Featured Worlds (cards): title, brief, Key Performance Indicators snapshot, entrants, [View]
  - Trending Strategies (mini list): name, world, rating, [View]
  - Getting Started: 1) Pick a world 2) Create a strategy 3) See results

World Overview
- Breadcrumbs: Worlds > {World Name}
- Left column:
  - World summary: description, assumptions
  - Rules: decision space, outcome space, interaction policy, iterations
  - Key Performance Indicators (saved): percentage of defectors, mutual cooperation, exploitation rate
- Right column:
  - Leaderboard (table): Rank | Actor | Average Score | Variance | Entries
  - Seed actors list
  - Calls to action: [Try your strategy] [View analytics (subscribers)]

Strategy Creator
- Stepper header: 1 World → 2 Define → 3 Preview → 4 Quick Test* → 5 Explain* → 6 Save → 7 Simulate
- 1 Select world: picker (cards + filters)
- 2 Define strategy:
  - Authoring mode toggle: Natural Language | Code
  - Name, description, tags
  - If Natural Language:
    - Natural language prompt editor (guidance + constraints)
    - Helper: examples (Tit For Tat, Win‑Stay/Lose‑Shift, etc.)
  - If Code:
    - Language selector (for example, JavaScript/TypeScript or Python)
    - Read‑only interface contract (input shape, allowed outputs)
    - Code editor with sandbox notes (timeouts, memory, no I/O)
- 3 Preview (no compute):
  - Behavior summary (state machine)
  - Static traces vs Always Cooperate / Always Defect (non-executable samples)
  - Validation messages
- 4 Quick Test (subscriber-gated):
  - “Run 10 simulation runs vs baselines” [Run]
  - Free state: teaser card + “Upgrade to test”
- 5 Explanations (subscriber-gated):
  - Filters: opponent(s), round window
  - Outcome mix: C/C, C/D, D/C, D/D
  - Pivotal rounds timeline
  - Free state: locked with preview snapshot
- 6 Save:
  - Version note, visibility (public/private if allowed)
  - [Save version]
- 7 Simulate:
  - Runs quota meter (with rollover)
  - Seed control (deterministic by default)
  - [Run full simulation] [Enter tournament]

World Creator (paid-only; all features enabled)
- Stepper header: 1 Template → 2 Rules → 3 Scoring → 4 State & Key Performance Indicators → 5 Seeds → 6 Preview → 7 Validate → 8 Publish
- 1 Template:
  - Cards: Prisoner’s Dilemma v1, Blank, Custom Matrix Game
  - Metadata: name, description, visibility
- 2 Rules:
  - Decision space (per actor)
  - Outcome space (auto-derived preview, editable toggle)
  - Interaction policy: all_pairs | round_robin | random | swiss
  - Time: iterations; termination (fixed for the Minimum Viable Product)
  - Determinism: random number generator seed (fixed; no noise in the Minimum Viable Product)
  - Actor–Strategy relationship:
    - Radio: One actor = one strategy (default for Prisoner’s Dilemma)
    - Radio: Many actors can share a strategy
- 3 Scoring:
  - Payoff: matrix editor or function ref
  - Ranking: sum | average | discounted (gamma)
  - Assumptions panel (public)
- 4 State & Key Performance Indicators:
  - Initial state editor (round, histories)
  - State update: built-in selector
  - Key Performance Indicator Builder:
    - Scope: world | actor | pair
    - Filters: outcome/decision, windows
    - Aggregations: counts, percentages, thresholds
    - [Save indicator]
- 5 Seed actors:
  - Add baselines (Always Cooperate, Always Defect, Tit For Tat, Win‑Stay/Lose‑Shift)
  - Submission policy: open/curated/invite-only
- 6 Preview (static):
  - Summary: rules, outcome map, sample trace
- 7 Validate (simulation):
  - “Run 10 sanity runs vs seeds”
  - Output: pairwise averages, extremes, invalid checks
- 8 Publish:
  - Public world page
  - Optional tournament scheduling

Results / Analytics
- Tabs: Leaderboard | Pairwise | Key Performance Indicators | Replays*
- Leaderboard (free):
  - Table + sorting; search actors
- Pairwise (subscriber):
  - Matrix view (actor vs actor): average payoff, outcome mix
  - Filters: cohorts, rounds window
- Key Performance Indicators (subscriber):
  - Saved indicator cards with charts and definitions
  - Compare snapshots by date/seed
- Replays (subscriber):
  - Timeline with event markers (first D, forgiveness thresholds)
  - Step controls; seed shown; [Change seed] if allowed

Profile / Billing
- Quota meters: simulation runs (with rollover cap)
- Plans: Free, Creator Basic, Creator Pro
- Usage packs: Actors, World Seed, Simulation Runs, Private Worlds
- Invoices/history

Upgrade Modals (contextual)
- Quick Test action when free: modal showing benefits of subscriber features + call to action
- Explanations locked panel: small inline upsell with sample chart
- Simulation over quota: modal to add Simulation Runs pack or upgrade plan

Empty / Error States
- No strategies yet: guidance card to create first strategy
- No worlds yet (paid): suggest World Seed pack and templates
- Invalid strategy prompt: inline error and examples

Notes for design
- Maintain deterministic labeling on results (“Seed #1234”)
- Always display world assumptions and limits near results
- Keep simulation run meters visible where relevant


