---
description: >
  Full PocketBase v0.39.3 API compatibility audit.
  Usage: /workflow:compat-audit
---

# PocketBase Compatibility Audit

## 1. API surface extraction

- Fetch the PB v0.39.3 API docs or OpenAPI spec
- List every endpoint: method, path, params, response shape
- Group by: records, auth, files, realtime, settings, admin

## 2. Test generation

For each endpoint:

- Generate an HTTP request that exercises it
- Include: happy path, auth required, forbidden, not found
- Use the PB JS SDK as the client (same as real apps would)

## 3. Execution

- Start nano with a test schema (users + posts + files collections)
- Run every request against nano
- Record: pass (identical response), partial (status matches but
  shape differs), fail (wrong status or crash)

## 4. Gap analysis

For each failure:

- Expected PB behavior (from docs)
- Actual nano behavior
- Severity: breaking (apps crash) / degraded (apps work wrong) /
  cosmetic (different format but functional)

## 5. Report

Output: `docs/compat/audit-<date>.md`

- Total endpoints: X
- Pass: X | Partial: X | Fail: X
- Coverage: X%
- Blocking issues (must fix before claiming PB compatibility)
- Full endpoint-by-endpoint table
