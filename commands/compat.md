---
description: Run the PocketBase feature-parity comparison for Grobase nano. Usage: /compat [feature-area]
---

Feature area: $ARGUMENTS

Compare Grobase nano against PocketBase for the given feature area (or all if none given). For a deep,
endpoint-by-endpoint pass, use `/workflow:compat-audit`.

## Workflow

### Phase 1 — Enumerate

- List the PocketBase capabilities in scope and nano's equivalent, citing `wiki/competitive/` (nano-vs-pocketbase, competitive-matrix).

### Phase 2 — Compare

- For each capability: WIN / PARITY / honest-LOSS — the "choose them if" discipline. No invented numbers; cite artifacts.

### Phase 3 — Report

- A markdown table: capability | PocketBase | Grobase nano | verdict.
