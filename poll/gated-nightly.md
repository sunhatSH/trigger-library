---
name: gated-nightly
enabled: true
schedule:
  every: day
  at: "02:00"
probe: "test -f /data/ready"    # 同时声明 schedule + probe => 两者都满足才触发(AND)
---

# gated-nightly（组合型 time+event）

每天 02:00 **且** `/data/ready` 存在时才执行（到点但没 ready 就不跑；
ready 但没到点也不跑）。去重键 = 当日周期键，所以满足条件后当天只触发一次。

## 典型场景

- 夜间训练任务：只在数据准备完毕 (`/data/ready`) 且时间合适时启动。
- 夜间同步/清理：等待上游就绪信号 + 低峰时段执行。
- 定时发布：时间窗口 + 发布审批标志文件。

## 触发后执行

1. **确认 `/data/ready` 仍存在**（避免竞态：检测→执行间可能被消费）。
2. **执行夜间任务**（训练/同步/清理/发布等）：
   - 记录开始时间。
   - 分步执行，每一步汇报进度。
3. **完成后清理**（可选）：`rm -f /data/ready` 防止下次轮询重复消费。
   - 如果 `/data/ready` 是其他流程的生产者标志，则不要删，由生产者管理生命周期。
4. **汇报**：任务结果、耗时、是否成功。

## 设计要点

- combo 型触发器使用 **schedule 的去重键**（这里是 `day`），而非 probe 的 `dedup_cmd`。
  这意味着同一天即使 `/data/ready` 反复出现，也只触发一次。
- 如果需要每次 `/data/ready` 出现都触发，去掉 `schedule` 改用纯 `probe` + `dedup_cmd`。
- `probe` 必须是幂等的——每次 poll 都会执行，不应有副作用。
