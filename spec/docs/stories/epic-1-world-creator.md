# Epic 1 — World Creator

Goal: Paid users can create, validate, and publish worlds with clear rules, seeded randomness, Key Performance Indicators, and baseline actors.

## Story 1.1 — Create world from template
As a paid user, I want to create a world from a template so that I can start quickly with sensible defaults.

Acceptance criteria:
- Template picker shows: Prisoner’s Dilemma v1, Blank, Custom Matrix Game.
- Form captures name, description, visibility (public/private) and saves.
- After save, world detail page opens with next steps highlighted.

## Story 1.2 — Define rules (decisions, outcomes, policy, iterations)
As a world creator, I want to define rules so that simulations behave as specified.

Acceptance criteria:
- Decision space is an array; outcome space auto‑derived with manual override option.
- Interaction policy choices: all_pairs, round_robin, random; validation on change.
- Iterations is a positive integer; invalid inputs blocked with inline errors.

## Story 1.3 — Configure determinism and randomness
As a world creator, I want deterministic seeds and optional schedule randomization so that runs are reproducible yet flexible.

Acceptance criteria:
- rng_seed is required and persisted.
- schedule_randomization toggle appears when policy is random; uses seeded shuffles.
- World schema preview reflects chosen settings.

## Story 1.4 — Set scoring and payoff
As a world creator, I want to define payoff and scoring so that rankings reflect my world.

Acceptance criteria:
- Payoff matrix editor with Prisoner’s Dilemma defaults; validation prevents missing cells.
- Scoring options: sum | average | discounted (gamma input appears only when chosen).
- Function reference input is present but disabled (future).

## Story 1.5 — Configure state and state update
As a world creator, I want to configure state so that the engine can track histories and rounds.

Acceptance criteria:
- Initial state shows round and history defaults; editable JSON with schema validation.
- Built‑in update selector (pd.update_state) selectable; custom disabled (future).

## Story 1.6 — Build Key Performance Indicators
As a world creator, I want to define Key Performance Indicators so that I can track important signals.

Acceptance criteria:
- Builder supports scope (world/actor/pair), filters, window, aggregation.
- Save/list/delete Key Performance Indicators per world.
- Latest run snapshot values are rendered on the world page.

## Story 1.7 — Seed baseline actors
As a world creator, I want baseline actors so that entrants have references.

Acceptance criteria:
- Add Always Cooperate, Always Defect, Tit For Tat, Win‑Stay/Lose‑Shift with one click.
- actor_strategy_mode (one_to_one | many_to_one) governs whether duplicates are allowed.

## Story 1.8 — Validate world (10‑run sanity)
As a world creator, I want a sanity check so that obvious issues are caught early.

Acceptance criteria:
- Triggers a 10‑run deterministic job; artifacts stored; status surfaced.
- Summary view shows pairwise averages and outlier detection; errors are actionable.

## Story 1.9 — Publish world page
As a world creator, I want to publish the world so that others can view and participate.

Acceptance criteria:
- Public overview includes rules, assumptions, Key Performance Indicators, leaderboards, and seed actors.
- Submission policy is configurable: open/curated/invite‑only; reflected in UI and Application Programming Interface.


