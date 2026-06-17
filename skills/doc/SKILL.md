---
name: doc
description: >
  Generate or update documentation. Auto-triggers on:
  "document this", "update the README", "write docs"
tools: Read, Write, Bash, Grep
---

# Documentation

## 1. Inventory

- What exists: README, inline docs, API docs, architecture docs
- What's stale: compare doc claims against actual code

## 2. Generate

For the target module/file:

- Purpose (one paragraph)
- Usage example (working code, not pseudocode)
- API surface (every public function with signature + doc)
- Configuration (env vars, flags, config fields)
- Architecture decision (why it's built this way)

## 3. Conventions

- English, present tense, imperative ("Returns X" not "This returns X")
- Code examples must compile/run — verify with a test
- No "simply", "just", "easy" — if it were easy nobody'd need docs
- Link to related modules, don't duplicate their docs

## 4. Verify

- Every code example runs
- No dead links
- Spell check
