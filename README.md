# Task Launcher Control — v4 (JSONC)

The single source of truth is **[control.jsonc](control.jsonc)**. It contains all tasks and RUN/PAUSE mode in one readable file, with comments showing **every supported setting and schedule form**.

## Editing

Edit `control.jsonc` on `main`, using GitHub's editor. JSONC accepts `//` line comments and `/* … */` block comments; trailing commas are also tolerated. Comments are ignored by the launcher, but are visible to humans, so you can keep explanations and example options directly in the file.

The parser is **strictly v4-only**. There is no alternate `control.json`, v3 support, nested `configuration`, `optionCatalog`, `schemaVersion`, revision or update timestamp. The extension verifies GitHub's file SHA and freshness before launches; malformed or inaccessible controller data **blocks launches**.

## Profiles — no duplicate ID/name

Every entry needs only a **unique `name`** and a **`prompt`**. The extension derives an internal ID from the name for Chrome alarms and run history. No separate public ID is supported. Renaming a task therefore changes its internal identity and starts fresh history for that task; avoid renaming a task while it is actively running. The **first** profile is the default when clicking the extension toolbar icon, so reorder the list if needed.

Optional settings have defaults: `url` = `https://chatgpt.com/` (new chat); `effort` = `current`; `focusMode` = `background`; `sendMethod` = `button`; `enabled` = `true`; `autoClose` = `true`; `schedule` = `manual`. Available alternatives are documented directly in `control.jsonc`. The user can manually run a task even if its `enabled` flag is false, provided the global/local launch gates permit it.

## Schedules

Write one plain-language `schedule` string with one of the following exact patterns:

| Meaning | Example |
| --- | --- |
| Only manual | `"manual"` (or omit schedule entirely) |
| Every N minutes | `"every 60 minutes"` (whole N from 60 to 10,080) |
| Every day at local time | `"daily at 09:00"` |
| Specific weekday at local time | `"weekly on Sun at 09:00"` |
| Multiple weekdays at local time | `"weekly on Sun,Wed at 09:00"` |

Days must be `Sun`, `Mon`, `Tue`, `Wed`, `Thu`, `Fri`, `Sat`; use a comma with no spaces for multiple days. Times are `HH:MM` in the computer's local time zone. Intervals schedule recurring Chrome alarms; daily/weekly follow local wall time. Chrome and your computer must be running/awake for automatic tasks.

The Applied materials weekly task is provisionally set to **Sunday at 09:00 local time**, because no schedule day/time was supplied previously. Adjust it to your preference.

## Privacy and limits

This repository is **public**; every URL, prompt, name and comment is visible to the public. The Applied materials entry includes an existing ChatGPT conversation URL. It does not itself authorize access to the private chat, but exposes its identifier. Avoid such URLs if you prefer not to publish them.

The file allows 1–500 uniquely named profiles (max 512 KiB); each prompt is 1–30,000 characters. All unknown fields, invalid schedule expressions and old formats are rejected. New launches require a fresh verified `RUN`. The file does not launch tasks in the cloud and does not upload run history or results.

**Installation:** Update Task Launcher to the matching v0.14.60 extension, then refresh settings. v3 extensions read the retired `control.json` path and cannot use v4.
