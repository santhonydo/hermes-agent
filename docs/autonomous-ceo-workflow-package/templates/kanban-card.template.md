# <task title>

## Goal
<What should be true when this task is complete.>

## Source
- Source channel/topic: `<platform:workspace:topic>`
- Source message/task: `<id>`
- Source request: `<summary>`

## Context
- Workdir: `<absolute path>`
- Docs: `<paths>`
- Related parent tasks: `<ids>`
- Concurrent agents: `<yes/no/unknown>`

## Multi-agent coordination
If concurrent agents may be active in this same project/workdir:

- Other agents/models may be changing files at the same time.
- If anything odd happens or files change that you did not change, assume it may be another agent before treating it as corruption.
- Use `hey.md` in the project root to coordinate: read existing notes, leave concise notes before broad/shared edits, and continue working.
- Never block solely waiting for another agent. Narrow scope, re-read changed files, coordinate in `hey.md`, and keep moving toward this task's goal.
- Clean up or archive your resolved `hey.md` messages before completing this task.

## Scope
Do:
- [ ] <task>

Do not:
- [ ] <out of scope>

## Acceptance criteria
- [ ] <criterion>
- [ ] <criterion>

## Verification
Run:
```bash
<commands>
```

Provide:
- Test/build output
- Artifact path / URL / commit
- Screenshots when UI is involved

## Safety gates
Stop and block if the next step requires passwords, 2FA, billing/legal, destructive admin, unapproved public posting, or physical unlocks.

## Completion format
Complete with:
- Outcome
- Evidence
- Artifacts
- Remaining risks/blockers
- Recommended next task if not fully complete
