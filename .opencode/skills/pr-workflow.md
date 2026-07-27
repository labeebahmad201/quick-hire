---
name: pr-workflow
description: Use when creating, reviewing, or modifying pull requests. Follows QuickHire's PR workflow.
---

## PR Workflow

### Before creating a PR

1. Run `git status` to check current state
2. Run `git log --oneline -10` to see recent commits
3. Run `git diff` to review all changes

### Commit conventions

Use conventional commits:
- `feat:` new feature
- `fix:` bug fix
- `refactor:` code change without feature/fix
- `docs:` documentation only
- `chore:` maintenance, tooling, dependencies
- `test:` adding or updating tests

### PR requirements

- Every PR must be reviewed via CodeRabbit before merging
- CI must pass (lint, typecheck, tests)
- Include a short description of what and why
- Reference related issues/ADRs if applicable

### Branch naming

- `feat/<short-description>`
- `fix/<short-description>`
- `refactor/<short-description>`
- `docs/<short-description>`

Add examples for the implemented feature along with readable description of the feature implemented.





