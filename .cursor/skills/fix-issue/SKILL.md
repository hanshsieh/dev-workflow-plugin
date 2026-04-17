---
name: fix-issue
description: Diagnoses and fixes a user-specified issue in an existing codebase. Use when the user asks to resolve an issue.
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

## Execution Workflow

Copy this checklist and keep it updated:

```markdown
Issue Progress:
- [ ] 1) Confirm issue scope and expected behavior
- [ ] 2) Reproduce or validate failing behavior
- [ ] 3) Trace to root cause
- [ ] 4) Design minimal fix and compatibility plan
- [ ] 5) Implement targeted change
- [ ] 6) Run focused tests
- [ ] 7) Run broader regression checks
```

### 1) Confirm scope

Capture:

- Observed behavior
- Expected behavior
- Impact area (security/performance/reliability/tooling/tests)
- Constraints (time, migration risk, compatibility requirements)

If the issue statement is ambiguous, ask targeted clarification questions before editing code.

### 2) Reproduce or validate

- Reproduce with tests, logs, profiler output, or a deterministic local scenario.
- If no reproduction exists, create a focused failing test when practical.
- Record trigger conditions and blast radius.

### 3) Diagnose root cause

Use evidence from code paths, runtime behavior, and configuration.

Reject fixes that only hide symptoms (for example: blanket retries, silent catches, over-broad caching, or disabling checks) unless explicitly requested.

### 4) Design minimal fix

Before editing, define:

- Exact code location(s) to change
- Why each change is necessary
- Backwards compatibility/migration implications
- Rollback safety

Prefer one issue = one small patch = one commit.

### 5) Implement targeted change

- Keep edits localized.
- Avoid opportunistic refactors.
- Add concise comments where logic is non-obvious.
- Maintain existing interfaces unless required by the fix.

### 6) Validate continuously

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
