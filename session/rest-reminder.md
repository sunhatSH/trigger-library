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
