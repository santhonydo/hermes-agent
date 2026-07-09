# Autonomous CEO Workflow Package

A generic operating system for running an AI-assisted business where the human is the **CEO** and autonomous agents operate as the execution organization.

This package is project-neutral. It does not assume any specific product, repo, customer, channel, or technology stack. It describes the organizational protocol, routing rules, profiles, durable work objects, watchdogs, reporting loops, and safety gates needed for another team to duplicate the workflow.

## Core idea

The CEO communicates intent in normal business channels. The system converts that intent into durable work, routes it to specialized agent profiles, verifies work through independent QA, and reports only meaningful outcomes back to the CEO.

**Human chat is not the source of truth. Durable work items are the source of truth.**

## Package contents

| File | Purpose |
|---|---|
| `01-principles.md` | Operating principles and CEO/agent contract |
| `02-system-architecture.md` | Components: channels, router, Kanban boards, workers, cron, memory |
| `03-profile-roster.md` | Generic subagent profile roster and model-routing guidance |
| `04-autonomous-work-protocol.md` | Full lifecycle from CEO ask → routed work → QA → final report |
| `05-cross-topic-handoff-protocol.md` | How work moves between departments/topics without becoming chat-only |
| `06-controller-and-cron-protocol.md` | Watchdogs, reporters, schedulers, silence/noise policy |
| `07-qa-and-release-gates.md` | Independent verification and not-green recovery |
| `08-business-ops-skills-catalog.md` | Skills/capabilities that help run the business |
| `09-setup-runbook.md` | How another team can install/duplicate this workflow |
| `10-operating-checklists.md` | Daily/weekly/onboarding/incident checklists |
| `11-tools-catalog.md` | Generic tool inventory needed to run the workflow |
| `templates/` | Reusable JSON/Markdown templates |

## Minimum implementation stack

You can implement this package with any tooling, but the reference design assumes:

- A messaging platform with channels/topics/threads.
- A durable work queue or Kanban system with task state, assignees, dependencies, comments, logs, and dispatch.
- Multiple isolated agent profiles or workers with different roles.
- A scheduler/cron system for recurring controllers and reporters.
- Durable local docs or a knowledge base for stable business memory.
- A safe shell/file/web/tool environment for workers.
- Explicit side-effect gates for secrets, 2FA, legal, billing, public posting, and destructive actions.

## CEO-mode summary

The CEO should be able to say:

> Build this, test it, launch it, market it, and tell me only when you need me or when it is actually done.

The workflow should then:

1. Classify the ask.
2. Create or update durable work.
3. Assign the right owner profile.
4. Dispatch execution.
5. Independently QA.
6. Recover from not-green states without waiting for the CEO.
7. Send concise, plain-English terminal outcomes to the right topic.
8. Stay quiet on routine healthy ticks.
