几种情况下，operator 无法响应新的操作：

1. `PHASE` 在 `Deleting`状态，意味着在删除中
2. `PHASE` 在 `Locked`状态，意味着在锁定中
3. `PHASE` 在 `Upgrading`状态，且 `STAGE` 在 `RebalanceStart`、`RebalanceWatch`和 `Clean` 状态时，无法中断，意味着此时在进行数据的迁移工作
