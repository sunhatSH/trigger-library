---
name: rest-reminder
enabled: true
inject: false
mode: statusline-only
cleanup: [statusline]
when: Local time (TRIGGERCTL_TZ_OFFSET, default UTC+8) between 22:00 and 10:00
---

# rest-reminder

> **Statusline-only, never injected into agent context.** This trigger declares
> `cleanup: [statusline]` in frontmatter — triggerctl auto-configures the 🌙 status
> bar at install time, and auto-cleans it from settings.json on remove.
> No session prompt space is occupied — zero context consumption.

## Behavior

When the current local time (respecting `TRIGGERCTL_TZ_OFFSET`, default UTC+8) falls
between **22:00 and 10:00**, the Claude Code status line appends a 🌙 rest window indicator:

```
Sonnet 4.6 · my-project · 23:15  🌙 rest window (23:15)
```

This is purely visual — no model context, no hook injection, no trigger execution.
The agent is **not** instructed to change behavior; the indicator is for the human user.

## How it works

- `triggerctl install --statusline` reads the trigger and auto-configures the status
  line with rest-window logic in `settings.json`.
- On `triggerctl remove rest-reminder` or `triggerctl uninstall`, the statusLine entry
  is automatically cleaned up (the `cleanup: [statusline]` frontmatter drives this).
- The rest window is deterministic (time-only, no model involvement).
