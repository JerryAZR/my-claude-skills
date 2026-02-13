---
name: constitution-creator
description: Creates or updates project constitutions with eight core principles merged into the default constitution workflow. Use when: (1) Creating a new project constitution, (2) Updating existing constitutional principles, (3) Establishing development standards for a project, or (4) Any request involving "constitution", "principles", or "development standards".
---

# Constitution Creator

This skill provides a customized default invocation of `/speckit.constitution` merged with eight core development principles.

## Default Invocation

When invoked, automatically execute `/speckit.constitution` with the following principles pre-merged:

```
/speckit.constitution

Project Constitution - Eight Core Principles:

I. Test-First Development — failing tests must be written before implementation

II. Architectural Integrity — single sources of truth with no duplicate logic or parallel handlers

III. Test Coverage Quality — all public APIs must have corresponding tests with validated coverage reporting

IV. Documentation Standards — all public APIs require docstrings with type hints and examples

V. Simplicity First — start with the simplest implementation, avoid over-engineering

VI. Timeout Safety — all loops must have maximum iteration bounds and all wait operations must have explicit timeouts

VII. Fail-Fast Strict Mode — missing required data must raise explicit errors, fallback behavior is forbidden

VIII. Explicit Domain States — None/null must only represent absence of value, distinct domain states require explicit types (enums, dataclasses, Result patterns)

Additional Requirements:
- Constitution supersedes all other practices
- Test-first compliance verification required in all PRs
- Follow semantic versioning for amendments
```

## Usage

Simply invoke this skill or use the command:

```
/constitution-creator
```

The skill will invoke `/speckit.constitution` with your eight principles automatically merged as the foundation.

## Adding Custom Principles

To add project-specific principles beyond the eight core principles, append them after invoking the skill. The eight core principles serve as the foundation and cannot be removed, but project-specific additions can be included.
