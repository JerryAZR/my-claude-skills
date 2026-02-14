---
name: architect
description: System architecture design agent. Use when starting a new project, designing a new system component, or restructuring existing code. Invoked at architecture boundaries to produce a timeless steady-state structure.
---

# Architect

## When to Use

Invoke this skill when:
- Starting a new project from scratch
- Designing a new system or major subsystem
- Restructuring existing code at system level
- Asked to "design", "architect", or "structure" a system

**Do NOT use when:**
- Architecture already exists (suggest architectural review instead)
- Implementation planning (use milestone planning after architecture is complete)
- Writing code (this skill produces architecture, not implementation)

## Pre-requisites

Before designing architecture, verify you have:

1. **Complete problem statement** - What problem does this system solve?
2. **Explicit scope** - What is in/out of bounds?
3. **Constraints** - Performance? Scale? Tech stack? Regulatory?
4. **Stakeholders** - Who uses this system?

**If any are missing, stop and request clarification.**

## Workflow

### 1. Verify No Existing Architecture

Check if architecture already exists in the project. If it does:
- Stop and inform the user
- Suggest architectural review instead

### 2. Decompose Top-Down

Decompose hierarchically:
```
System → Subsystems → Modules → Data Models
```

Each level must justify the next.

### 3. Define Components

For each component, specify:
- **Responsibility**: Single sentence, no "and"
- **State ownership**: What data does it own?
- **Authority boundaries**: What decisions does it make?
- **Contracts**: How does it interact with others?
- **Invariants**: What must always be true?

### 4. Stop Conditions

Stop decomposing when:
- Responsibility fits in one sentence without conjunctions
- Internal logic directly serves that responsibility
- Clear ownership of state/behavior
- Stable contract boundary (further splitting increases coupling)
- No artificial fragmentation (no coordination-only wrappers)

See [DECOMPOSITION.md](DECOMPOSITION.md) for detailed criteria.

## Output Structure

Produce architecture document with:

```markdown
# [System Name] Architecture

## Problem Statement
[What this system solves]

## Scope
- [In scope]
- [Out of scope]

## High-Level Structure
[System components and their relationships]

## Subsystem: [Name]
### Responsibilities
### State Ownership
### Authority Boundaries
### Contracts

## Data Model
[Key data entities and ownership]

## Forbidden Dependencies
[What this system must NOT depend on]

## Extension Points
[Where the system can be extended]
```

## Constraints

Architecture must be:
- **Timeless**: No timelines, phases, or MVP discussions
- **Complete**: All components defined, no gaps
- **Consistent**: No duplicated responsibilities
- **Frozen**: Later planning cannot alter without reopening architecture phase
