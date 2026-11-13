# Task Launcher Control — v3

This public repository contains the **single authoritative** `control.json` for the ChatGPT Task Launcher Chrome extension, format version **3 only**. It provides global RUN/PAUSE and all task profiles. The extension checks GitHub before every launch and periodically refreshes the profiles. Editing this file does not run tasks unless Chrome is open and the local Task Scheduler is enabled.

## Editing

Edit `control.json` directly on `main`. **No revision counter, timestamp, inline option catalog, nested `configuration`, or `schemaVersion` is required.** The GitHub API's file SHA identifies the retrieved file and its HTTP Date/Age headers provide freshness verification. A rollback cannot be detected with a monotonic revision anymore; use Git history for audit or recovery.

Strict JSON only: no comments, trailing commas, unsupported fields or invalid schedules. The extension **does not** understand the old v2 format and fails closed if it encounters it.

## Minimal form

```json
{
  "format": "chatgpt-task-launcher-control",
  "formatVersion": 3,
  "mode": "RUN",
  "profiles": [
    {"name": "My task", "prompt": "Do this task."}
  ]
}
```

`mode` is `RUN` or `PAUSE` (a global stop on new launches, not on already-running jobs). `profiles` is an array of 1–500 entries. `defaultProfileId` is optional; the first profile is the default.

Each profile requires `name` (nonempty, max 90 chars) and `prompt` (1–30,000 chars). Its `id` is optional if the name can be converted into a unique, valid ASCII slug. Explicit IDs remain stable across renames and should be preferred for ongoing scheduled tasks; valid IDs are 1–64 lowercase ASCII letters, digits, underscores or hyphens, starting with a letter/digit.

Profile defaults if omitted: `url` = `https://chatgpt.com/` (new chat), `effort` = `current`, `focusMode` = `background`, `sendMethod` = `button`, `enabled` = `true`, `autoClose` = `true`, and `schedule` = `{"type":"manual"}`. A manual schedule does not run automatically.

Schedule keys depend on `type`:
- `{"type":"manual"}` — manual only.
- `{"type":"interval","minutes":60}` — every 60 minutes (integer 60–10,080).
- `{"type":"daily","time":"09:00"}` — every day, local HH:MM.
- `{"type":"weekly","time":"09:00","days":[0,3]}` — Sunday and Wednesday at local 09:00; day numbers are 0=Sunday through 6=Saturday.

The other allowed values are `effort`: `high`, `medium`, `current`; `focusMode`: `background`, `manual`, `always`; `sendMethod`: `button`, `keyboard`. `enabled:false` disables scheduled runs but still permits a manual Run Now; `autoClose` controls eligible completed tabs. `url` permits a new chat, a temporary chat, or an existing chat URL.

## Existing schedules

`hourly-example` runs Shock Lab every 60 minutes. `applied-materials` has been given a **provisional Sunday 09:00 local schedule**, since the original weekly schedule did not specify a time or weekday. Change these values to your actual preferred time if needed. `manual-example` remains available but disabled.

**Privacy:** This repository is public. All prompt text, profile names, schedules, and included chat URLs can be read by anybody. The Applied materials profile currently publishes an existing ChatGPT conversation identifier. That does not grant access to the conversation by itself, but it exposes the identifier. Prefer a new chat target if you do not want to publish it. The extension never uploads task replies, results, diagnostics, or credentials here.

`RUN` requires a fresh valid file from GitHub. An invalid file, GitHub API error or stale response blocks new launches, preserving the fail-closed behavior.
