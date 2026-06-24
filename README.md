# trigger-library

Official optional triggers for [triggerctl](https://github.com/sunhatSH/triggers).  
**Separate repository** — triggerctl is the engine; this repo is the template library.

Nothing here is auto-installed into your registry. After installing triggerctl:

```bash
triggerctl library sync          # clone → ~/.local/share/triggerctl/library
triggerctl library list
triggerctl library install rest-reminder auto-commit-push
```

## Sync sources

| SOURCE form | Example |
|---|---|
| Default remote | `triggerctl library sync` → `sunhatSH/trigger-library` |
| GitHub shorthand | `triggerctl library sync --source sunhatSH/trigger-library` |
| Git URL | `triggerctl library sync --source https://github.com/you/triggers.git` |
| Local path | `triggerctl library sync --source /path/to/trigger-library` |

Environment:

- `TRIGGERCTL_LIBRARY` — local fixed directory (default `~/.local/share/triggerctl/library`)
- `TRIGGERCTL_LIBRARY_REMOTE` — default remote for sync (default `sunhatSH/trigger-library`)

## Layout

```
manifest.yaml     # index for `triggerctl library list`
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

## Ad-hoc source (no sync)

```bash
triggerctl library list --source ./session
triggerctl library install rest-reminder --source /path/to/trigger-library
```
