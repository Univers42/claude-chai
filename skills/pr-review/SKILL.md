---
name: pr-review
description: >
  Full quality audit before a PR/merge. Auto-triggers on:
  "review this PR", "is this ready to merge", "pre-merge check"
tools: Read, Bash, Grep, Glob
---

# PR Review

## Phase 1 — Scope

`$(git diff --stat main...HEAD 2>&1)`

List every changed file. Categorize: new | modified | deleted.

## Phase 2 — Correctness

- Read every changed file in full
- Check for logic errors, off-by-ones, race conditions
- Verify error handling on every new fallible path
- Check for resource leaks (memory, FDs, goroutines, subscriptions)

## Phase 3 — Norm compliance

- Run the appropriate linter/norm checker per technology
- Check line counts, function counts, parameter counts
- Flag any violation of .claude/rules/refactor-common.md

## Phase 4 — Tests

- Run the full test suite
- Check if new code has test coverage
- Flag any changed function without a corresponding test change

## Phase 5 — Performance

- If any hot-path file changed, require benchmark numbers
- Check for O(n²) patterns, unnecessary allocations, missing caches

## Phase 6 — Report

Output a markdown table:

| Category | Pass/Fail | Issues |
| -------- | --------- | ------ |

List every issue with file:line and severity (blocker/warning/nit).
Blockers must be fixed. Warnings should be fixed. Nits are optional.
