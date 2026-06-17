---
name: incident
description: >
  Bug investigation and post-mortem. Auto-triggers on:
  "post-mortem", "incident", "production issue", "outage"
tools: Read, Bash, Grep, Glob
---

# Incident Investigation

## 1. Timeline

- When did it start?
- When was it detected?
- When was it resolved?
- What was the blast radius?

## 2. Root cause (use the debug skill internally)

## 3. Fix applied

## 4. Action items

For each:

- What to do
- Why it would have prevented this
- Priority: P0 (do now) / P1 (this week) / P2 (backlog)

## 5. Output

Generate an incident report in markdown at docs/incidents/YYYY-MM-DD-<slug>.md
