---
description: >
  Full hardening pass: security, memory, error handling, edge cases.
  Usage: /workflow:harden <module or directory>
---

# Harden Module

Target: $ARGUMENTS

## 1. Attack surface inventory

- List every public function and what input it accepts
- List every external call (network, file, DB, exec)
- List every place user input reaches without validation

## 2. Input validation audit

For every public entry point:

- What's the max length? Is it enforced?
- What characters are allowed? Is it filtered?
- What happens with nil/null/empty/zero?
- What happens with absurdly large input?
- SQL injection, path traversal, command injection — check all

## 3. Memory / resource audit

- Run valgrind (C), go vet + race detector (Go),
  cargo clippy + miri (Rust), or equivalent
- Check every allocation has a matching free
- Check every file open has a matching close
- Check every goroutine/thread has a termination path
- Check every subscription has an unsubscribe

## 4. Error handling audit

- Every function that can fail: does the caller handle it?
- No panic/exit in library code
- Error messages don't leak internals (no stack traces to users,
  no file paths, no SQL in API responses)

## 5. Concurrency audit

- Shared state identified and protected
- No TOCTOU races
- Locks acquired in consistent order (no deadlocks)
- Timeout on every blocking operation

## 6. Fix

- Apply fixes, one commit per category
- Each commit references what was found

## 7. Verify

- Run all tools again — zero issues
- Run full test suite
- Run stress test on the module's endpoints

## 8. Report

Output: `docs/audits/<module>-<date>.md`

- Issues found per category
- Issues fixed
- Issues deferred (with justification)
- Tools used and their output
