Title: Epic 4 — Story 4.3: Pairwise matrix API

User story:
As a researcher, I want pairwise metrics so that I can analyze interactions between actors.

Acceptance criteria:
- Returns average payoff, outcome distribution (C/C, C/D, D/C, D/D), first defection round, punish duration.
- Supports filtering by cohorts and round windows (subscriber).

Technical notes:
- Backed by pairwise_matrix artifact; derive cohorts/windows server-side.

Links:
- docs/stories/epic-4-analytics.md#story-43--pairwise-matrix-application-programming-interface

Labels: epic:analytics, type:feature, gating:subscriber


