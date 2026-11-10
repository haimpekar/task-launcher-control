# Task Launcher Control

This public repository provides the single settings source for the private ChatGPT Task Launcher extension. The extension reads control.json for RUN/PAUSE, profiles, prompts, target URLs, schedules, and supported preferences. It refreshes before each launch and hourly.

The file is public: anyone can read its contents. Do not put secrets, credentials, private conversation URLs, or personal information in it. The extension only reads GitHub; run history, diagnostics, and replies remain local.

Edit control.json on main. Keep its schema and optionCatalog intact, use only supported values, increment revision for every change, and update updatedAt to a canonical UTC timestamp in the same commit. The extension rejects malformed, stale, or rolled-back settings and will not open a task tab until it verifies a valid RUN configuration.
