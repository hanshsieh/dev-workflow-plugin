---
name: create-issue-pr
description: Commits only issue-related changes into a single commit and creates one pull request. Use after fixes are implemented and validated, when the user asks to submit work as a PR.
disable-model-invocation: true
---

# Create Issue PR

## Goal

Commit only validated, issue-related work into a single commit and create one review-ready pull request with a clear summary.

## Arguments

- `base=<remote>/<branch>`: Optional. PR target, for example `base=origin/main` or `base=upstream/main`.
- `push=<remote>` or `push=<remote>/<branch>`: Optional. Push destination, for example `push=origin` or `push=origin/fix-issue`.
- If `base` is not provided, default to `origin/main`.
- If `push` is not provided, default to `origin` with the same current branch name.
- If provided arguments are invalid or ambiguous (for example, missing remote or branch), stop and ask the user.

## Preconditions

Before using this skill, confirm:

- From the previous context, the issue that has been fixed is clear.
- There's only one issue.
- Fixes are already implemented
- Relevant tests/checks have been run

If multiple issues are detected in the current changes, stop and ask the user to split scope before proceeding.

### Inspect git state

Review:

- Staged and unstaged diffs
- Untracked files
- Current branch is not `main` or `master`
- Parse `base=<remote>/<branch>` and `push=<remote>[/<branch>]` first.
- Confirm base remote/branch and push remote/branch exist.
- Confirm current branch is up to date with the selected base branch (for example, `origin/main`). If not, stop and ask the user before going forward.
- Classify changed files into issue-related vs unrelated.
- Stage only issue-related files for commit. Leave unrelated local changes unstaged and untouched.

### Commit

The commit message template:

```markdown
<title>

<description>

Validation: <what validation was run>
```

Create a single commit containing only issue-related files.  
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
Push branch to the selected push destination before creating PR (for example, `git push -u <push_remote> <local_branch>:<push_branch>`).  
Create the PR against the selected base target from `base=<remote>/<branch>`.  
Use `gh` to create draft PR. No confirmation needed.  

### Final handoff

Return:

- Commit list (hash + subject)
- PR URL
- Residual risks or follow-up items

## Quality Bar

Delivery is complete only when:

- There is exactly one commit for the PR
- The commit includes only files related to the issue
- Commit messages explain why, not only what
- PR description is review-friendly and scoped to exactly one issue
- Remote branch and PR are in a clean, shareable state

## Anti-Patterns

Do not:

- Use vague commit subjects like "update stuff"
- Hide risky changes without documenting trade-offs
- Include unrelated local changes in the commit
