---
name: code-review
description: Structured, enforceable code review guide for AI agents. Use when reviewing code changes, pull requests, or when asked to "review", "audit", "check", or "analyze" code quality, correctness, or adherence to specifications.
---

# Code Review

## Quick Reference

When reviewing code, run through these 11 checks in order:

1. **Correctness & Spec Compliance** - Check for MUST/CANNOT violations
2. **Fail-Fast & Error Handling** - Find silent fallbacks and swallowed exceptions
3. **Determinism & Reproducibility** - Identify randomness and non-determinism
4. **Hidden Coupling** - Detect globals, singletons, test leaks
5. **Single Source of Truth** - Flag duplicated state
6. **Architectural Pollution** - Look for scope creep
7. **Test Quality** - Verify properties, not just outcomes
8. **Liveness & Termination** - Ensure loops terminate
9. **Observability** - Check logging coverage
10. **Authority Boundaries** - Identify who decides correctness
11. **Domain State Representation** - Flag None/null for domain decisions

## Immediate Flags

Report these patterns immediately:

- Fallback behavior, silent catch blocks
- Randomized defaults, probabilistic tests
- Duplicate state, multiple authority sources
- "Edge case" rationalization
- "Probably fine" assessments

## Detailed Guide

For comprehensive checks, see [REVIEW_GUIDE.md](REVIEW_GUIDE.md).

## Parallel Review

For large reviews, spawn multiple reviewer agents to check categories in parallel:

```
/task --name "fail-fast-check" --prompt "Check sections 2, 3 for fail-fast violations..."
/task --name "state-check" --prompt "Check sections 5, 11 for state representation issues..."
/task --name "architecture-check" --prompt "Check sections 4, 6 for architectural issues..."
```

## Output Format

Report findings structured by category:

```
## [Category Name]

**Status:** PASS | FAIL | PARTIAL

**Findings:**
- [Issue 1]
- [Issue 2]

**Recommendation:**
[Clear fix or next step]
```
