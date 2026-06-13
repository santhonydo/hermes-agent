# Siri Hermes Voice Companion Implementation Plan

> **For Hermes:** Use subagent-driven-development skill to implement this plan task-by-task.

**Goal:** Let Anthony invoke Hermes from Siri on iPhone with a clean MVP path that starts with Apple Shortcuts and can evolve into a native iOS App Intents companion app.

**Architecture:** Keep Hermes server-side capabilities inside `hermes-agent`; keep Apple client code out of the core repo unless we later decide to productize it as a first-party companion app. Phase 1 uses the existing Hermes API Server (`gateway/platforms/api_server.py`, default local port 8642) or webhook adapter (`gateway/platforms/webhook.py`, default local port 8644) behind a secure tunnel. Phase 2 creates a separate SwiftUI/App Intents app repo that calls that same stable endpoint.

**Tech Stack:** Hermes gateway API/webhook, Cloudflare Tunnel or Tailscale Funnel for HTTPS, iOS Shortcuts, later SwiftUI + App Intents + URLSession + push notifications.

---

## Clean project boundary decision

- **Do not put the iOS app directly in `/Users/anthony/hermes-agent` initially.** This repo is the Hermes framework/runtime; mixing Xcode project state, signing, assets, and iOS release artifacts into it will make both projects noisier.
- **Create a separate project later:** `/Users/anthony/ios-app-business/apps/HermesVoice` or `/Users/anthony/apps/HermesVoice`, depending on where Anthony wants personal iOS apps to live.
- **Keep shared protocol docs in Hermes:** endpoint contract, auth scheme, request/response schema, and Shortcut setup docs can live under `docs/` in `hermes-agent` because they document Hermes integration surfaces.
- **MVP first:** build a Shortcut that POSTs dictated text to Hermes. Only create the native app after the voice loop feels useful.

---

## Phase 1: Shortcuts MVP

### Task 1: Decide endpoint mode

**Objective:** Choose between API Server and Webhook for the first Siri bridge.

**Recommendation:** Use API Server for interactive ask/answer and webhook for fire-and-forget automations.

**Files:**
- Reference: `gateway/platforms/api_server.py`
- Reference: `gateway/platforms/webhook.py`
- Create: `docs/siri-shortcuts-bridge.md`

**Acceptance criteria:**
- We know whether the Shortcut calls `/v1/chat/completions`, `/v1/runs`, or `/webhooks/siri`.
- Endpoint requires a secret or bearer token before any public exposure.

### Task 2: Add Siri bridge documentation

**Objective:** Document the exact iOS Shortcut steps and Hermes endpoint contract.

**Files:**
- Create: `docs/siri-shortcuts-bridge.md`

**Content requirements:**
- Prereqs: gateway running, API/webhook platform enabled, HTTPS tunnel, auth secret.
- Shortcut steps: Dictate Text → Get Contents of URL → Parse JSON → Speak Text.
- Example request body.
- Example response extraction.
- Security notes.
- Long-running task behavior: immediate spoken acknowledgement plus Telegram delivery for artifacts/results.

**Verification:**
- A user can follow the doc on iPhone without guessing field names.

### Task 3: Configure local Hermes endpoint

**Objective:** Ensure the chosen gateway platform can receive local HTTP requests.

**Commands:**
```bash
hermes gateway status
hermes config path
hermes config edit
```

**Expected config shape:**
- For API Server: `platforms.api_server.enabled: true`, host bound to loopback for local testing.
- For Webhook: route `/webhooks/siri` with HMAC secret, rate limit, and a prompt template.

**Verification:**
```bash
curl http://127.0.0.1:8642/health
# or
curl http://127.0.0.1:8644/health
```

### Task 4: Test with curl before Siri

**Objective:** Prove endpoint works without iOS/Shortcuts complexity.

**API Server curl:**
```bash
curl -s http://127.0.0.1:8642/v1/chat/completions \
  -H 'Content-Type: application/json' \
  -H 'X-Hermes-Session-Key: siri-anthony' \
  -d '{"model":"hermes-agent","messages":[{"role":"user","content":"Say pong in one sentence."}],"stream":false}'
```

**Expected:** JSON response containing a short Hermes answer.

### Task 5: Expose HTTPS endpoint safely

**Objective:** Make the local Hermes endpoint reachable by iPhone/Siri.

**Preferred options:**
- Tailscale Funnel if Anthony wants private-device-first networking.
- Cloudflare Tunnel if Anthony wants stable public HTTPS with access controls.

**Security requirements:**
- Do not expose an unauthenticated powerful Hermes endpoint.
- Prefer a narrow bridge endpoint that validates a token and forwards only approved request shapes.
- Rate-limit Siri requests.

### Task 6: Build iOS Shortcut

**Objective:** Create the first usable Siri phrase.

**Shortcut actions:**
1. Dictate Text: prompt “What should I ask Hermes?”
2. Text/Dictionary: build JSON payload.
3. Get Contents of URL: POST to bridge endpoint.
4. Get Dictionary Value: extract response text.
5. Speak Text: read a concise result.
6. Optional: Show Result / Copy to Clipboard.

**Siri phrase:** “Ask Hermes.”

**Verification:**
- Say “Hey Siri, Ask Hermes.”
- Dictate a small request.
- Hermes responds through Siri within acceptable latency.

---

## Phase 2: Native iOS app, separate project

### Task 7: Create separate SwiftUI app

**Objective:** Productize the Shortcut into a real app only after MVP validation.

**Path:** `/Users/anthony/ios-app-business/apps/HermesVoice`

**App features:**
- Settings screen: endpoint URL, auth token, default Hermes session/profile.
- Voice ask screen: native microphone/dictation UI or text input.
- Recent requests/results.
- “Deliver long results to Telegram” toggle.
- App Intents for Siri.

**Apple HIG requirements:**
- Native SwiftUI navigation/settings patterns.
- Dynamic Type and VoiceOver labels.
- Light/dark/increased contrast.
- Privacy explanation before microphone/network usage.

### Task 8: Add App Intents

**Objective:** Let Siri invoke Hermes actions naturally.

**Example intents:**
- `AskHermesIntent(prompt: String)`
- `CheckHermesStatusIntent()`
- `StartMacReachQARunIntent()`

**Verification:**
- Siri can run an App Intent phrase.
- Short answer is spoken.
- Long answers are summarized and delivered elsewhere.

---

## Recommended immediate next action

Start with Phase 1 using the existing Hermes API Server if enabled; otherwise configure a narrow webhook route. Validate via curl, then build the Shortcut manually on Anthony’s iPhone. Create the native app only after we like the loop.
