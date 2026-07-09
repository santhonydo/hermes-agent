# 08 — Business Ops Skills Catalog

This workflow works best when agents can load specialized skills. Below is a generic catalog of skills/capabilities that help run an autonomous business.

## Core autonomy and orchestration

| Skill/capability | Use |
|---|---|
| Kanban orchestrator | Decompose CEO asks into durable cards, dependencies, owners, reporters |
| Kanban worker | Execute assigned tasks, update status, block/complete correctly |
| Autonomous product/business ops | Long-running product goals, cron loops, daily reports, blocker policy |
| Cron script hardening | Make script-only watchdogs silent, idempotent, crash-resistant |
| Systematic debugging | Root-cause bugs before patching |
| Test-driven development | Red/green/refactor for risky code changes |
| Writing plans | Turn ambiguous goals into bite-sized implementation plans |
| Requesting code review | Pre-commit security/quality review |
| Subagent-driven development | Parallel specialist review and implementation patterns |

## Engineering

| Skill/capability | Use |
|---|---|
| GitHub repo management | Clone/create/fork repos, remotes, releases |
| GitHub PR workflow | Branch, commit, PR, CI, merge |
| GitHub code review | Review diffs and comments |
| GitHub issues | Create/triage/label issues |
| Codebase inspection | LOC metrics, structure review, risk areas |
| Node inspect debugger | Debug Node/JS services |
| Python debugpy | Debug Python apps |
| Apple-platform app development | Native Apple app workflows |
| Coolify/deployment | Deploy apps and verify production |
| Transactional email delivery | Implement and verify user-facing email |

## QA and browser/desktop verification

| Skill/capability | Use |
|---|---|
| Dogfood / exploratory QA | Find bugs in web apps with evidence |
| Computer use / macOS computer use | Drive native desktop apps safely |
| Browser automation | Open exact links, check UI, console, network |
| OCR and documents | Extract text from screenshots/PDFs |
| Exact-link visual QA pattern | Verify the exact outbound URL/artifact |

## Research and market intelligence

| Skill/capability | Use |
|---|---|
| App store competitive research | Competitor mining and positioning |
| Web research / blog watcher | Monitor markets and feeds |
| YouTube content | Transcript summarization and content research |
| Maps | Local/business/geocoding research |
| Polymarket / market data | Market signals when relevant |
| Academic/arXiv research | Technical literature reviews |

## Marketing, growth, and content

| Skill/capability | Use |
|---|---|
| Social media scheduling | Draft, validate, schedule, approve, publish via connected scheduler |
| Autonomous product business ops | SEO/pSEO/social autonomy and reporting |
| Humanizer | Make copy less generic/AI-sounding |
| Short-form video generation | TikTok/Reels/Shorts workflows |
| Image generation | Ad/landing/social visuals |
| Blog/content watcher | Track competitors and trends |
| Resume/business-specific growth skills | Domain-specific growth ops where applicable |

## Business/admin operations

| Skill/capability | Use |
|---|---|
| Google Workspace | Gmail, Calendar, Drive, Docs, Sheets |
| Airtable | Lightweight database/CRM ops |
| Notion | Docs/database ops |
| Linear | Product issue/project tracking |
| Email / IMAP/SMTP | Search/send/manage email safely |
| PowerPoint / documents / spreadsheets | Investor/customer/internal docs |
| Rental/property/domain-specific ops | Specialized admin workflows where relevant |

## Infrastructure and agent platform

| Skill/capability | Use |
|---|---|
| Hermes Agent | Configure models, profiles, gateway, tools, cron, MCP, skills |
| Native MCP | Add external tools safely |
| Webhook subscriptions | Event-driven agent runs |
| Local operator maintenance | Keep local Mac/operator healthy |
| MCP server integration | Connect schedulers, databases, internal services |

## Skill governance

When a new process succeeds repeatedly, package it as a skill:

- Trigger conditions
- Step-by-step workflow
- Exact commands/tools
- Pitfalls
- Verification steps
- Examples/templates

When a skill is wrong or stale, patch it immediately. Skills are procedural memory; task progress belongs in boards/logs.
