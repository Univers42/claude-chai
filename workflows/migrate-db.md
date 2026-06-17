---
description: >
  Author and land a new database migration safely.
  Usage: /workflow:migrate-db <what the migration does>
---

# Migrate DB

Change: $ARGUMENTS

## 1. Design

- Pick the next sequential number in `scripts/migrations/postgresql/` (highest is 065; respect the 057–059 gap).
- Decide engines: postgres (`postgresql/`), mongo (`mongodb/`), mysql (`mysql/`).
- If it backs a cloud/enterprise feature, the table is OFF by default (master + sub-flag AND pattern).

## 2. Author

- `NNN_<slug>.sql` — forward-only, idempotent (`IF NOT EXISTS`, guarded).
- Engine-agnostic intent: a change that works on Postgres but breaks the others is not done.
- **Present the migration. Wait for approval.**

## 3. Apply

- `make migrate` (postgres) / `make migrate-mongo` / `make migrate-mysql` (mongo/mysql need the `data-plane` profile).
- `make migrate-status` to confirm it applied.

## 4. Gate

- Add or extend a verify gate `scripts/verify/m<NN>-*.sh` that exercises the new schema.
- A gate that passes vacuously is not a gate.

## 5. Report

Output: `docs/migrations/db-<date>.md` — the migration number, engines touched, the flag (if any), and the gate that proves it.
