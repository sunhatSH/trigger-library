---
name: daily-backup
enabled: true
schedule:
  every: day        # day | hour | week | month
  at: "02:00"       # "HH:MM" 或 ":MM"
dedup: day          # 去重粒度，默认同 every
---

# daily-backup（定时型 time）

每天 02:00 执行备份。调度由 `triggerctl poll` 检测，触发后通过 agent CLI 执行。

## 执行步骤

1. **确认源目录存在**；不存在则报告并跳过（不要自动创建）。
2. **执行备份**：
   - 推荐：`rsync -a --delete <src>/ <dst>/`（增量、保留权限、删除目标已无源的文件）。
   - 或打 tar 包：`tar czf <dst>/backup-$(date +%Y%m%d).tar.gz <src>/`。
3. **验证备份完整性**（可选）：校验 tarball 用 `tar tzf`，rsync 后对比文件计数。
4. **清理旧备份**：保留最近 7 天（或 30 天），删除更早的归档。
5. **汇报**：备份了什么、大小、耗时、是否成功。

## 约束

- 源只读、目标只写。
- 磁盘空间不足或源缺失时：停下、报告、不要静默失败。
- 不要在备份过程中中断 (`SIGINT`/`SIGTERM` 应完成当前文件再退，但 CLI 调用无法保证；偏好短事务)。

## 配置提示

替换下方占位符后使用：

```bash
# 编辑触发器 body 来设定源/目标
# SRC=/path/to/important/data
# DST=/path/to/backup/location
# RETENTION_DAYS=30
```

## 去重说明

`dedup: day` 确保同一天内即使 poll 多次触发也只执行一次。若要每小时备份但
日级去重，可设 `schedule.every: hour` 配合 `dedup: day`。
