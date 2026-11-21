---
description: Review the changes from a branch.
argument-hint: "<branch>"
---

Review the changes introduced by the target branch $1

# Step-by-Step Process

## 1. Figure out what the base branch is

Use `git branch` to figure out what the base branch is. It should be either `master` or `main`.

## 2. Get the diff

Use the following to get the diff of the change:

```bash
git diff --no-color <base branch>...<target branch>
```

## 3. Get commit messages

Get the commit messages from the new commits:

```bash
git log --no-color --pretty=format:'%h - %s' <base branch>...<target branch>
```

## 4. Review the diff

Review the diff from the step above, focusing on:

- Bugs and logic errors
- Security issues
- Error handling gaps
- Test coverage for new, complex logic.
- Commit messages should be according to conventional commit format

Return your review in a concise form. If no changes are required and no issues are detected, just
reply with "LGTM" and a single sentence summary of the changes.
