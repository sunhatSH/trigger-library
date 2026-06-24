---
name: rest-reminder
enabled: true
inject: false
mode: statusline-only
when: Local time (TRIGGERCTL_TZ_OFFSET, default UTC+8) between 22:00 and 10:00
---

# rest-reminder

> **Statusline-only, never injected into agent context.** This trigger exclusively uses
> `triggerctl install --statusline` to show 🌙 in the status bar during 22:00–10:00.
> No session prompt space is occupied — zero context consumption.
