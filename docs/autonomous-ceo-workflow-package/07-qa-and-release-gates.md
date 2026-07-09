# 07 — QA and Release Gates

## QA exists to protect CEO trust

Workers are allowed to be optimistic. QA must be skeptical.

A feature is not ready because an implementer says it is ready. It is ready when independent QA verifies the exact artifact the user/customer will touch.

## Gate types

| Gate | Required evidence |
|---|---|
| Unit/integration test | Exact commands and pass/fail counts |
| Build/package | Build command, artifact path, version/build ID, size/hash |
| Web link | Exact URL, screenshot, title/body/h1, console/network errors |
| API | Request/response, auth mode, status, data parity |
| Mobile/desktop app | Installed build/version, device/simulator, launch proof |
| Payment/checkout | Test-mode or approved live evidence, no real spend unless approved |
| Email/campaign | Draft/template/recipient counts, suppression/unsubscribe handling |
| Public post | Scheduler slot/account approval/remote URL, only if authorized |
| Release/deploy | Deployment URL/build ID, health checks, rollback plan |

## Exact-link visual gate

For any user-facing URL:

1. Open the exact URL that will be sent to the CEO/customer.
2. Capture screenshot.
3. Read visible text/title/h1.
4. Check console errors.
5. Check network failures.
6. Verify route/data parity.
7. Verify mobile top-of-page if relevant.
8. Confirm no placeholder/not-found/wrong-page state.

HTTP 200 is not enough. A semantic “not found” page is not green.

## Not-green protocol

When QA finds a failure:

1. Mark QA task `done` or `blocked` with a terminal `not_green` verdict and evidence.
2. Create a narrow recovery task.
3. Parent follow-up QA behind recovery.
4. Rewire release/final-report behind follow-up QA.
5. Dispatch immediately.
6. Report only if user action is needed or a meaningful state changed.

## Review-required protocol

When an implementation worker finishes but asks for review:

1. Do not ship.
2. Create QA/review card with commit/artifact/files/tests.
3. If QA approves, complete/unblock the implementation parent so downstream work can promote.
4. If QA rejects, use not-green protocol.

## Timeout-with-evidence protocol

If QA times out but produced useful findings:

1. Extract findings.
2. Create smaller follow-up QA or recovery tasks.
3. Do not leave broad QA as a permanent blocker.

## True CEO blockers

Only escalate when no autonomous path remains:

- Password/2FA/passkey
- Billing/legal/contract acceptance
- Paid spend
- Production admin permission change
- Destructive account/data action
- Physical hardware/device unavailable
- Strategic decision
- CAPTCHA/anti-bot challenge

Report should include:

- What is blocked.
- Why it requires CEO.
- Exact steps for CEO.
- What work can continue meanwhile.
