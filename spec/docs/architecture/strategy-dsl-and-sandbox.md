### Strategy Domain‑Specific Language and Execution Sandbox (Minimum Viable Product)

Goals
- Enable deterministic, safe strategy behavior without executing user code.
- Make strategies explainable (finite states, guards, actions).
- Compile natural language prompts into an auditable intermediate form.

Design philosophy
- No user-provided code in the Minimum Viable Product; interpret a constrained Domain‑Specific Language / Finite State Machine.
- World‑aware contracts: strategies must only emit decisions allowed by the world.
- Deterministic by construction (no random number generator inside strategies in the Minimum Viable Product).

Strategy model
- identity:
  - id, name, description, tags, world_id, version
- type: "Finite State Machine"
- states: string[]                      # e.g., ["START","PUNISH","COOP"]
- initial_state: string                 # e.g., "START"
- transitions: Transition[]             # see below
- action_map: Record<state, decision>   # state → decision in decisions.allowed
- memory:
  - builtin counters only (derived from world state/observations), e.g.:
    - consecutive_opponent_C
    - since_last_opponent_D_coops
    - round_index
    - last_opponent_action

Transition
- from: string
- to: string
- guard: GuardExpression

GuardExpression (grammar)
- Primitives:
  - last_opponent_action == "C" | "D"
  - consecutive_opponent_C >= n
  - since_last_opponent_D_coops >= n
  - round_index >= n
  - streak("D/D") >= n               # optional pair-outcome streak helper
- Operators:
  - and, or, not
- Parentheses for grouping

Examples (Strategy A – ForgiveAfter3Coops)
- states: ["START","PUNISH","COOP"]
- initial_state: "START"
- action_map:
  - START: "C"
  - PUNISH: "D"
  - COOP: "C"
- transitions:
  - { from: "START", to: "PUNISH", guard: last_opponent_action == "D" }
  - { from: "PUNISH", to: "COOP", guard: since_last_opponent_D_coops >= 3 }
  - { from: "COOP", to: "PUNISH", guard: last_opponent_action == "D" }

Compiler (prompt → constrained representation)
- Inputs:
  - Natural language: “Start C; after any D, defect until 3 consecutive opponent C; then return to C.”
  - World contract: decisions.allowed, available counters, observations
- Outputs:
  - Finite State Machine JSON as above (+ rationale block for audit)
- Steps:
  1) Extract intent, states, action defaults, forgiveness/punish rules.
  2) Map rules to guards from the allowed primitives.
  3) Validate decisions against world decisions.allowed.
  4) Static checks (see below).

Validation & static checks
- Structural:
  - initial_state ∈ states; transitions reference valid states.
  - action_map covers all states; actions ∈ decisions.allowed.
- Safety:
  - No unreachable states (warn).
  - Dead end detection: states with no exit; allow if intentional but warn.
  - No non-deterministic branches (guards must be disjoint or prioritized).
- Determinism:
  - No random primitives; no timeouts beyond engine step limits.

Execution sandbox (interpreter)
- Input per step:
  - current state, last_opponent_action, counters, round_index
- Process:
  1) Evaluate transitions from current state in order; first true guard wins.
  2) If none true, remain in current state.
  3) Emit decision from action_map[state].
  4) Engine updates counters from pair outcome; increments round_index.
- Output:
  - decision ∈ world decisions.allowed
  - trace hooks (when enabled): (state_before, guard_matched, state_after, decision)

Explainability hooks
- State trace: list of (round, state, decision, opponent_action).
- Transition hits: counts per guard.
- Counter snapshots: min/avg/max for selected counters.

Constraints (Minimum Viable Product)
- No user variables or arbitrary arithmetic.
- Only provided counters/observations are available.
- Guards limited to simple comparisons and boolean ops.

Extensibility
- Add windowed counters (e.g., count of “C/D” in last N rounds).
- Parameterized strategies (tunable n for forgiveness, etc.).
- Library of common guards and templates (Tit For Tat, Win‑Stay/Lose‑Shift, Grim Trigger).
- Alternate models: table policies, simple rulesets (if/then) compiled to a Finite State Machine.

### Code strategy interface (optional, sandboxed)

Purpose
- Allow advanced users to implement strategy logic as code while preserving safety, determinism, and compatibility with world rules.

Execution model
- Strategies must implement a pure function that returns a valid decision from the world’s decision space.
- The function receives a strictly defined, serializable input and MUST NOT access system resources or global state.
- Execution occurs in a constrained sandbox with:
  - wall‑clock timeouts per step,
  - memory limits,
  - deterministic inputs (seed included if needed),
  - banned I/O and networking.

Language support
- Start with one runtime (for example, JavaScript/TypeScript or Python) with preloaded standard library restrictions.

Example interface (language‑agnostic)
```
decision = strategy_step({
  roundIndex: number,
  lastOpponentAction: "C" | "D" | null,
  historySummary: {
    consecutiveOpponentC: number,
    sinceLastOpponentDCoops: number
  },
  pairContext: {
    // optional: limited, precomputed summary the engine exposes
  },
  world: {
    allowedDecisions: string[] // e.g., ["C","D"]
  }
})
// decision MUST be one of world.allowedDecisions
```

Validation
- Pre‑execution static checks:
  - signature compliance,
  - forbidden imports/globals,
  - size/complexity limits.
- Runtime checks:
  - timeouts, memory guardrails, decision validation.

Determinism
- No access to non‑deterministic sources unless provided by the engine as seeded inputs.
- Same inputs → same decision.

Explainability
- The engine records:
  - input snapshot and decision per step (for sampled rounds),
  - aggregate counters (decision distributions),
  - optional developer‑provided “reason” string (size‑limited) for debugging.

JSON schema sketch (abridged)
{
  "type": "object",
  "required": ["type","states","initial_state","action_map","transitions"],
  "properties": {
    "type": { "const": "Finite State Machine" },
    "states": { "type": "array", "items": { "type": "string" } },
    "initial_state": { "type": "string" },
    "action_map": { "type": "object", "additionalProperties": { "type": "string" } },
    "transitions": {
      "type": "array",
      "items": {
        "type": "object",
        "required": ["from","to","guard"],
        "properties": {
          "from": { "type": "string" },
          "to": { "type": "string" },
          "guard": { "type": "string" }  // parsed by guard interpreter
        }
      }
    }
  }
}


