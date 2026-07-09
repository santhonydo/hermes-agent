# Cron job template

## Name
`<Company/Product> - <purpose>`

## Schedule
`every 5m` / `0 9 * * *` / etc.

## Mode
- [ ] Script-only/no-agent
- [ ] Agent-driven reasoning

## Delivery
- Routine: `local`
- Terminal outcome: exact topic `platform:workspace:topic`

## Prompt/script contract

```text
You are running a durable autonomous controller/reporter.

Mission:
<mission>

Scope:
<boards/topics/workdirs>

Allowed:
<low-risk actions>

Gated:
passwords, 2FA, billing/legal, destructive admin, unapproved public posting, physical unlocks.

Output policy:
- Empty output means healthy/no delivery.
- Emit only state changes, terminal outcomes, or actionable errors.
- Use plain English; do not lead with raw logs or IDs.

Do not recursively create/modify cron jobs unless this job explicitly owns scheduling.
```

## Verification
- [ ] First run produces expected output or action.
- [ ] Second unchanged run is silent.
- [ ] Duplicate prevention verified.
- [ ] Delivery target verified.
