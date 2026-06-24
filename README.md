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

## Agent compatibility

All triggers work with **Claude Code**, **Hermes Agent**, and **Codex CLI** — the same
registry and semantics are shared across agents. Hook injection and poll execution are
handled by triggerctl's agent abstraction layer.

| Agent | Session hook | Poll execution | Status bar |
|---|---|---|---|
| Claude Code | `UserPromptSubmit` → `triggerctl hook` | `claude -p` (default) | `triggerctl statusline` |
| Hermes | `pre_llm_call` → `triggerctl hermes-hook` | `hermes chat -q` | `triggerctl doctor` |
| Codex CLI | `UserPromptSubmit` → `triggerctl codex-hook` | `codex exec` | `triggerctl doctor` |

Install agent integration:

```bash
triggerctl install --hook          # Claude Code (default)
triggerctl install --hermes        # Hermes Agent (hook + skill)
triggerctl install --codex         # Codex CLI (hook + skill)
```

Each trigger's frontmatter declares compatible agents via the `agents:` field in
`manifest.yaml`. All current triggers support all three agents.

## Layout

```
manifest.yaml     # index for `triggerctl list`
session/          # semantic session triggers (hook)
poll/             # time / event / combo (triggerctl poll)
```

## Triggers

| Name | Kind | Agents | Description |
|---|---|---|---|
| `rest-reminder` | session | claude, hermes, codex | Rest window via statusLine (22:00–10:00) |
| `auto-commit-push` | session | claude, hermes, codex | Commit/push when feature complete |
| `commit-on-feature` | session | claude, hermes, codex | Chinese commit-on-feature template |
| `daily-backup` | time | claude, hermes, codex | Daily backup at 02:00 |
| `on-train-done` | event | claude, hermes, codex | Probe when done.flag exists |
| `gated-nightly` | combo | claude, hermes, codex | Schedule AND probe |
