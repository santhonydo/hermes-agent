# 11 — Tools Catalog

This is the generic tool inventory another team should provide to make the autonomous CEO workflow work end-to-end. Tool names can differ by platform; the important part is the capability and safety boundary.

## Core agent/system tools

| Tool category | Required capability | Used by |
|---|---|---|
| Profile manager | Create/list/smoke-test isolated agent profiles | Orchestrator, operators |
| Model router | Pin model/provider per profile and per cron job | Platform admin |
| Skill loader | Load reusable procedures/playbooks by task type | All profiles |
| Memory store | Save stable preferences, routing rules, environment facts | Orchestrator |
| Session search | Retrieve prior conversations and delivered reports | Orchestrator, reporters |
| Delegation/subagent runner | Spawn isolated worker agents with task context | Orchestrator |
| Scheduler/cron | Run controllers, watchdogs, reports, and periodic operators | Orchestrator, ops |
| Process manager | Track long-running local processes started by agents | Engineering, QA |
| Gateway/channel manager | Restart/check messaging gateway and delivery channels | Platform admin |

## Work-management tools

| Tool category | Required capability | Used by |
|---|---|---|
| Kanban/task CLI/API | Create/list/show/update/archive tasks | Orchestrator, controllers |
| Dispatcher | Assign ready cards to profile workers | Controller |
| Dependency manager | Parent/child tasks, release gates, follow-up QA | Orchestrator |
| Board reporter | Detect terminal done/blocked/not-green transitions | Reporter cron |
| Idempotency/search | Prevent duplicate routed cards | Routers/watchdogs |

## Developer tools

| Tool category | Required capability | Used by |
|---|---|---|
| Terminal/shell | Run builds, tests, package managers, git, deploy commands | Engineering, QA |
| File read/search/write/patch | Inspect and edit repos/docs safely | All technical profiles |
| Git CLI | Branch, diff, status, commit, inspect history | Engineering, QA |
| GitHub/GitLab CLI/API | Issues, PRs, reviews, releases, CI status | Engineering |
| Language package managers | Install/test/build per stack | Engineering |
| Debuggers | Inspect Python/Node/native runtime failures | Engineering |
| Deployment CLI/API | Deploy, rollback, read logs, health checks | Backend/platform |

## Web and QA tools

| Tool category | Required capability | Used by |
|---|---|---|
| Browser automation | Navigate exact URLs, forms, screenshots, console/network errors | QA, web engineering |
| Desktop/computer-use automation | Operate native apps and desktop workflows safely | QA, platform engineering |
| Screenshot/vision analysis | Verify visual state, layouts, screenshots | QA |
| HTTP/API client | Probe endpoints, webhooks, auth flows | Backend, QA |
| Log readers | Read app/server/build/gateway logs | Engineering, QA |
| Device/simulator tools | Install/run mobile/desktop builds where relevant | Platform engineering, QA |

## Research tools

| Tool category | Required capability | Used by |
|---|---|---|
| Web search | Current facts, vendors, docs, market research | Researcher |
| Web/page extraction | Fetch documentation/articles into markdown/text | Researcher, engineering |
| Official docs integrations | Pull first-party platform/API docs | Engineering, researcher |
| PDF/OCR/document extraction | Read reports, contracts, screenshots, scanned docs | Researcher, bizops |
| Competitive data tools | App stores, search results, pricing pages, ads libraries | Researcher, marketing |

## Communication and business tools

| Tool category | Required capability | Used by |
|---|---|---|
| Messaging delivery | Send reports to exact channel/topic/thread | Reporters |
| Email/IMAP/SMTP | Search, draft, send approved operational email | BizOps, support |
| Calendar | Schedule meetings/reminders/report cadences | BizOps |
| Docs/Drive/knowledge base | Create/update shared operating docs | Orchestrator, bizops |
| Spreadsheets | Track metrics, experiments, customer lists | BizOps, marketing |
| CRM/support inbox | Triage customers and route feedback | Support, product |

## Marketing and growth tools

| Tool category | Required capability | Used by |
|---|---|---|
| Social scheduler | Create drafts, variants, slots, approvals, publish if authorized | Marketing ops |
| Analytics dashboards/API | Read traffic, funnel, attribution, revenue metrics | Marketing, bizops |
| Email marketing/automation | Draft campaigns, automations, test emails, suppression groups | Marketing ops |
| UTM/link builder | Create unique attribution links | Marketing ops |
| Content generation | Draft copy, images, video prompts, creative assets | Marketing ops |
| Asset storage/CDN | Store images/videos/docs and produce shareable URLs | Marketing ops |

## Infrastructure and reliability tools

| Tool category | Required capability | Used by |
|---|---|---|
| Secret manager | Store/retrieve credentials without exposing them in prompts/docs | Platform admin |
| Health checks | Check gateway, cron, disks, ports, services | Watchdogs |
| Audit log | Record side effects, publishing, approvals, cron runs | Operators |
| Backup tooling | Back up routing registry, docs, board state, configs | Platform admin |
| Config validator | Verify profiles/cron/model routing/delivery targets | Platform admin |

## Safety-gated tools

These tools are allowed only inside explicit policy boundaries:

| Tool category | Gate |
|---|---|
| Payment/billing/admin portals | CEO approval required for spend, legal, tax, production admin changes |
| Public posting/publishing | Requires approved channel policy, connected account, and audit trail |
| Email sends to real users | Requires suppression/unsubscribe compliance and approved audience |
| Production database writes | Requires scoped migration/backup/rollback plan |
| Destructive actions | CEO approval required |
| Password/2FA/passkey/CAPTCHA | Human-only; agents must not type or bypass |

## Minimum viable tool stack

A team can start with:

1. Messaging platform with stable topic/thread IDs.
2. Kanban/task API.
3. Profile-capable agent runner.
4. Scheduler/cron.
5. Terminal + file tools.
6. Browser automation for QA.
7. Web search/extract.
8. Git/GitHub or equivalent.
9. Docs/knowledge-base storage.
10. Secret manager and explicit safety gates.

Everything else can be added by department as the workflow matures.
