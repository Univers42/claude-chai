---
name: debug
description: >
  Structured debugging protocol. Auto-triggers on:
  "this is broken", "bug", "not working", "segfault",
  "panic", "crash", "failing test"
tools: Read, Bash, Grep, Glob
---

# Debug Protocol

DO NOT start changing code. Follow this sequence exactly.

## 1. Reproduce

- Get the exact error message or behavior description
- Write a minimal reproduction (test case or command)
- Confirm the reproduction fails

## 2. Isolate

- Binary search the problem space:
  - Which commit introduced it? (git bisect if needed)
  - Which file? Which function? Which line?
- Read the surrounding code to understand intent

## 3. Understand

- Explain the root cause in one sentence
- Explain WHY the code does the wrong thing, not just WHAT is wrong
- Check if the same pattern exists elsewhere (grep for it)

## 4. Fix

- Minimal change that fixes the root cause
- Not a band-aid, not a workaround
- If the fix is more than 10 lines, explain why

## 5. Verify

- Reproduction test now passes
- Full test suite still passes
- Add a regression test if one didn't exist

## 6. Report

- Root cause (one line)
- Fix applied (one line)
- Files changed
- Tests added
- Related code that might have the same bug
