# 10 — Operating Checklists

## Daily CEO/operator checklist

- [ ] Review terminal reports, not raw activity.
- [ ] Check true CEO blockers.
- [ ] Decide strategic questions only humans can decide.
- [ ] Verify no high-frequency cron spam.
- [ ] Confirm important boards have active dispatcher/reporter.
- [ ] Check that not-green QA is routed to recovery.

## Weekly systems checklist

- [ ] `profile list` / profile roster still matches expected roles.
- [ ] Smoke-test critical profiles.
- [ ] Audit cron jobs: delivery target, frequency, model, no-agent vs agent.
- [ ] Verify routing registry topic IDs still match real channels.
- [ ] Verify project hold states are respected.
- [ ] Check board DB integrity or equivalent queue health.
- [ ] Review skills/protocol docs for stale instructions.
- [ ] Back up routing registry, docs, and board state if needed.

## New product/department onboarding

- [ ] Create topic/channel.
- [ ] Create board.
- [ ] Create workdir/docs.
- [ ] Add routing registry entry.
- [ ] Assign default owner profile.
- [ ] Add dispatcher watchdog.
- [ ] Add terminal reporter.
- [ ] Add hold policy.
- [ ] Run synthetic CEO intake test.
- [ ] Run no-op healthy tick test.

## CEO ask handling checklist

- [ ] Is this actionable or explanatory?
- [ ] Which project/department owns it?
- [ ] Does it need durable work?
- [ ] Is the project on hold?
- [ ] Which profile owns the first step?
- [ ] Are dependencies needed?
- [ ] Is QA/review required?
- [ ] Is a reporter already in place?
- [ ] Dispatch and inspect.

## Multi-agent same-project checklist

- [ ] Does this board/project have multiple active agents in the same workdir?
- [ ] Did each task card say whether concurrent agents may be active?
- [ ] Does the project root have `hey.md` when concurrent work is active?
- [ ] Did workers read `hey.md` before broad/shared edits?
- [ ] Did workers leave concise coordination notes instead of blocking?
- [ ] Were surprising file/test changes treated as possible peer-agent work before rollback?
- [ ] Were resolved `hey.md` messages cleaned up or archived before task completion?
- [ ] Is durable status still captured in board/tasks, not only `hey.md`?

## Not-green recovery checklist

- [ ] Extract exact QA finding.
- [ ] Create narrow recovery card.
- [ ] Parent follow-up QA behind recovery.
- [ ] Rewire release/final report behind follow-up QA.
- [ ] Archive/reclaim stale duplicate recovery cards.
- [ ] Dispatch.
- [ ] Report only if state changed or CEO needed.

## Cron noise checklist

- [ ] Identify noisy job IDs.
- [ ] Check `deliver` field.
- [ ] Move routine output to local.
- [ ] Patch script healthy tick to empty stdout.
- [ ] Add seen-state/idempotency.
- [ ] Force-run twice: first may report state, second must be silent.

## Profile auth crash checklist

- [ ] Read worker log.
- [ ] Smoke-test profile.
- [ ] Repair auth/credentials.
- [ ] Restart profile/gateway if needed.
- [ ] Reclaim stuck task.
- [ ] Dispatch.
- [ ] Confirm live log activity.

## Public-posting checklist

- [ ] Is channel/account already authorized?
- [ ] Is real publishing enabled by policy?
- [ ] Are legal/billing/brand risks acceptable?
- [ ] Is there a draft/approval trail?
- [ ] Are links/UTMs verified?
- [ ] Is the post within guardrails?
- [ ] Can it be rolled back/deleted if needed?

## Final report checklist

- [ ] Plain-English headline.
- [ ] What changed.
- [ ] Evidence.
- [ ] Business/customer impact.
- [ ] Remaining CEO action, if any.
- [ ] Next step.
- [ ] No raw logs before explanation.
- [ ] No redundant static topology/status facts unless they changed, regressed, block work, or were explicitly requested.
