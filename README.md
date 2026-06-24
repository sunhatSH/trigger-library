# trigger-library

Official optional triggers for [triggerctl](https://github.com/sunhatSH/triggers).  
**Separate repository** — triggerctl is the engine; this repo is the template store.

Nothing here is auto-installed into your registry. After installing triggerctl:

```bash
triggerctl list
triggerctl install rest-reminder
triggerctl install rest-reminder auto-commit-push
```

First `install` auto-syncs this repo to a fixed local cache (default hidden).

## Install from a specific path

```bash
triggerctl install rest-reminder --from sunhatSH/trigger-library
triggerctl install --from /path/to/trigger-library/session/rest-reminder.md
triggerctl install --from <PATH> --list    # preview only
```

`PATH`: `owner/repo[/path]`, git URL, local directory, or a single `.md`.

Environment:

- `TRIGGERCTL_LIBRARY` — local fixed directory (default `~/.local/share/triggerctl/library`)
- `TRIGGERCTL_LIBRARY_REMOTE` — default remote for auto-sync (default `sunhatSH/trigger-library`)

## Layout

```
manifest.yaml     # index for `triggerctl list`
session/          # semantic session triggers (hook)
poll/             # time / event / combo (triggerctl poll)
```

## Triggers

| Name | Kind | Description |
|---|---|---|
| `rest-reminder` | session | Rest window via statusLine (22:00–10:00) |
| `auto-commit-push` | session | Commit/push when feature complete |
| `commit-on-feature` | session | Chinese commit-on-feature template |
| `daily-backup` | time | Daily backup at 02:00 |
| `on-train-done` | event | Probe when done.flag exists |
| `gated-nightly` | combo | Schedule AND probe |
