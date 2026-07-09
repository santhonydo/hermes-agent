# 02 — System Architecture

## Reference components

```text
CEO / humans
   ↓ messages
Messaging topics / channels
   ↓ intake router
Routing registry
   ↓ creates durable work
Kanban boards / task queue
   ↓ dispatcher
Specialist agent profiles
   ↓ artifacts + comments + status
QA / reviewer profiles
   ↓ terminal verdicts
Reporter / controller cron jobs
   ↓ concise updates
CEO / relevant topic
```

## Component responsibilities

### 1. Messaging platform

Purpose:

- Human interface for CEO, product topics, departments, and reports.
- Not the work source of truth.

Requirements:

- Stable channel/topic identifiers.
- Message ingestion into searchable session history or event stream.
- Ability to deliver to exact topic/thread IDs.

### 2. Routing registry

A machine-readable map of:

- Topic/channel → project/department
- Project/department → board
- Board → default workdir/repo/docs
- Work type → owner profile
- Live controllers/reporters → cron job IDs
- Hold/paused states

Use JSON/YAML so scripts and agents can read it.

Recommended fields:

```json
{
  "routing": {
    "platform:workspace:topic": {
      "name": "Human-readable topic name",
      "project": "project-or-department-key",
      "board": "board-slug",
      "workdir": "/absolute/path/to/workspace",
      "default_mode": "kanban_required"
    }
  },
  "profiles": {
    "default": "Orchestrator/router/final report owner",
    "backend_engineer": "Backend/API/DB worker"
  },
  "live_work": [
    {
      "id": "portfolio_autonomy_controller",
      "type": "cron",
      "job_id": "...",
      "script": "autonomy_controller.py",
      "expected_delivery": "local"
    }
  ]
}
```

### 3. Intake router

Purpose:

- Watch a CEO/portfolio topic or general intake channel.
- Detect actionable messages.
- Classify product/department.
- Create a durable work item on the right board.
- Assign an owner profile.
- Dispatch.
- Stay silent on no-op.

It should not route explanatory questions as work.

### 4. Kanban/task board

Required task fields:

- ID
- Title
- Body/instructions
- Assignee profile
- Status: todo/ready/running/blocked/done/archived
- Parent dependencies
- Comments/events
- Worker run logs
- Metadata/idempotency keys

Task dependency rule:

- Independent tasks run in parallel.
- Dependent tasks are created with parent links before dispatch.
- QA/release/final-report tasks are parented behind implementation tasks.

### 5. Dispatcher

Purpose:

- Claim ready tasks.
- Spawn the assigned profile.
- Reclaim stale/running-dead tasks.
- Avoid spawning unknown profiles.

Dispatcher should be safe to run repeatedly.

### 6. Agent profiles

Profiles are isolated worker identities. Each may have:

- Model/provider
- Tools
- Skills
- Credentials
- Memory
- Working directory
- Max turn/runtime budgets

### 7. Controller / watchdog cron

Purpose:

- Keep work moving when no human is present.
- Dispatch ready work.
- Detect stuck, blocked, crashed, fake-running, not-green states.
- Create recovery tasks when possible.
- Report only state changes or true blockers.

### 8. Terminal reporter

Purpose:

- Detect done/blocked/final/release/QA milestones.
- Deliver concise plain-English summaries to the correct human topic.
- Avoid repeating already-seen outcomes.

### 9. Knowledge base

Use durable docs for:

- Operating protocol
- Product strategy
- Routing map
- Profile roster
- Safety gates
- Decisions
- Stable reusable procedures

Do not use it for noisy task progress; boards/logs own that.
