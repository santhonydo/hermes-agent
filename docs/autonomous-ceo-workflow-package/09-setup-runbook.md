# 09 — Setup Runbook for Another Team

## Phase 1 — Define the operating model

1. Name the CEO/general intake channel.
2. Name product/department channels.
3. Decide which work must be durable.
4. Define side-effect gates.
5. Define reporting cadence.
6. Define what should stay silent.

## Phase 2 — Create routing registry

Create `routing-registry.json` from `templates/routing-registry.template.json`.

For each product/department:

- Topic/channel ID
- Board slug
- Workdir/repo/docs path
- Default mode
- Reporter delivery target
- Hold state if any

## Phase 3 — Create profiles

Create at least:

- `orchestrator`
- `backend_engineer`
- `web_engineer`
- `qaagent`
- `researcher`
- `marketing_ops`
- `bizops`

Then smoke-test each profile.

## Phase 4 — Create boards

For each product/department:

1. Initialize board.
2. Set default workdir.
3. Seed initial docs/backlog.
4. Verify `list`, `create`, `show`, `dispatch` work.

## Phase 5 — Install controllers

Minimum controllers:

- CEO intake router
- Portfolio autonomy controller
- Per-board dispatcher watchdog
- Per-board terminal reporter
- Cross-department handoff watcher if relevant
- Gateway/platform liveness alert

Start with delivery `local` until silence/idempotency is proven.

## Phase 6 — Verify with synthetic events

Test cases:

1. Non-actionable CEO question → no card.
2. Actionable product ask → one card on right board.
3. Re-run router → no duplicate.
4. Ready card → dispatcher starts right profile.
5. QA not_green → recovery + follow-up QA created.
6. Engineering done → marketing durable task created.
7. Project on hold → no routing.
8. Terminal done → reporter posts once.
9. Healthy tick → no stdout/message.

## Phase 7 — Document the system

Create durable docs:

- `OPERATING_SYSTEM.md`
- `ROUTING_REGISTRY.md`
- `PROFILE_ROSTER.md`
- `AUTONOMY_PROTOCOL.md`
- `SAFETY_GATES.md`
- `REPORTING_POLICY.md`
- `INCIDENT_RUNBOOK.md`

## Phase 8 — Run pilot

Pick one low-risk product lane. Run for one week.

Measure:

- How many CEO asks routed without manual ticketing.
- Duplicate card rate.
- Silent healthy tick rate.
- Not-green recovery success.
- Time from done to CEO report.
- Spam/noise complaints.
- Human interventions required.

## Phase 9 — Harden

Patch:

- Routing misses.
- Wrong assignee rules.
- Noisy cron jobs.
- Missing QA gates.
- Profile auth failures.
- Stale reporter filters.
- Missing durable docs.

## Phase 10 — Scale

Add new products/departments only when:

- Routing entry exists.
- Board exists.
- Owner profile exists.
- Reporter exists.
- Hold policy exists.
- Synthetic route test passes.
