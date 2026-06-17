---
name: release
description: >
  Cut a release. Auto-triggers on: "release", "cut a version",
  "publish", "tag a release"
tools: Read, Write, Bash
---

# Release

1. Determine version bump (semver) from commits since last tag
2. Update version in all relevant files (Cargo.toml, package.json, go module)
3. Generate changelog (use /changelog command internally)
4. Run full test suite — abort if anything fails
5. Run benchmarks — abort if regression > 5%
6. Create git tag
7. Present summary for approval before pushing
