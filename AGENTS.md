# Multi-agent work in this repo

How to use subagents and the Workflow tool here without making a mess. This standalone repo has **no
orchestrator kernel** (unlike the monorepo's `apps/baas/.claude/`) — keep multi-agent work **lean and
disposable**: fan out for the task, converge, throw the scaffolding away. Do **not** build half a kernel.

## 1. Decompose, then pick a shape

- **Fan out (parallel)** only for genuinely *independent* slices — separate files, separate engines,
  separate review dimensions. No shared write target.
- **Sequence** dependency chains (metering → billing; migrate → verify). Parallelizing them corrupts state.
- **Right-size.** A trivial or conversational task needs zero subagents. Reserve fan-out for breadth
  (sweep many files) or confidence (independent perspectives before an irreversible step).
- **Hybrid is normal:** scout inline to discover the work-list, *then* fan out over it.

## 2. Every subagent gets

- **One job, one "done when."** An objective with no verifiable done-condition is not a task.
- **The context it needs + the binding rules.** Assume it shares none of your memory. State the cwd, the
  paths, and the non-negotiables (§5) explicitly.
- **A schema, when you'll act on the result.** Force structured output so you consume data, not prose.
- **Read-by-query discipline.** Subagents `tail`/`rg`/`jq`/`awk` and return the *conclusion*, never the
  dump. The cheapest read returns only what you need. Logs are JSONL — filter, don't slurp.

## 3. Verify before you trust — and before you act

- **Cross-check claims. UNKNOWN = FAIL.** A finding without evidence (command + output, file + line) is a
  hypothesis, not a fact.
- **Re-verify state right before any destructive or irreversible step.** Files, branches, and data change
  under you — a human may be editing in parallel. A stale inventory is how you clobber someone's work or
  delete the wrong thing. Confirm the target *now*, not from a scan you ran five steps ago.
- **Adversarial pass for high-stakes findings:** spawn skeptics prompted to *refute*; default to refuted
  when uncertain. Diverse lenses (correctness / security / does-it-reproduce) beat N identical voices.

## 4. Converge on a gate

- Funnel parallel work into **one** quality gate — a tester + a reviewer, or a numbered verify gate
  `scripts/verify/m<NN>-*.sh`. A gate that passes vacuously (no-op) is not a gate.
- **Measured, not claimed.** Every perf/capacity statement cites an artifact (`artifacts/bench/…`) + the
  `make` target that reproduces it. No invented numbers.
- Land behind a gate; sync the docs you touched; then stop.

## 5. Non-negotiables (inherited from [`../CLAUDE.md`](../CLAUDE.md))

Every subagent obeys these, even for a one-off slice:

- **Never co-author** a commit/PR (no `Co-Authored-By` / "Generated with").
- **Docker-first** — toolchains run in containers via the root `Makefile`; no host node/cargo/go.
- **Flag-gated OFF by default** — behavior changes stay byte-parity with the OSS baseline until a flag flips.
- **Engine-agnostic** — a fix for one of the 8 adapters that breaks the others is not done.
- **Confirm the irreversible** — pushes, deploys, deletions, npm publish, RS256 cutover → explicit human trigger.
- **Shadow → parity → cutover → delete** — no legacy-TS deletion unless m18 + shadow-parity + CI-forward all PASS.
- **Report faithfully** — failures stated, skips stated; a clean result claimed only when verified.

## 6. Where things live

- Reusable procedures → a `workflows/<name>.md` playbook (human-readable) — not hard-coded here.
- Auto-firing capabilities → a `skills/<name>/SKILL.md`. One-shot actions → a `commands/<name>.md`.
- Durable constraints → a `rules/*.md`. Orientation + conventions → [`README.md`](README.md).
- This repo keeps **one source of truth per concept** — reference it, don't re-document it.
