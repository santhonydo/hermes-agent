# 06 — Controller and Cron Protocol

Cron/controllers are what make autonomy durable when no human is present.

## Controller classes

### 1. Intake router

Watches CEO/general topic and routes actionable messages into boards.

Schedule: every 1–5 minutes.
Delivery: local/silent unless error.

### 2. Board dispatcher watchdog

Checks each active board for ready work and dispatches.

Schedule: every 2–10 minutes.
Delivery: local or product topic only on failure/state change.

### 3. Blocker classifier / recovery router

Classifies blocked/not_green/review-required tasks and creates recovery work.

Schedule: every 5–15 minutes.
Delivery: local unless new user-only blocker or state change.

### 4. Terminal reporter

Reports completed milestones, final QA, release, deploy, or true blockers.

Schedule: every 5–15 minutes.
Delivery: exact product/department topic.

### 5. Business report

Synthesizes portfolio/product status for CEO.

Schedule: daily/weekly.
Delivery: CEO/report topic.

### 6. Script-only operational watchdog

Checks disk, gateway, scheduler, account health, publisher due slots, etc.

Schedule: depends on risk.
Delivery: silent unless threshold crossed.

## Script-only vs agent-driven cron

Use script-only cron when:

- Output shape is deterministic.
- No reasoning is needed.
- Empty stdout should mean silent.
- The script itself decides exactly what to say.

Use agent-driven cron when:

- It needs synthesis, judgment, writing, or planning.
- It must read multiple context surfaces and make tradeoffs.
- It may create/update work dynamically.

## Silence contract

A healthy no-op tick should produce no user-visible message.

Report only:

- New done milestone.
- New blocker.
- Recovery created.
- Watchdog/controller failure.
- Safety gate requiring CEO.
- Material metrics change.

Do not report static topology or already-known routing facts as a fresh outcome. For example, once profile/channel routing is fixed and verified, future reports should not keep repeating that worker profiles remain disabled in chat or that traffic enters through the same orchestrator path. Mention routing/profile topology only when it changed, regressed, blocks work, or the CEO explicitly asks.

## State files

Every repeating script should keep state:

```json
{
  "seen_done": [],
  "seen_blocked": [],
  "routed": [],
  "last_cursor": 0,
  "last_reported_hash": "...",
  "updated_at": "..."
}
```

## Idempotency

Any automated creation should use an idempotency key:

```text
<router-name>:<source-channel>:<source-message-id>:<target-board>:<content-hash>
```

Before creating, search existing tasks/comments/metadata for the key.

## Healthy tick verification

For every controller:

1. Run it manually.
2. Confirm empty stdout when no state changed.
3. Force a dry-run/synthetic event.
4. Confirm it creates or reports exactly once.
5. Confirm second run is silent.

## Noise triage

If the CEO complains about spam:

1. List high-frequency cron jobs.
2. Identify jobs with `deliver=origin` or explicit chat delivery.
3. Move routine controllers to local delivery.
4. Keep only terminal reporters and true blockers chat-visible.
5. Patch scripts to suppress unchanged state.
6. Force-run and verify empty stdout.

## Cron model policy

Recommended:

- Script-only cron: no model.
- Routine synthesis/report cron: stable baseline model.
- Engineering/QA workers: strongest coding/review model.
- Avoid surprise global model upgrades changing existing cron behavior. Pin LLM cron jobs if model spend/quality stability matters.

## Cron prompt rules

Agent-driven cron prompts must be self-contained:

- Mission
- Workdir/docs
- Allowed actions
- Gated actions
- Reporting style
- “Do not schedule or modify cron jobs” unless the job specifically owns scheduling
- Toolsets needed
- Delivery target

## Failure modes

| Symptom | Likely cause | Fix |
|---|---|---|
| Repeated duplicate cards | Missing idempotency | Add key + seen-state |
| Raw cron spam | Wrong delivery / non-empty healthy stdout | Set local, suppress unchanged output |
| Board idle but no report | Reporter title filter stale | Prefix matching + board-idle safety net |
| Work stuck in ready | Unknown profile or dispatcher down | Verify profile exists, dispatch manually |
| Work running but no progress | Fake-running worker | Inspect logs, reclaim, reassign/split |
| Auth crash loops | Profile credential missing/expired | Smoke-test profile and repair auth |
| Recovery cards not running | Parented behind failed QA incorrectly | Complete diagnostic parent or rewire dependencies |
