# trigger-library

Official optional triggers for [triggerctl](https://github.com/sunhatSH/triggers).  
**Separate repository** — triggerctl is the engine; this repo is the template store.

Nothing here is auto-installed into your registry. After installing triggerctl:

```bash
triggerctl fetch                 # → ~/.local/share/triggerctl/library
triggerctl list --store
triggerctl add rest-reminder --store
triggerctl add rest-reminder auto-commit-push --store
```

## Sync sources (`triggerctl fetch --source …`)

| SOURCE form | Example |
|---|---|
| Default remote | `triggerctl fetch` → `sunhatSH/trigger-library` |
| GitHub shorthand | `triggerctl fetch --source sunhatSH/trigger-library` |
| Git URL | `triggerctl fetch --source https://github.com/you/triggers.git` |
| Local path | `triggerctl fetch --source /path/to/trigger-library` |

Environment:

- `TRIGGERCTL_LIBRARY` — local fixed directory (default `~/.local/share/triggerctl/library`)
- `TRIGGERCTL_LIBRARY_REMOTE` — default remote for fetch (default `sunhatSH/trigger-library`)

## Ad-hoc source (no fetch)

```bash
triggerctl list --store --source ./session
triggerctl add rest-reminder --store --source /path/to/trigger-library
```

## Layout

```
manifest.yaml     # index for `triggerctl list --store`
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
