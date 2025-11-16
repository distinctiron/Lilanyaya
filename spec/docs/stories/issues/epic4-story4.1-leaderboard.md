Title: Epic 4 — Story 4.1: Leaderboard (latest)

User story:
As a spectator, I want a leaderboard so that I can see top performers at a glance.

Acceptance criteria:
- Sorted by average score; shows variance and entries count.
- Cached for fast loads; links to underlying run details.

Technical notes:
- GET /worlds/{id}/leaderboard returns latest leaderboard; cache in Redis.

Links:
- docs/stories/epic-4-analytics.md#story-41--leaderboard-latest

Labels: epic:analytics, type:feature, area:web


