# Decomposition Guidelines

## Top-Down Decomposition

Decompose hierarchically from full system to data models:

```
System → Subsystems → Modules → Data Models
```

Each level must **justify** the next. Ask: "Why does this component exist?"

---

## Stop Conditions

Stop decomposing when ALL conditions are met:

### 1. Single Responsibility Achieved

The unit's responsibility can be stated in one precise sentence without:
- "and"
- "or"
- "with"

**Bad**: "The user service handles authentication and authorization and manages user profiles."

**Good**: "The user service manages user identity and authentication."

### 2. Cohesive Internal Logic

All internal behavior directly serves that single responsibility. No unrelated branching logic.

### 3. Clear Ownership

The unit owns a well-defined portion of state or behavior that is not shared implicitly with peers.

### 4. Stable Contract Boundary

The unit interacts with others only through explicit, minimal interfaces. Further splitting would increase coupling rather than reduce it.

### 5. No Artificial Fragmentation

Further decomposition would create:
- Coordination-only wrappers
- Pass-through modules
- Abstractions without independent decision-making authority

---

## If Conditions Are Not Met

- **Continue decomposing** if any condition fails
- **Consider current level atomic** if further decomposition would violate simplicity or introduce artificial structure

---

## Rules

### Single Source of Truth

If two units require the same logic or data, lift it to a single higher-level authority rather than duplicate.

### Mutual Exclusivity

Responsibilities must be:
- **Mutually exclusive** - No overlap
- **Collectively complete** - All work is covered

### State Ownership

One authoritative owner per piece of data. Define who owns what.

### Contracts Over Internals

Interactions between units via clear contracts, not shared internals.

### The "And" Test

If a unit's responsibility cannot be stated without "and", it must be further decomposed.

---

## Output Requirements

Architecture output must describe:
- What exists (not when or how)
- How components relate (not implementation order)
- Steady-state structure (not milestones)

**Forbidden in architecture:**
- Timelines
- Phases
- MVP discussions
- Implementation order
- Sequencing
