---
description: >
  Take an external app and migrate it to run entirely on nano.
  Usage: /workflow:onboard-app <repo-url-or-path>
---

# Onboard External App to Nano

Target: $ARGUMENTS

## Phase 1 — Recon (read only, change nothing)

1. Clone or locate the app
2. Identify the stack:
   - Frontend framework (React, Vue, Svelte, etc.)
   - Current backend (Express, Django, Rails, Firebase, Supabase, etc.)
   - Database (Postgres, MySQL, MongoDB, SQLite, etc.)
   - Auth method (JWT, sessions, OAuth providers)
   - File storage (local, S3, etc.)
   - Realtime (websockets, SSE, polling, none)

3. Map every backend call:
   - Extract every API endpoint the frontend hits
   - Extract every data model / schema
   - Extract every auth flow
   - Extract every file upload path
   - List any server-side business logic (cron, webhooks, triggers)

4. Produce a compatibility matrix:

   | Feature           | App uses | Nano supports | Gap |
   | ----------------- | -------- | ------------- | --- |
   | CRUD              |          |               |     |
   | Auth (email/pass) |          |               |     |
   | OAuth providers   |          |               |     |
   | Realtime          |          |               |     |
   | File storage      |          |               |     |
   | API rules / ACL   |          |               |     |
   | Server-side logic |          |               |     |
   | Fulltext search   |          |               |     |
   | Joins / expand    |          |               |     |

5. Verdict: FULL / PARTIAL / BLOCKED
   - FULL = can migrate everything, no custom backend needed
   - PARTIAL = most works, some needs hooks or workarounds
   - BLOCKED = fundamental feature nano can't provide yet

**STOP. Present the matrix. Wait for go/no-go.**

## Phase 2 — Schema migration

1. Generate nano collection schemas (JSON) from the data models
2. Map field types: string, number, bool, email, url, date,
   file, relation, select, json
3. Define API rules per collection (list, view, create, update, delete)
4. Define auth collection if the app has users
5. Import schemas into a fresh nano instance
6. Seed with realistic test data (10 records per collection)

## Phase 3 — Frontend rewiring

1. Install the PocketBase JS SDK (or identify existing SDK usage)
2. Replace every backend call with the PB SDK equivalent:
   - REST calls → pb.collection('x').getList/getOne/create/update/delete
   - Auth → pb.collection('users').authWithPassword / authWithOAuth2
   - Realtime → pb.collection('x').subscribe
   - File uploads → pb.collection('x').create with FormData
3. Update the API base URL to point at nano
4. Remove the old backend dependency entirely

## Phase 4 — Validation

1. Start nano with the migrated schema + seed data
2. Start the frontend dev server
3. Walk through every user flow manually:
   - Sign up / login / logout
   - CRUD on every collection
   - File upload/download
   - Realtime updates (if applicable)
   - Permission checks (can a non-owner edit? should they?)
4. Run the app's existing test suite if it has one
5. Run `/bench` against nano with the new schema under load

## Phase 5 — Report

Output: `docs/migrations/<app-name>.md`

- App name + source URL
- Stack before → after
- Collections created (with field summary)
- Endpoints mapped (count + list)
- Gaps found and how they were handled
- Performance numbers
- Total migration time
- Difficulty rating: trivial / medium / hard / required-custom-hooks
