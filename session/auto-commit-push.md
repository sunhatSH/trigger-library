---
name: auto-commit-push
enabled: true
when: When you finish a feature in development (small feature = commit only; large = commit and push)
---

# auto-commit-push

When you judge that a **feature is complete**, manage git proactively — do not ask
"should I commit?" every time. Prefix actions with `[Trigger: auto-commit-push]`.

## Clean gate (all must pass; if any fail → do not auto-commit, tell the user why)

1. On a feature branch (not `main`/`master`/`release*`, not detached `HEAD`).
2. Not mid merge/rebase/cherry-pick (`MERGE_HEAD`, `rebase-merge/`, etc.).
3. Staged or unstaged changes belong to this feature only (no unrelated files mixed in).
4. No secrets or huge artifacts (`.env`, `credentials.*`, `*.pkl`, datasets, model weights, `node_modules`).
5. Working tree is not dirty with non-feature changes (e.g. formatting fixes from a different PR).

## After the gate passes

- **Small feature** (one-off fix, single-file change, trivial config tweak):
  `git add -A` → `git commit` (no push).
- **Large feature** (multi-file milestone, user-facing change, breaking change):
  commit then `git push` (current branch upstream).

When unsure, treat as small (commit only). One feature → one commit; no half-baked "WIP" commits.

## Post-commit checklist

1. Verify the commit was created (`git log -1 --oneline`).
2. If push was needed, verify push succeeded (no rejected, no conflict).
3. Report summary: what was committed (scope/files), whether pushed, and to which branch.

## Red lines (never cross)

- Never `git push --force` or `git push --force-with-lease`.
- Never amend published commits.
- Never switch branches or rebase as part of this trigger.
- On push failure (no upstream, conflict, auth): stop and report; do not retry with force.
