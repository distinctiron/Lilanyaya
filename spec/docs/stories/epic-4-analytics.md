# Epic 4 — Analytics (Key Performance Indicators, Pairwise, Replays)

Goal: Provide leaderboards, saved Key Performance Indicators, pairwise analysis, and replay sampling with gating.

## Story 4.1 — Leaderboard (latest)
As a spectator, I want a leaderboard so that I can see top performers at a glance.

Acceptance criteria:
- Sorted by average score; shows variance and entries count.
- Cached for fast loads; links to underlying run details.

## Story 4.2 — Key Performance Indicator snapshots
As a world creator, I want Key Performance Indicator snapshots so that I can track trends across runs.

Acceptance criteria:
- Key Performance Indicators evaluated per run, stored and listed chronologically.
- Compare view shows last N runs with deltas.

## Story 4.3 — Pairwise matrix Application Programming Interface
As a researcher, I want pairwise metrics so that I can analyze interactions between actors.

Acceptance criteria:
- Returns average payoff, outcome distribution (C/C, C/D, D/C, D/D), first defection round, punish duration.
- Supports filtering by cohorts and round windows (subscriber).

## Story 4.4 — Replays (subscriber)
As a subscriber, I want seeded replays so that I can inspect pivotal moments.

Acceptance criteria:
- Timeline with event markers (first D, forgiveness threshold, switches).
- Step controls; shows the seed and allows change when world permits reseeding.

## Story 4.5 — Gating and exports (subscriber)
As a product, I want analytics gating so that premium features drive conversion.

Acceptance criteria:
- Filters and drilldowns are locked for free users with teasers.
- Export aggregated stats (CSV/JSON) available to subscribers only.


