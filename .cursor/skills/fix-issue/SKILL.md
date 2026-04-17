---
name: fix-issue
description: Diagnoses and fixes a user-specified issue in an existing codebase. Use when the user asks to resolve an issue.
disable-model-invocation: true
---

# Fix Issue

## Goal

Fix one user-specified issue with:

1. Root-cause diagnosis (not symptom patching)
2. Minimal, surgical code changes
3. Backwards compatibility validation
4. Clear written justification

## Operating Principles

- Fix the problem, not the neighborhood.
- Prefer the smallest safe change that resolves the root cause.
- Preserve existing behavior unless the issue explicitly requires behavior change.
- Keep explanations concise and technical: problem, impact, trigger, fix rationale.
- Always verify with tests after each meaningful change.
- If the issue statement is ambiguous, ask targeted clarification questions before editing code.

## Execution Workflow

- Confirm issue scope and expected behavior
- Reproduce or validate failing behavior
- Trace to root cause
- Design minimal fix and compatibility plan
- Implement targeted change
- Add corresponding tests if missing
- Run focused tests
- Run broader regression checks

### Reproduce or validate

- Reproduce with tests, logs, profiler output, or a deterministic local scenario.
- If no reproduction exists, create a focused failing test when practical.
- Record trigger conditions and blast radius.

### Diagnose root cause

Use evidence from code paths, runtime behavior, and configuration.

Reject fixes that only hide symptoms (for example: blanket retries, silent catches, over-broad caching, or disabling checks) unless explicitly requested.

### Design minimal fix

Before editing, define:

- Exact code location(s) to change
- Why each change is necessary
- Backwards compatibility/migration implications
- Rollback safety

### Implement targeted change

- Keep edits localized.
- Avoid opportunistic refactors.
- Add concise comments where logic is non-obvious.
- Maintain existing interfaces unless required by the fix.

### Validate continuously

After each significant edit:

1. Run narrow tests nearest to the changed area.
2. Run broader tests or build checks relevant to regressions.
3. Confirm no previously passing behavior regressed.

If test coverage is missing, add only the minimum test needed to lock in the fix.

## Anti-Patterns

Do not:

- Rewrite broad subsystems to fix a local bug.
- Ship unverified fixes without running checks.
- Explain only what changed without explaining why.
- Ignore migration or compatibility risk for security-sensitive fixes.
