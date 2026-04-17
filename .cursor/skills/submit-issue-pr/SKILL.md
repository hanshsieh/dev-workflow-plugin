---
name: submit-issue-pr
description: Commits all current changes into a single commit and creates one pull request. Use after fixes are implemented and validated, when the user asks to submit work as a PR.
disable-model-invocation: true
---

# Submit Issue PR

## Goal

Commit validated work into a single commit and create one review-ready pull request with a clear summary.

## Preconditions

Before using this skill, confirm:

- From the previous context, the issue that has been fixed is clear.
- There's only one issue.
- Fixes are already implemented
- Relevant tests/checks have been run

Also confirm your working tree contains only intended changes for this PR (avoid including unrelated work).
If multiple issues are detected in the current changes, stop and ask the user to split scope before proceeding.

### Inspect git state

Review:

- Staged and unstaged diffs
- Untracked files
- Current branch is not `main` or `master`
- Is up to date with `origin/main`. If not, stop and ask the user before going forward.

### Commit

The commit message template:

```markdown
<title>

<description>

Validation: <what validation was run>
```

Create a single commit for all changes.  
Before creating the commit, confirm with the user first.

### Create PR

Use a concise PR structure:

```markdown
# Issue: <title>
<root cause summary>

# Approach
<brief how the fix addresses the root cause>

# Compatibility
<any migration/backwards-compat notes>
```

Avoid force-push unless explicitly requested by the user.  
Use `gh` to create draft PR. No confirmation needed.  

### Final handoff

Return:

- Commit list (hash + subject)
- PR URL
- Residual risks or follow-up items

## Quality Bar

Delivery is complete only when:

- There is exactly one commit for the PR
- Commit messages explain why, not only what
- PR description is review-friendly and scoped to exactly one issue
- Remote branch and PR are in a clean, shareable state

## Anti-Patterns

Do not:

- Use vague commit subjects like "update stuff"
- Hide risky changes without documenting trade-offs
