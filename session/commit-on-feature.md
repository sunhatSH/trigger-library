---
name: commit-on-feature
enabled: true
when: 开发过程中完成了一个特性时（小特性=仅提交；大特性=提交并推送）
---

# commit-on-feature（会话型 session）

`when` 是**语义条件**——没有 shell 探针能判定"完成了一个特性"，所以轮询器不碰它，由 **Agent 在会话内自检并执行**（做开发的就是 Agent，它知道完成的时刻）。

## 干净前置检查（任一不满足→不自动做，只提醒用户）

1. 在功能分支上（非 `main`/`master`/`release*`，非 detached HEAD）。
2. 不在 merge/rebase/cherry-pick 中途。
3. 改动仅包含当前特性的文件（未混入无关改动）。
4. 无密钥/大文件（`.env`、`credentials.*`、模型权重、数据集等）。

## 触发后（开头标来源 `[触发器: commit-on-feature]`）

- **小特性**（单文件修复、配置调整）：`git add -A` → `git commit`（不推送）。
- **大特性**（多文件里程碑、用户可见变更）：提交后 `git push`（当前分支）。

不确定时按小特性处理。一个特性一个提交，不做半成品 "WIP" 提交。

## 提交后确认

1. 用 `git log -1 --oneline` 确认提交已创建。
2. 如推送到远端，确认推送成功（未被拒绝、无冲突）。
3. 汇报摘要：改动范围、是否推送、目标分支。

## 红线（绝不越界）

- 禁止 `git push --force` 或 `--force-with-lease`。
- 禁止修改已发布的提交（amend published commits）。
- 禁止在此触发器内切换分支或 rebase。
- 推送失败时（无 upstream、冲突、认证问题）：停止并报告，不以 force 重试。
