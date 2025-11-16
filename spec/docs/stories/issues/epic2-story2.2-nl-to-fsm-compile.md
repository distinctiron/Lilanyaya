Title: Epic 2 — Story 2.2: Natural Language → Finite State Machine compile

User story:
As a creator, I want my natural language prompt compiled to a Finite State Machine so that it runs safely and is explainable.

Acceptance criteria:
- Compiler validates guards, actions, and decision set.
- Preview renders states, transitions, and two sample traces (vs Always Cooperate/Always Defect).
- Errors point to the offending rule with suggestions.

Technical notes:
- Implement prompt → FSM compiler; reuse counters/guards from strategy spec.

Links:
- docs/stories/epic-2-strategy-creator.md#story-22--natural-language--finite-state-machine-compile
- docs/architecture/strategy-dsl-and-sandbox.md

Labels: epic:strategy-creator, type:feature, area:compiler


