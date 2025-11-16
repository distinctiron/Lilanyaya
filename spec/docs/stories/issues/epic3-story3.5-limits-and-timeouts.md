Title: Epic 3 — Story 3.5: Limits and timeouts

User story:
As an operator, I need guardrails so that runs finish and resources are protected.

Acceptance criteria:
- Per‑decision step timeout and overall run timeout enforced; clear error codes.
- Maximum steps bound prevents pathological runs; failures recorded in summary.

Technical notes:
- Abort and mark run status as failed/invalid with reason; partial artifacts allowed.

Links:
- docs/stories/epic-3-simulation-engine.md#story-35--limits-and-timeouts

Labels: epic:engine, type:feature, area:reliability


