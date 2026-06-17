---
description: Run comparative benchmarks (nano vs PocketBase) and flag regressions. Usage: /bench [load|capacity|footprint|mem|startup]
---

Scope: $ARGUMENTS

Run the canonical benchmark for the given scope on the current branch (Docker-first).

## Workflow

### Phase 1 — Run

- Map scope → target: `make bench-load` | `bench-capacity` | `bench-footprint` | `bench-mem` | `bench-startup`.
- No scope → run the full suite.

### Phase 2 — Analyze

- Read the artifact under `artifacts/bench/` — every cited number comes from there (measured, not claimed).
- Flag any regression over 5% from the baseline.
- Compare read, write, login, boot time, and memory; compare against PocketBase where relevant.

### Phase 3 — Report

- A markdown table with before / after / delta columns and a ✅ / ⚠️ / ❌ status per row.
