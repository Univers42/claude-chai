---
description: Generate a CHANGELOG entry from commits since the last tag. Usage: /changelog [version]
---

Version: $ARGUMENTS

Generate release notes for the commits since the last tag.

## Workflow

### Phase 1 — Collect

- `$(git log "$(git describe --tags --abbrev=0 2>/dev/null)"..HEAD --pretty='%s' 2>&1)`
- If there is no prior tag, use the full history.

### Phase 2 — Group

- Bucket by Conventional-Commit type: feat, fix, refactor, perf, docs, chore.
- Drop noise (merge commits, WIP, formatting-only).

### Phase 3 — Emit

- Keep-a-Changelog format under a `## [<version>] — <date>` header.
- Sections: Added / Fixed / Changed / Performance.
- No `Co-Authored-By` / "Generated with" trailers (repo rule).
