### World Schema v1 and Key Performance Indicator Model

Purpose
- Define a deterministic, minimal, extensible schema for worlds (PD first).
- Provide a Key Performance Indicator model for querying/deriving metrics from simulation state/outcomes.

World Schema v1 (conceptual)
- metadata:
  - name: string
  - description: string
  - version: semver
  - visibility: public | private
- decisions:
  - allowed: string[]                 # e.g., ["C","D"]
- outcomes:
  - allowed: string[]                 # auto-derived from decisions × decisions (override optional)
- interaction:
  - policy: enum                      # all_pairs | round_robin | random | swiss
- time:
  - iterations: number
  - termination?:                     # Minimum Viable Product: fixed rounds only
    - type: enum                      # fixed_rounds | score_threshold | convergence
    - value: any
- determinism:
  - rng_seed: number                  # required
- randomness:
  - schedule_randomization?: boolean  # when true, use seeded random pairing/scheduling
  - noise?:                           # reserved; default disabled in Minimum Viable Product
    - enabled: boolean
    - params?: object                 # domain-specific noise parameters
- actor_strategy_mode: enum          # one_to_one | many_to_one
  # one_to_one: each actor must define its own strategy instance (default for Prisoner’s Dilemma)
  # many_to_one: multiple actors may reference the same strategy (enables shared baselines/entries)
- payoff:
  - type: enum                        # matrix | function_ref
  - matrix?: Record<string, Record<string, [number, number]>>   # outcome → [A,B]
  - function_ref?: string             # future: custom scorer by ref
- scoring:
  - aggregate: enum                   # sum | avg | discounted
  - discount_gamma?: number
  - ranking: enum                     # descending | percentile
- state:
  - initial: object                   # { round: 0, history: {} }
  - update: string                    # builtin.ref (e.g., pd.update_state)
- kpis:
  - definitions: KpiDefinition[]      # saved Key Performance Indicator configs (see below)

Actor/Strategy contract (summary)
- actors[].id/name/strategy_ref/params
- strategies must emit decisions ∈ decisions.allowed
- strategies are deterministic within a run (no internal RNG in MVP)

Deterministic execution rules
- Fixed rng_seed per run; same inputs → same outputs.
- Pairing determined by interaction.policy; if policy is random and schedule_randomization is true, seeded shuffles are used for reproducible randomness.
- No stochastic noise in strategies or payoffs in the Minimum Viable Product unless explicitly enabled under randomness.noise.

Prisoner’s Dilemma Example (YAML‑ish)
world:
  metadata:
    name: "PrisonersDilemma-v1"
    description: "Repeated Prisoner’s Dilemma with matrix payoff"
    version: "1.0.0"
    visibility: "public"
  decisions:
    allowed: ["C","D"]
  outcomes:
    allowed: ["C/C","C/D","D/C","D/D"]   # auto-derivable
  interaction:
    policy: "all_pairs"
  time:
    iterations: 1000
  determinism:
    rng_seed: 42
  actor_strategy_mode: "one_to_one"
  payoff:
    type: "matrix"
    matrix:
      "C":
        "C": [3,3]
        "D": [0,5]
      "D":
        "C": [5,0]
        "D": [1,1]
  scoring:
    aggregate: "sum"
    ranking: "descending"
  state:
    initial:
      round: 0
      history: {}
    update: "pd.update_state"
  kpis:
    definitions:
      - id: "mutual_cooperation_rate"
        label: "Mutual Cooperation Rate"
        scope: "world"
        query:
          filter: { outcome: "C/C" }
          window: { type: "all_rounds" }
          aggregation: "percentage_of_rounds"
      - id: "percent_defectors"
        label: "% Actors with >50% D"
        scope: "actor"
        query:
          filter: { decision: "D", ratio_gt: 0.5 }
          window: { type: "all_rounds" }
          aggregation: "percentage_of_actors"
      - id: "exploitation_rate"
        label: "Exploitation (C/D count)"
        scope: "pair"
        query:
          filter: { outcome: "C/D" }
          window: { type: "last_n_rounds", n: 100 }
          aggregation: "count"

Key Performance Indicator Model
- KpiDefinition
  - id: string
  - label: string
  - scope: enum                      # world | actor | pair
  - query:
    - filter: Outcome/Decision filters
      - outcome?: string             # e.g., "C/D"
      - decision?: string            # e.g., "D" (per actor)
      - ratio_gt?: number            # thresholds for proportions
      - actor_ids?: string[]         # optional focus set
      - opponent_ids?: string[]
    - window:
      - type: enum                   # all_rounds | first_n_rounds | last_n_rounds | range
      - n?: number
      - start?: number
      - end?: number
    - aggregation: enum              # count | sum | average | percentage_of_rounds | percentage_of_actors | variance
  - presentation:
    - chart?: enum                   # line | bar | table | heatmap
    - precision?: number
    - notes?: string

Key Performance Indicator Evaluation (engine perspective)
1) Resolve scope (world/actor/pair) → row set.
2) Apply window to outcomes/time series.
3) Apply filters (outcome/decision thresholds, cohorts).
4) Aggregate to metric(s); format per presentation hints.
5) Store snapshot with run metadata (seed, iterations).

Pairwise Metrics (common outputs)
- Average payoff (actor A vs actor B)
- Outcome distribution (C/C, C/D, D/C, D/D)
- First defection round
  - Punish duration (average rounds in D/D after first D)

Validation & Safety
- Schema validation (required fields, enums).
- Determinism checks (no random number generator use).
- Payoff sanity checks (for Prisoner’s Dilemma, optional constraints: T>R>P>S, 2R > T+S).

Extensibility Notes
- outcomes.allowed can be extended for non-binary decisions.
- payoff.function_ref enables domain-specific scoring in later versions.
- Key Performance Indicator filters/windows/aggregations are composable; add cohorts and tags later.


