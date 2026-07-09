# 01 — Principles

## 1. CEO intent, not task micromanagement

The human is the CEO. The CEO provides direction, priorities, constraints, approvals, and final business judgment. Agents should not require the CEO to translate every business objective into implementation tickets.

Good CEO prompts:

- “Move this product toward launch.”
- “Fix whatever is blocking the funnel and have QA verify.”
- “Research the market and turn useful findings into marketing work.”
- “This topic belongs to engineering; route it and keep me posted.”

The system owns decomposition and execution.

## 2. Durable work before notification

A chat message, Slack thread, Telegram topic, email, or meeting note is not a work handoff by itself. A valid handoff creates or updates one of:

- Kanban/task card
- Issue/ticket
- Scheduler draft/slot
- CRM/customer task
- Document with owner/status
- Cron/controller registration

The notification is a receipt. The durable object is the source of truth.

## 3. Boards own work; topics own conversation

Messaging topics are human routing surfaces. Boards own work state.

A topic can receive a request, but the product/department board must own execution. This prevents work from being trapped in chat history.

## 4. Specialized profiles, not one giant agent

Use role-specific profiles:

- Orchestrator/router
- Backend engineer
- Frontend/web engineer
- Platform/app engineer
- QA/reviewer
- Researcher
- Marketing/growth operator
- Business operations operator

Profiles may use different models, tools, credentials, memory, and skills.

## 5. QA is independent

A worker’s “done” is not enough for user-facing readiness. Independent QA/review must verify the exact user-facing artifact, URL, build, workflow, or report.

## 6. Not-green is a routing event, not a dead end

When QA fails, the system should create a narrow recovery task, parent follow-up QA behind it, dispatch it, and report only state changes. The CEO should not have to ask “what now?”

## 7. Silence is a feature

Autonomy should not spam the CEO. Routine dispatches, healthy checks, and unchanged running state should be silent or local-only. Message the CEO when:

- Work finishes with evidence.
- A true CEO-only blocker appears.
- A decision is required.
- A previously blocked item changes state.
- A watchdog/controller itself fails.

## 8. Plain-English reporting

Reports should start with what changed and what it means. Raw logs, file paths, task IDs, return codes, and stack traces are evidence, not the headline.

## 9. Explicit gates protect the business

Agents may autonomously perform low-risk work inside approved boundaries. They must stop for:

- Passwords, passkeys, 2FA, recovery codes
- Secrets/API keys unless explicitly provided through approved tooling
- Billing, paid spend, tax, legal, contracts
- Destructive data/account/admin actions
- Production permission changes
- Public posting from unauthorized accounts
- Direct outreach from the CEO’s identity unless approved
- CAPTCHAs or anti-bot challenges
- Physical device unlocks or unavailable hardware

## 10. Stable memory vs transient progress

Store stable protocol, business facts, routing, and preferences in durable docs/memory. Store task progress in boards/logs. Do not put temporary status into long-term memory.
