---
description: Review the current diff for over-engineering. Finds what to delete.
---

$(git diff --cached --diff-filter=ACMR 2>/dev/null || git diff HEAD~1 --diff-filter=ACMR 2>&1)

Review this diff exclusively for unnecessary complexity. Not correctness, not security, not
performance — just bloat.

For each finding, one line:

`L<line>: <tag> <replacement>`

Tags:

- `stdlib` — reimplements something in the standard library
- `platform` — reimplements a native feature
- `dep` — adds a dependency existing ones already cover
- `speculative` — abstraction for a future that isn't here
- `dead` — unreachable or unused code
- `wrapper` — wraps something that was already clean
- `ceremony` — boilerplate with no functional purpose

The diff's best outcome is getting shorter. A single smoke test or assert is the minimum — never flag
it for deletion.
