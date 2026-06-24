---
name: auto-commit-push
enabled: true
when: When you finish a feature in development (small feature = commit only; large = commit and push)
---

# auto-commit-push

## Hard rule (must follow every turn)

**Every response that produces or detects changes to track** (edits files, creates files, runs
git commands, or the user says "提交"/"commit"/"push"/"推送") MUST:

1. Check whether this trigger's condition matches the current turn.
2. If it matches → prefix the response with `[Trigger: auto-commit-push]` and follow the steps below.
3. If it does not match → still briefly note why (e.g. "not on feature branch", "no changes").

**Do not wait for a "feature" to feel "complete".** A single commit-worthy change is enough.
The user should never need to explicitly say "commit" — if you made changes, auto-commit.

## Condition

This trigger fires when **any** of these are true:
- You modified/created files in this turn and are about to respond.
- The user said "提交" / "commit" / "推送" / "push" / "更新" / "提交到 git".
- `git status` shows staged or unstaged changes that belong to current work.

## Clean gate (all must pass; if any fail → do not auto-commit, tell the user why)

1. On a feature branch (not `main`/`master`/`release*`, not detached `HEAD`).
2. Not mid merge/rebase/cherry-pick (`MERGE_HEAD`, `rebase-merge/`, etc.).
3. Changes belong to this turn's work only (no unrelated files mixed in).
4. No secrets or huge artifacts (`.env`, `credentials.*`, `*.pkl`, datasets, model weights, `node_modules`).
5. Working tree is not dirty with non-feature changes (e.g. formatting fixes from a different PR).

## After the gate passes

- **Small change** (one-off fix, single-file change, trivial config tweak):
  `git add -A` → `git commit` (no push).
- **Large change** (multi-file milestone, user-facing change, breaking change):
  commit then `git push` (current branch upstream).

When unsure, treat as small (commit only). One logical change → one commit; no half-baked "WIP" commits.

## Post-commit checklist

1. Verify the commit was created (`git log -1 --oneline`).
2. If push was needed, verify push succeeded (no rejected, no conflict).
3. Report summary: what was committed (scope/files), whether pushed, and to which branch.

## Red lines (never cross)

- Never `git push --force` or `git push --force-with-lease`.
- Never amend published commits.
- Never switch branches or rebase as part of this trigger.
- On push failure (no upstream, conflict, auth): stop and report; do not retry with force.
