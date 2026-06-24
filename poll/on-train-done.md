---
name: on-train-done
enabled: true
probe: "test -f /data/done.flag"            # 退出码 0 = 条件成立
dedup_cmd: "stat -c %Y /data/done.flag"     # 事件实例键：同一次完成只触发一次
---

# on-train-done（条件型 event）

约定长任务结束时落一个标志文件 `/data/done.flag`（producer 侧 `touch` 即可，
无需改其内部逻辑）。本触发器由轮询探针检测到该文件后触发。

## 触发后执行

1. **读取产出**（如 `/data/result/`、`/data/logs/` 等）。
2. **运行后续步骤**：
   - 跑评测（如果产出是模型 checkpoint）。
   - 归档结果到持久化存储。
   - 发通知（可写日志，由 agent 汇报给用户）。
3. **清理标志文件**：执行完毕后 `rm -f /data/done.flag`，避免下一轮重复触发。

## 要点

- `probe` 必须 **只读、轻量、快**（每轮 poll 都会跑）。
- `dedup_cmd` 使用 `stat -c %Y`（mtime 秒级时间戳）作为事件实例键：
  同一次 `touch` 产生的 mtime 不变 → 去重生效。
  下一次任务重新 `touch`（mtime 变）→ 新实例，再次触发。
- 标志文件路径可改（如用 `/data/training/batch-001/.done`），同步修改 probe 和 body 中的清理命令。
- 若 producer 在完成前 crash，`done.flag` 永远不会出现，触发器也永远不会误触发——这是期望的。

## 故障排查

- 触发器没触发？确认 `triggerctl poll` 正在运行（或 loop 脚本）。
- 重复触发？检查 `dedup_cmd` 输出是否稳定（`stat -c %Y` 不会变除非重新 `touch`）。
- probe 返回非零？手动跑 `test -f /data/done.flag && echo "ready"` 确认。
