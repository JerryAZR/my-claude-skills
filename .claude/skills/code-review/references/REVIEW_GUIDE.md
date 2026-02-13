# AI Code Review Guide

*(Structured, enforceable, non-opinionated)*

## 1. Correctness & Spec Compliance

### What to check

* Does the code violate **any MUST / CANNOT rule**?
* Are there paths that skip mandatory logic?
* Are there cases where the spec is *implicitly* interpreted?

### Actionable checks

* List **all spec rules referenced** in the code
* List **all spec rules NOT referenced**
* For each rule, answer: *Is there a code path that violates it?*

### Red flags

* "This is probably okay"
* "Edge case"
* "Unlikely scenario"
* "Shouldn't happen in practice"

These phrases indicate **spec drift**.

---

## 2. Fail-Fast & Error Handling

### What to check

* Missing data → does the system fail explicitly?
* Invalid state → is it detected immediately?
* Are exceptions swallowed or converted into defaults?

### Actionable checks

* Enumerate all `try/except`, `if x is None`, `get(..., default)` patterns
* For each, classify:

  * Explicit failure
  * Silent fallback
  * Guess / default
  * Randomized continuation

### Forbidden patterns

* Silent catch blocks
* Default values masking missing state
* "Best effort" continuation
* Logging without aborting

**Rule of thumb:**
If execution continues after an error *without human visibility*, it's a bug.

---

## 3. Determinism & Reproducibility

### What to check

* Is behavior reproducible given the same inputs?
* Is randomness controlled, seeded, or eliminated?
* Are tests deterministic?

### Actionable checks

* List all sources of randomness
* List all time-dependent logic
* List all environment-dependent logic

For each:

* Is it explicitly controlled in tests?
* Is it forbidden in production paths?

### Forbidden patterns

* Random behavior in core logic
* Probabilistic test doubles
* "10% chance", "occasionally", "sometimes"

---

## 4. Hidden Coupling & Information Leaks

### What to check

* Does the function under test have access to:

  * global state?
  * test expectations?
  * shared mutable objects?
* Can tests pass because code *knows the answer*?

### Actionable checks

* Identify all global variables accessed
* Identify shared singletons / registries
* Identify bidirectional dependencies

### Red flags

* Tests passing when they "should fail"
* Functions accessing config/test data implicitly
* Circular imports with logic

This is where architectural bugs hide.

---

## 5. Single Source of Truth (SSOT)

### What to check

* Is the same concept represented in multiple places?
* Are there derived values treated as authoritative?

### Actionable checks

* For each domain concept:

  * Where is it defined?
  * Where is it derived?
  * Who owns mutation authority?

### Forbidden patterns

* Duplicated state with "sync" logic
* Cached values without invalidation rules
* Multiple writers to the same conceptual state

---

## 6. Architectural Pollution & Scope Creep

### What to check

* Did new logic introduce:

  * parallel pipelines?
  * new managers/handlers?
  * side-paths bypassing core flow?

### Actionable checks

* List all entry points for a given behavior
* Check if behavior can occur through more than one path

### Red flags

* "To make this easier, I added..."
* "Temporary helper"
* "Just for this case"

These are pollution markers.

---

## 7. Test Quality (NOT quantity)

### What to check

* Do tests assert *properties*, or just outcomes?
* Do tests fail when they should?
* Are tests sensitive to randomness, ordering, luck?

### Actionable checks

* Identify tests that:

  * do not assert invariants
  * rely on random behavior
  * pass without verifying state transitions

### Mandatory test categories

* Negative tests (should-fail)
* Boundary tests
* Symmetry / fairness tests
* Distribution tests (for randomness)

---

## 8. Liveness & Termination

### What to check

* Can loops run indefinitely?
* Are there retry loops without bounds?
* Is progress guaranteed?

### Actionable checks

* List all loops
* For each loop:

  * What enforces termination?
  * Is there a max iteration / timeout?

### Forbidden patterns

* `while True` without guard
* Retry until success
* Waiting for state change without timeout

---

## 9. Observability & Debuggability

### What to check

* Can failures be explained from logs alone?
* Are state transitions logged meaningfully?
* Is the causal chain visible?

### Actionable checks

* For each critical state transition:

  * Is it logged?
  * Is the reason logged?
  * Is the before/after state visible?

### Red flags

* Logs that say *what* but not *why*
* Missing logs on failure paths
* Overly verbose logs without structure

---

## 10. Authority & Decision Boundaries

### What to check

* Who decides correctness?
* Who classifies severity?
* Who resolves conflicts?

### Actionable checks

* Identify all places where the code:

  * decides something is "acceptable"
  * downgrades an error
  * ignores a violation

### Forbidden patterns

* "This is acceptable"
* "Probably fine"
* "Edge case, not a bug"

Classification authority must live **outside** the code.

---

## 11. Domain State Representation

### What to check

Does None/null represent a domain decision, outcome, or rule-relevant state such as:
- skip, tie, no winner
- default action
- any conceptual state that matters to the domain

### Actionable checks

For every assignment, return value, or conditional check involving None/null:

1. Identify what conceptual state None represents
2. Determine if the variable participates in:
   - Software logic
   - Rule enforcement
   - Decision-making
3. Check if None can represent multiple conceptual states (overloaded meaning)

### Forbidden patterns

* `result = None` meaning "no winner" vs "not computed"
* `if x is None: handle_skip()` where None means skip
* `return None` for domain outcomes like tie, no-result
* `status = None` where None could mean pending, unknown, or not-applicable

### Examples

| Code | Problem |
|------|---------|
| `winner = None` | None used for "tie" outcome |
| `action = None` | None used for "default action" |
| `if result is None: return` | Silent skip behavior |

**Rule:** If None participates in logic, rules, or decisions, use an explicit enum or typed state instead.

---

# Summary: Patterns to Avoid (Explicit List)

AI reviewers should **flag these immediately**:

* Fallback behavior
* Randomized defaults
* Silent exception handling
* Probabilistic tests
* Duplicate state
* Multiple authority sources
* Graceful degradation in core logic
* "Edge case" rationalization
* Fix-first, analyze-later behavior
* Architecture invented mid-fix
