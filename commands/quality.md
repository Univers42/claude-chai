---
description: Run every strict quality gate in the repo and report PASS/FAIL/SKIP. Usage: /quality [--no-audit] [--with-tests]
---

Args: $ARGUMENTS

Run the full strict gate and report — the static half of "done" (see
`rules/quality-bar.md`). If `.claude/tools/quality.sh` is missing, stop and say so.

## Workflow

### Phase 1 — Run

- Execute `.claude/tools/quality.sh $ARGUMENTS`.
- It is verify-only — it never writes. `--with-tests` adds the test suite,
  `--no-audit` skips the network audits.

### Phase 2 — Report

- Show the table as-is. Don't soften it: ❌ is a blocker, ⚪ is uncovered surface.
- For each ❌: name the `file:line` and the strict rule it breaks.
- For each ⚪ that matters (SAST, audit, a11y): name the tool to install.

### Phase 3 — Fix (only if asked)

- Fixing is the builder's job, under TDD. This command reports; it doesn't mutate the
  tree. If asked to fix, hand off to `agents/builder.md`.
