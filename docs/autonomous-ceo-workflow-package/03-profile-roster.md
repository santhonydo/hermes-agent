# 03 — Generic Profile Roster

Profiles are agent identities with separate configuration, tools, model routing, memory, and skills. Another team can rename them, but the roles should exist.

## Recommended profiles

| Profile | Purpose | Typical model tier | Tools/skills |
|---|---|---|---|
| `default` / `orchestrator` | CEO-facing router, decomposer, final status owner | Strong reasoning | Kanban, cron, session search, docs |
| `backend_engineer` | APIs, databases, auth, payments, email, infra, deployment | Strong coder | terminal, file, web docs, tests |
| `web_engineer` | Frontend, web UI, landing pages, dashboards, checkout, responsive UX | Strong coder | browser, terminal, file, visual QA helpers |
| `platform_engineer` / `app_engineer` | Mobile/desktop/native/platform-specific implementation | Strong coder | platform SDK docs, build/test tools |
| `qaagent` | Independent QA, review, exact-link visual gates, release gates | Strongest verification model | browser, terminal, file, screenshots, test tools |
| `researcher` | Market, competitor, docs, vendor, compliance, customer research | Accurate researcher | web, browser, note-taking |
| `marketing_ops` | SEO, pSEO, social, content, campaigns, lifecycle email | Medium/cheap unless public-risk high | social scheduler, docs, analytics, email |
| `bizops` | Reports, admin workflows, customer ops, finance ops, internal docs | Medium | docs, spreadsheets, email, CRM |
| `support_ops` | Customer support triage, bug reproduction, feedback routing | Medium | email/chat/search/issue creation |

## Model routing guidance

Use the strongest model where errors are expensive:

- Orchestration decisions
- QA/review/release gates
- Security/legal-sensitive analysis
- Ambiguous product judgment

Use cheaper/faster models for high-volume implementation or routine ops when an independent QA gate catches mistakes.

Recommended policy:

```text
Cron/reporting agents: stable strong baseline model, avoid surprise upgrades.
Engineering + QA: strongest available coding/review model.
Marketing/BizOps: cost-effective model unless public posting or legal-sensitive.
Script-only watchdogs: no model.
```

## Profile-local skills

Do not assume every profile has the same skills. When creating a task:

1. Either verify the target profile has the needed skill.
2. Or embed the required procedure directly in the task body.

If a worker crashes due to missing skills, it is an orchestration error. Replace/relink the card; do not leave it retrying.

## Profile smoke tests

Before relying on a profile:

```bash
agent --profile backend_engineer 'Reply exactly: OK'
agent --profile qaagent 'Reply exactly: OK'
```

Also verify:

- Auth works.
- Model/provider is correct.
- Required toolsets are enabled.
- Working directory is accessible.
- Max turns/runtime is adequate for large tasks.

## Role assignment rules

Example classifier:

| Ask contains | Assign to |
|---|---|
| API, DB, auth, payments, webhook, server, deploy | `backend_engineer` |
| React, page, landing, checkout, frontend, web, responsive | `web_engineer` |
| iOS, Android, desktop app, native helper, build, Xcode/Gradle | `platform_engineer` |
| test, verify, QA, review, screenshot, release gate, not_green | `qaagent` |
| research, compare, market, docs, competitor, vendor | `researcher` |
| SEO, content, social, campaign, post, lifecycle email | `marketing_ops` |
| report, finance, admin, process, customer ops | `bizops` |

Default fallback: orchestrator creates a clarification card or asks the CEO.
