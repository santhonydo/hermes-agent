# 04 — Autonomous Work Protocol

This is the core CEO-mode workflow.

## Lifecycle overview

```text
CEO ask
  → classify scope and risk
  → find/create board
  → create durable task graph
  → dispatch owner profiles
  → worker implements/researches
  → independent QA/review
  → if green: final report / release / marketing handoff
  → if not_green: recovery task + follow-up QA
  → if true CEO blocker: concise blocker report
```

## Step 1 — Classify the ask

Classify by:

1. Product/department.
2. Work type.
3. Risk level.
4. Whether it can complete in one turn.
5. Whether it needs multiple profiles.
6. Whether it should survive restart/crash.

Use Kanban/durable work when any are true:

- Product/customer-facing work.
- Build/fix/ship/deploy/test/release/market/grow.
- Work may outlive the current conversation.
- Multiple profiles are useful.
- Review/QA is expected.
- Audit trail matters.

Direct chat is only for quick answers or explicitly scoped one-turn edits.

## Step 2 — Check routing registry

Find:

- Project key
- Board slug
- Workdir/repo
- Topic/channel to report to
- Owner profile
- Whether project is on hold
- Existing watchdog/reporters

If project is on hold, do not dispatch without explicit CEO resume.

## Step 3 — Decompose into a graph

Create one card per concrete workstream.

Common graph shapes:

### Research → implementation → QA

```text
Research card → Implementation card → QA card → Final report
```

### Parallel implementation + fan-in QA

```text
Backend card ─┐
Frontend card ├─→ QA integration card → Release/report card
Infra card   ─┘
```

### Market/research + product in parallel

```text
Product implementation ─┐
Market research         ├─→ Synthesis / launch plan
Analytics review        ┘
```

### Not-green recovery

```text
QA card: not_green
  → Recovery implementation card
  → Follow-up QA card
  → Final report only after follow-up QA green
```

## Step 4 — Create cards with complete context

Every card should include:

- Goal
- Source request/topic/message ID if applicable
- Workdir/repo/docs
- Constraints and gates
- Exact acceptance criteria
- Expected artifacts
- Verification commands
- Parent/child dependencies
- Reporting target
- What not to do

Use idempotency keys for automatic routers/watchers so repeated ticks do not create duplicates.

## Step 5 — Dispatch and inspect immediately

After creating cards:

1. Dispatch the board.
2. Within 1–2 minutes, inspect statuses/logs.
3. Confirm tasks are running under the intended profiles.
4. Fix unknown-profile, auth, missing-skill, or bad-workdir errors immediately.

Never treat “card created” as progress.

## Step 6 — Multi-agent same-project coordination

When multiple agents may work in the same project/workdir at the same time, every task card and worker prompt must include this coordination instruction:

> Other agents/models may be working in this project at the same time. If anything odd happens, files change, tests shift, or work appears that you did not do, assume it may be another agent before treating it as corruption. Use `hey.md` in the project root to coordinate with other agents. Do not block waiting for a reply; leave concise notes, read existing notes, adapt safely, and keep making progress toward your assigned goal. When your coordination thread is resolved, clean up or archive your messages in `hey.md` so the file does not become stale noise.

### `hey.md` rules

Create `hey.md` at the project/workdir root when concurrent agents are active and it does not already exist.

Use it for short-lived coordination only:

- Announce files/areas you are editing.
- Ask/answer narrow coordination questions.
- Note surprising changes you did not make.
- Warn about commands that may rewrite shared generated files.
- Record temporary handoffs between simultaneously running agents.

Do not use `hey.md` for durable status, CEO reports, secrets, credentials, or long-term documentation. Durable work still belongs in the board/task system.

Recommended message format:

```markdown
## <timestamp> — <profile/task id>
- Working on: <files/area>
- Intent: <one sentence>
- Coordination note: <what changed / what I need / what I am avoiding>
- Cleanup: remove this section when resolved
```

Worker rules:

1. Read `hey.md` before editing shared files when the task says concurrent agents are active.
2. Leave a brief note before broad edits, migrations, generated asset rewrites, dependency changes, or formatting sweeps.
3. Never stop solely because another agent is present. Coordinate, narrow scope, pull/re-read changed files, and continue.
4. If a conflict appears, preserve both agents’ work where possible, then update the durable task with the decision.
5. Remove or archive your resolved notes before completing the task.

## Step 7 — Worker execution contract

Workers must:

- Read the task body and relevant local docs.
- Read `hey.md` when concurrent work is possible.
- Preserve unrelated dirty work.
- Make focused changes.
- Run tests/builds/probes appropriate to the task.
- Commit or produce artifacts when requested by protocol.
- Complete with evidence or block with a precise reason.
- Never claim user-facing success without verification.

## Step 8 — QA contract

QA must independently verify:

- Exact artifact/link/build the user will receive.
- Required test/build commands.
- Screenshots/visual behavior when UI is involved.
- Console/network errors for web apps.
- Physical-device or production evidence when required.
- That the result matches product/design/business requirements.

QA verdicts:

- `green`: ready to proceed.
- `not_green`: concrete fix required.
- `blocked_user`: true CEO/user action required.
- `blocked_external`: external service/hardware/network issue.
- `timeout_with_evidence`: produced useful evidence but needs split/retry.

## Step 9 — Recovery loop

When QA is not green:

1. Extract exact finding.
2. Create a narrow recovery card.
3. Parent follow-up QA behind recovery.
4. Rewire final report/release behind follow-up QA, not the failed QA.
5. Dispatch in same tick.
6. Report only if state changed or CEO input is needed.

Do not wait for the CEO to ask “what now?”

## Step 10 — Final report

A final report must include:

- Outcome in plain English.
- What changed.
- Evidence: tests/builds/screenshots/logs/commit/artifact IDs.
- What it means for the business/customer.
- Any remaining CEO-only action.
- Next executable step if not complete.

Do not lead with raw task IDs. Include them as references only.

## Step 11 — Update durable knowledge

Update only stable docs:

- New routing rule
- New profile role
- New safety policy
- New reusable procedure
- Long-lived decision

Do not store temporary progress in permanent memory.
