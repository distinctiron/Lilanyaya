# Epic 2 — Strategy Creator (Natural Language and Code)

Goal: Creators can define strategies via Natural Language or Code, preview, test, simulate, and publish.

## Story 2.1 — Choose authoring mode
As a strategy creator, I want to choose Natural Language or Code at creation so that I can use the best tool for my skillset.

Acceptance criteria:
- Mode toggle is required on first save and becomes read‑only thereafter.
- UI adjusts fields and validation rules per mode.

## Story 2.2 — Natural Language → Finite State Machine compile
As a creator, I want my natural language prompt compiled to a Finite State Machine so that it runs safely and is explainable.

Acceptance criteria:
- Compiler validates guards, actions, and decision set.
- Preview renders states, transitions, and two sample traces (vs Always Cooperate/Always Defect).
- Errors point to the offending rule with suggestions.

## Story 2.3 — Code strategy editor (sandboxed)
As an advanced creator, I want to implement a constrained function so that I can express behavior as code safely.

Acceptance criteria:
- Editor shows interface contract (input shape and allowed outputs) and language selector.
- Save runs static checks (signature, forbidden globals/imports, size).
- Runtime enforces timeout and memory limits; invalid outputs are rejected with clear errors.

## Story 2.4 — Static preview (no compute)
As a creator, I want a static preview so that I can verify structure before spending runs.

Acceptance criteria:
- Behavior summary with validation results and, for Natural Language, state/transition diagram.
- No quota is consumed.

## Story 2.5 — Quick Test (subscriber)
As a subscriber, I want a 10‑run quick test vs baselines so that I can gauge behavior fast.

Acceptance criteria:
- Deterministic seed used; per‑opponent mini‑cards show averages and outcome mix.
- Free users see a teaser with upgrade call to action.

## Story 2.6 — Full simulation
As a creator, I want to run full simulations so that I can get ranked, reproducible results.

Acceptance criteria:
- User selects iterations, seed, and policy (respecting world settings); quotas enforced on enqueue.
- Artifacts (summary, pairwise, traces if enabled) are persisted and linked.

## Story 2.7 — Versioning
As a creator, I want to version my strategy so that I can iterate safely.

Acceptance criteria:
- Save with notes creates a new immutable version; list and revert supported.
- Published link points to latest by default; specific versions linkable.

## Story 2.8 — Publish strategy
As a creator, I want to publish my strategy so that others can view and compete against it.

Acceptance criteria:
- Visibility control (public/private); optional readme/description.
- Shareable link displays performance summary and compatibility (world).


