---
name: new-module
description: >
  Scaffold a new module, package, service, or adapter in the hexagonal
  architecture. Auto-triggers on: "create a new module", "add a new service",
  "scaffold", "new package"
tools: Read, Write, Bash
---

# New Module

Scaffold a new module in the hexagonal architecture.

## Inputs

- Module name
- Layer: domain | port | adapter | infrastructure
- Technology: go | rust | typescript

## Steps

1. **Validate** — check the module doesn't already exist.
2. **Scaffold** — create the files inline, mirroring the nearest existing module:
   - Domain layer: types + interfaces (port)
   - Adapter layer: implementation + constructor
   - Test file: table-driven tests with at least 3 cases
3. **Wire** — register the module in the appropriate dependency graph.
4. **Verify** — run the linter and compiler on the new files.
5. **Report** — list every file created with a one-line purpose.

## Constraints

- Follow the naming convention of adjacent modules.
- No circular imports — check with the appropriate tool.
- Every public function has a doc comment.
- Every file has the project header.
