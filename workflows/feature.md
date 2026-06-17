---
description: >
  Full feature implementation cycle.
  Usage: /workflow:feature <description of the feature>
---

# Feature Workflow

Feature: $ARGUMENTS

## 1. Understand

- Restate the feature in one sentence
- List what it changes for the user
- List what it changes in the codebase
- Identify affected modules

## 2. Plan

- Break into tasks (max 5 — if more, the feature is too big, split it)
- For each task: which files change, estimated complexity
- Identify risks or unknowns
- **Present the plan. Wait for approval.**

## 3. Branch

$(git checkout -b feat/$(echo "$ARGUMENTS" | tr ' ' '-' | tr '[:upper:]' '[:lower:]' | head -c 40) 2>&1)

## 4. Implement (per task)

For each task in order:
a. Write the test first (invoke write-test skill)
b. Implement until the test passes
c. Run linter + norm checker
d. Commit: `feat(<scope>): <what>`

## 5. Integration check

- Run full test suite
- Run benchmarks if hot path touched
- Check no unrelated files changed

## 6. Self-review

- Invoke pr-review skill on the branch diff
- Fix every blocker
- Fix every warning if cheap

## 7. Ready

- Push the branch
- Output a PR description: what, why, how, test plan
