# Della — personal AI executive assistant

A mobile-first assistant for someone whose mind moves faster than their organisational system:
brain dump → structure → plan → reminders → check-ins → rewards → weekly review.

## What's here

| Folder | What it is |
|---|---|
| `web/` | The app. `web/src/` is the source; `web/della.html` is the single-file build that runs as a Claude artifact. |
| `mobile/` | Expo (React Native) wrapper that turns the same app into an installable Android APK with phone notifications. See `mobile/README.md`. |
| `test/` | Engine unit tests and browser scenario tests (Playwright). |

## Architecture

- **Deterministic engine** (`web/src/02-engine.js`): tasks, goal hierarchy (goal → milestone → project → weekly objective → task),
  planner, rescheduling, week planning, deadline risk, points, streaks, analytics, document retrieval (BM25).
  All date/time maths lives in `01-core.js` — the model never calculates dates.
- **AI orchestration** (`web/src/03-ai.js`): the model returns structured actions (JSON); the engine executes them.
  Brain dump extraction, goal decomposition, stuck help, onboarding profile, weekly review.
- **UI** (`04-ui.js`, `05-flows.js`, `06-boot.js`): Home, Today, Goals, Assistant, Progress; sheets, check-ins, focus mode.
- **Runtime surface**: the app talks to `window.claude.use("db" | "user" | "sample")`. On claude.ai that's the artifact
  runtime; in the mobile app `web/src/native-shim.js` provides the same surface backed by phone storage and the Anthropic API.

## Build

```sh
sh web/build.sh            # → web/della.html
sh mobile/build-html.sh    # → mobile/della-html.js (for the Android app)
node test/engine.test.js && node test/phase2.test.js
```
