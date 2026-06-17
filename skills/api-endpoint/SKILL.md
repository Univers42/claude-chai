---
name: api-endpoint
description: >
  Scaffold a new REST endpoint across the planes. Auto-triggers on:
  "add an endpoint", "new API route", "expose this over HTTP", "wire a handler"
tools: Read, Write, Bash, Grep
---

# API Endpoint

DO NOT add a route before reading the nearest existing handler and `.claude/rules/api-convention.md`.

## 1. Locate

- Which plane owns it: Go control-plane (`src/go/control-plane/internal/`), TS query/permission (`src/apps/*`), or the Rust data-plane.
- Find the closest existing endpoint; mirror its file, registration, and owner-scoping pattern.

## 2. Design

- Method, path (`/v1/...`), request/response shape, auth (API-key → identity), per-request owner-scope.
- Cloud/enterprise behavior is flag-gated OFF (`if envBool("FLAG")`, default false).

## 3. Implement

- Handler + route registration + the OpenAPI entry in `infra/config/openapi/grobase-public.json`.
- Engine-agnostic: if it touches data, it must hold across all 8 adapters.

## 4. Verify

- Run the plane's check (`make go-control-plane-check` / `make nestjs-ci` / `make rust-data-plane-check`).
- Regenerate SDKs if the spec changed (`cd sdks/js && npm run codegen:all`).
- Add a verify gate `scripts/verify/m<NN>-*.sh` that exercises the route.

## 5. Report

- Files changed, the new route + its auth/owner-scope, and the gate that proves it.
