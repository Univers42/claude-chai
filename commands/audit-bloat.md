---
description: Scan the whole repo for over-engineering, not just the diff.
---

Scan every source file in the project. For each file, apply the minimalism ladder
(see `rules/minimalism-ladder.md`) and report:

| File | Lines | Bloat lines | Top issue |
| ---- | ----- | ----------- | --------- |

Then list the top 10 worst offenders with specific line-by-line findings in the same one-line format
as `/slim`.

Sort by: most deletable lines first.
