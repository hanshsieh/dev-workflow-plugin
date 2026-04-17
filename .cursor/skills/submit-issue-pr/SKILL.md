---
name: submit-issue-pr
description: Commits all current changes into a single commit and creates one pull request. Use after fixes are implemented and validated, when the user asks to submit work as a PR.
disable-model-invocation: true
---

# Submit issue PR

## Goal

Commit validated work into a single commit and create one review-ready pull request with a clear summary.

## Preconditions

Before using this skill, confirm:

- Fixes are already implemented
- Relevant tests/checks have been run

Also confirm your working tree contains only intended changes for this PR (avoid including unrelated work).

If not, return to `fix-issue` first.

### 1) Inspect git state

Review:

- Staged and unstaged diffs
- Untracked files
- Current branch and upstream status

### 2) Stage and commit everything in one commit

Stage all intended changes and create exactly one commit.

### 3) Commit

Recommended subject:

```text
fix(<scope>): resolve <short problem> (root-cause addressed)
```

Commit body should include:

- What was fixed (at a high level)
- Why the approach is correct/minimal
- What validation was run

Confirm with the user for the commit message first.

### 4) Verify branch state

Confirm whether current branch:

- Is the intended branch for the fix
- Has an upstream
- Is ahead/behind remote
- Is not main branch

### 5) Push safely

Push with upstream tracking when first push is needed.

Avoid force-push unless explicitly requested by the user.

### 6) Create PR

Use a concise PR structure:

```markdown
## Summary
### Issue: <Issue 1 title>
<root cause summary>

### Issue: <Issue 2 title>
<root cause summary>

### Approach
<brief how the fix addresses the root cause>

### Compatibility
<any migration/backwards-compat notes>
```

Use `gh` to create draft PR.  
If multiple issues were fixed, ensure all issues are listed in the PR description.

### Final handoff

Return:

- Commit list (hash + subject)
- PR URL
- Residual risks or follow-up items

## Quality Bar

Delivery is complete only when:

- There is exactly one commit for the PR
- Commit messages explain why, not only what
- PR description is review-friendly (includes all issues combined if multiple)
- Remote branch and PR are in a clean, shareable state

## Anti-Patterns

Do not:

- Use vague commit subjects like "update stuff"
- Hide risky changes without documenting trade-offs
