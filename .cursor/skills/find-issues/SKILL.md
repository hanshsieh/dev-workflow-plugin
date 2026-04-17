---
name: issue-hunter
description: Performs fast, evidence-driven issue discovery of a repository and validates root causes without making changes. Use when diagnosing bugs and issues of a repository.
---

# Issue Hunter

## Purpose

Find the issues and bugs in a repository and rank by value.  
This skill is discovery-only and does not apply fixes.  

## Discovery Playbook

## 1) Build a Risk Map Quickly

Identify:

- Trust boundaries (client -> API -> DB -> external services)
- Failure boundaries (network, async jobs, caches, background tasks)
- Performance boundaries (hot endpoints, list/query paths, repeated computation)
- Tooling boundaries (build/test scripts, CI assumptions, environment coupling)

The issues could touch any part of the stack — security, performance, reliability, developer tooling, or testing.

## 2) Use Hypothesis-Driven Checks

For each suspected issue, record:

- Hypothesis: what is wrong
- Evidence: exact code/runtime signal
- Counter-check: what would disprove it

Only report an issue when evidence supports the hypothesis.

## 3) Domain-Specific Heuristics

### Security

- Missing authorization checks on sensitive actions.
- Input validated at edge but not at internal boundaries.
- Secrets/tokens logged or exposed.
- Dangerous dynamic query/command composition.

### Performance

- Repeated DB/API calls inside loops.
- Full-scan operations in request path.
- Missing pagination/limits on large responses.
- CPU-heavy work done synchronously per request.

### Reliability

- Missing timeout/retry/circuit handling for external dependencies.
- Unhandled promise rejections or swallowed errors.
- Non-idempotent retries causing duplicates.
- Shared mutable state without coordination.

### Tooling and Testability

- Inconsistent local vs CI scripts.
- Slow/flaky tests masking regressions.
- Missing test coverage around risky code paths.
- Broken developer scripts reducing confidence in changes.

## 4) Prioritization Matrix

Pick issues with:

- High impact if left unresolved
- High confidence in root cause
- Clear and actionable review recommendation

Flag large uncertain refactors as follow-up items for manual review.

## 5) Validation and Review Readiness

Before listing:
- Reproduce or logically prove the bug.

Before handing off to user:
- Verify the evidence is specific and reproducible.
- Provide a minimal review recommendation.
- Call out uncertainty or missing data explicitly.

## Evidence Note Format

Use this short format while auditing:

```markdown
Issue: <title>
Domain: <security/performance/reliability/tooling>
Evidence: <code path or runtime observation>
Root cause: <one sentence>
Suggested review action: <what user should inspect or approve>
Confidence: <high/medium/low>
Decision: Review now | Defer
```

## Guardrails

- Do not fix issues in this workflow; only report and prioritize.
- Do not include unrelated improvements in issue reports.
- Do not claim risk without a concrete trigger path.
- Do not skip evidence validation due to time pressure.
