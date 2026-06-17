# `.claude/` — agent configuration for Grobase

How Claude Code is wired for this repo. Four layers, **smallest scope first** — pick the smallest one
that fits the job.

| Layer | Path | What it is | How it fires |
|---|---|---|---|
| **Rules** | `rules/*.md` | Always-on / glob-scoped coding constraints (the 42 philosophy) | auto, by `alwaysApply` or `globs`; also read by `/refactor` |
| **Commands** | `commands/*.md` | One focused, single-shot action | you type `/<name> <args>` |
| **Skills** | `skills/<name>/SKILL.md` | A capability that auto-triggers on intent | trigger phrase, or by name |
| **Workflows** | `workflows/*.md` | Multi-phase playbooks that orchestrate the layers above | `/workflow:<name> <args>` |

**Rule of thumb:** a constraint that must always hold → **rule**; one shot → **command**; a capability
that should fire on intent → **skill**; a gated, multi-phase procedure → **workflow**.
Multi-agent / subagent work is its own discipline → see [`AGENTS.md`](AGENTS.md).

## Authoring conventions (match the existing good exemplars)

Exemplars to copy: `commands/refactor.md`, `rules/refactor-common.md`, `skills/debug/SKILL.md`,
`workflows/harden.md`. Voice everywhere: terse, imperative, present-tense, hard numbers, no filler
(`simply`/`just`/`easy` are banned). Em-dash `—` for clauses. Tool names in backticks.

- **Rules** — YAML frontmatter, then `# <Tech> Refactoring` + `## Idioms` / `## Patterns` /
  `## After refactoring`. Two shapes, never mixed: universal (`description` + `alwaysApply: true`, no
  globs — only `refactor-common.md`) or tech-scoped (`globs: ["**/*.ext"]` + `description`).
  **Load-bearing naming:** `/refactor <tech>` reads `rules/refactor-<tech>.md` literally — the filename
  spelling must be exact or the command dead-ends.
- **Commands** — frontmatter with one `description:` line ending in `Usage: /<name> <args>`; body opens
  with `<Label>: $ARGUMENTS`; phased `## Workflow` / `### Phase N — <verb>`; self-abort if a required
  file is missing.
- **Skills** — a directory `skills/<name>/` holding exactly `SKILL.md`. **The directory name must equal
  the frontmatter `name`** (lowercase-hyphenated, *no* `.md` suffix). Frontmatter: `name`,
  `description: >` ending in `Auto-triggers on: "phrase", "phrase"`, `tools:` (minimal set). Body: a
  preamble, numbered `## N. <Verb>` phases, last phase is `Report`.
- **Workflows** — frontmatter `description: >` ending in `Usage: /workflow:<name> <args>`; numbered
  `## N. <Phase>`; **one bold human gate** before any behavior change
  (`**Present the plan. Wait for approval.**`); final `## N. Report` naming a dated artifact
  (`docs/<area>/<slug>-<date>.md`). Workflows **reference** skills/commands by name — never re-explain
  them.

## Inherited binding rules (from [`../CLAUDE.md`](../CLAUDE.md))

These bind every command, skill, workflow, and subagent — even one-off tasks:

1. **Never co-author** — no `Co-Authored-By` / "Generated with" trailer.
2. **Docker-first** — drive the stack via the root `Makefile`; no host node/cargo/go for lifecycle.
3. **Flag-gated OFF by default** — cloud/enterprise behavior stays byte-parity with OSS until a flag flips.
4. **Engine-agnostic** — a fix for one of the 8 engine adapters that breaks the others is not done.
5. **Measured, not claimed** — every perf number cites an artifact + the `make` target that reproduces it.
6. **Confirm the irreversible** — pushes, deploys, deletions, RS256 cutover need an explicit human trigger.
7. **Shadow → parity → cutover → delete** — no legacy-TS deletion unless all three gates PASS; UNKNOWN = FAIL.
8. **A gate is the unit of "done"** — land work behind a numbered `scripts/verify/m<NN>-*.sh`.

## settings

- `settings.json` — committed, repo-wide config (permissions / env / hooks). **Must be valid JSON**
  (`{}` at minimum — an empty file fails to parse).
- `settings.local.json` — machine-local toggles (e.g. `disabledMcpjsonServers`); not shared.
