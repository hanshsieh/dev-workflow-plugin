---
name: find-issues
description: Performs evidence-driven issue discovery of a repository and validates root causes without changing source code. Use when diagnosing bugs and issues of a repository.
disable-model-invocation: true
---

# Find Issues

## Purpose

Find issues and bugs in a repository and rank by impact.  
This skill is discovery-only: do not apply fixes or change source code.  
You may create report files under the `issues` folder for documentation output.  

## Arguments

- `output=<resp|file>`
- Default: `file`
- If `output=file`, write issue reports to the `issues` folder.
- If `output=resp`, return all issue reports directly in the response and do not write files.

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

## 4) Prioritization Matrix (Rank by Impact)

Rank issues from high to low impact using:

- High impact if left unresolved
- High confidence in root cause
- Clear and actionable review recommendation

Flag large uncertain refactors as follow-up items for manual review.

## 5) Validation

- Reproduce or logically prove the bug.
- Verify the evidence is specific and reproducible.
- Provide a minimal review recommendation.
- Call out uncertainty or missing data explicitly.

## Output Format

Output behavior depends on `output`:

- `output=file` (default): output all issues to the `issues` folder, one issue per `.md` file.  
  Create a Markdown file for each issue, such as:  
  - `01-dry-run-update-db.md`
  - `02-missing-auth.md`
- `output=resp`: return all issues in the response directly using the same report schema.

The issues should be ordered from high to low impact.  
Use the format below:  

```markdown
# Issue: <title>

- Domain: <security/performance/reliability/tooling>
- Impact: <high/medium/low>
- Confidence: <high/medium/low>

## Evidence
...

## Trigger path
...

## Root cause
...

## Suggestion
...
```

Evidence must include exact file paths and code segments.  
Trigger path should describe how the issue is reached in real execution.  

## Guardrails

- Do not fix issues in this workflow; only report and prioritize.
- Do not include unrelated improvements in issue reports.
- Do not claim risk without a concrete trigger path.
