# 05 — Cross-Topic / Cross-Department Handoff Protocol

## Rule

A cross-topic message is not a handoff. A valid handoff creates or updates durable work first, then notifies the destination topic.

```text
Wrong: “Engineering is done, marketing please take over.”
Right: Create marketing task → attach engineering evidence → dispatch marketing owner → post concise receipt.
```

## Handoff types

| Source | Destination | Durable object |
|---|---|---|
| CEO/general intake | Product board | Kanban card |
| Engineering done | Marketing/growth | Marketing task/campaign draft |
| QA not_green | Engineering | Recovery implementation card |
| Research complete | Product/marketing | Synthesis or implementation card |
| Support/customer feedback | Product | Bug/feedback card |
| Marketing signal | Engineering/product | Product iteration card |
| Release complete | CEO/report topic | Terminal report with evidence |

## Generic intake router

A CEO-topic intake router should:

1. Watch the CEO/general topic.
2. Read only new messages after a cursor.
3. Remove sender prefixes/noise.
4. Ignore explanatory/non-actionable questions unless they contain action verbs.
5. Detect project/department keywords.
6. Look up target board/workdir/topic in registry.
7. Select assignee profile by work type.
8. Create a Kanban card with source metadata.
9. Dispatch target board.
10. Store routed message ID + content hash to avoid duplicates.
11. Emit only errors or optionally concise route receipts.

## Handoff card body template

```markdown
# Routed handoff

Source channel: `<platform:workspace:topic>`
Source message ID: `<id>`
Target board: `<board>`
Target topic: `<platform:workspace:topic>`
Assigned profile: `<profile>`
Idempotency key: `<key>`

## Original request
> <clean CEO or source message>

## Instructions
- Treat this card as the source of truth.
- If the ask needs decomposition, create child cards with dependencies.
- Report terminal outcomes through the destination board/topic reporter.
- Gate secrets, billing, legal, destructive actions, unapproved public posting, and physical unlocks.

## Acceptance criteria
- <criterion 1>
- <criterion 2>
```

## Engineering → Marketing handoff

Trigger when an engineering/release/deploy card completes with a marketable artifact.

Required durable marketing task contents:

- Artifact name and purpose.
- Exact public URL/build/demo path if any.
- QA status and evidence.
- Positioning constraints.
- Audience and suggested angle.
- Approved channels/accounts.
- UTM or attribution requirements.
- What marketing may do autonomously.
- What requires CEO approval.

## QA → Engineering handoff

Trigger when QA returns `not_green`, `review-required`, or `timeout_with_evidence`.

Required recovery card contents:

- QA task ID and verdict.
- Exact failure evidence.
- Reproduction steps.
- Scope of fix.
- Files/areas likely involved.
- Verification commands.
- Follow-up QA parent relationship.

## Research → Execution handoff

A research result is not complete unless it becomes one of:

- Decision memo with explicit recommendation.
- Implementation task.
- Marketing task.
- Backlog item with priority.
- “No action” decision with evidence.

## Loop guards

All automated handoff scripts must prevent recursion:

- Do not route cards already titled `marketing:` back into marketing.
- Do not route handoff receipts as new source tasks.
- Use idempotency keys based on source ID + destination + content hash.
- Store seen IDs in state.
- Respect project hold states.

## Delivery target rule

Avoid ambiguous `origin` for long-lived reporters. Store explicit destination:

```text
platform:workspace_id:topic_or_thread_id
```

If a topic changes, patch the job delivery target and verify with a safe test.
