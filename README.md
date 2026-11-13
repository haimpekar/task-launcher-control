# Task Launcher controller — JSONC v5

The only public controller is **[control.jsonc](control.jsonc)**. Edit the JSONC file on `main`. Its comments describe the supported options, including the **strict type-dependent schedule objects**. The formatVersion is **5** (no v4/text-schedule compatibility). Unknown fields, missing required fields and invalid schedules block new task launches.

## Profiles

Every profile requires exactly these six fields:
- `name`: user-visible task name; the application derives an internal unique ID
- `prompt`: exact task text, 1–30,000 characters
- `enabled`: boolean, whether automatic runs are enabled (a disabled task remains manually runnable)
- `autoClose`: boolean; safely close an eligible completed tab, not failed/uncertain tabs
- `effort`: one of `"current"`, `"medium"`, `"high"`
- `schedule`: an object in exactly one of the forms below

`url` is optional; default `https://chatgpt.com/`. You may use Temporary Chat or an existing chat URL. The first profile is the toolbar default. **No `id`, `focusMode`, `sendMethod`, or `defaultProfileId` fields.** The application always opens task tabs in the background, and uses one guarded Send-button path with positive UI acceptance checks. Technical UI/submission details are not configurable. Renaming a profile changes its internally generated ID.

## Strict schedule types

| Type | Exact shape | Meaning |
| --- | --- | --- |
| Manual | `{"type":"manual"}` | Only Run Now; never automatically |
| Interval | `{"type":"interval","minutes":60}` | Every 60 minutes; N integer 60–10080 |
| Daily | `{"type":"daily","time":"09:00"}` | Every day at 09:00 local computer time |
| Weekly | `{"type":"weekly","days":["Sun","Wed"],"time":"09:00"}` | Sunday and Wednesday at 09:00 local |

Days are unique strings from `Sun`, `Mon`, `Tue`, `Wed`, `Thu`, `Fri`, `Sat`. `time` uses 24-hour `HH:MM`. Fields that do not belong to the selected schedule type are rejected (e.g. `minutes` on a weekly schedule). No free-text scheduling strings are accepted. Chrome and the computer must be running and awake; GitHub does not run the tasks.

## Safety and privacy

`mode` is `RUN` or `PAUSE` globally. The local Scheduler switch also controls launches. Invalid or inaccessible files block new tasks, and GitHub's verified SHA and response freshness identify the current control. JSONC allows `//`, `/*...*/` comments, and trailing commas, but the file remains strictly validated.

This repository is **public**: prompts, profile names, comments, and target URLs are publicly readable. The Applied materials profile retains an existing conversation URL, including its identifier. Its Sunday 09:00 schedule is provisional and can be changed.

**Update extension to v0.14.61** together with this v5 file; older extension versions do not read this schema.
