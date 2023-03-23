备份调度
==========
PolarDB-X Operator 从 1.4.0 版本开始支持全量备份调度功能。本文介绍如何为集群配置备份调度。

## 注意事项

- 若到达调度时间，对应的PolarDB-X 集群有其他备份正在进行中，则此次备份任务会等进行中的备份任务结束后再开始
- 若同时发起多个调度，请合理制定调度规则，避免同一时间触发多个备份

## 创建备份调度

PolarDBXBackupSchedule 对象的示例如下所示：

```yaml
apiVersion: polardbx.aliyun.com/v1
kind: PolarDBXBackupSchedule
metadata:
  name: pxc-schedule
spec:
  schedule: "*/20 * * * *"
  maxBackupCount: 5
  suspend: false
  backupSpec:
    cluster:
      name: polardbx-test
    retentionTime: 240h
    storageProvider:
      storageName: sftp
      sink: default
    preferredBackupRole: follower
```

参数说明：
* schedule: 调度规则，即定期发起备份的时间点，须使用合规的cron表达式指定
* maxBackupCount: 保存的备份集数量上限，当备份集数超过上限，会从最旧的备份集开始清理，默认值为0，表示不做清理
* suspend: 调度是否暂停，默认为 `false`，表示不暂停
* backupSpec: 备份配置，可参考[集群备份](./2-cluster-backup.md)

参照上述示例编写 pxc-schedule.yaml 文件，通过以下命令创建备份调度：

```bash
kubectl apply -f pxc-schedule.yaml
```

可通过以下命令观察调度状态：

```bash
kubectl get pbs
```

可以从状态中获取到如下信息：

```bash
NAME           SCHEDULE       LAST_BACKUP_TIME       NEXT_BACKUP_TIME       LAST_BACKUP
pxc-schedule   */20 * * * *   2023-03-16T08:00:00Z   2023-03-16T08:20:00Z   polardbx-test-backup-202303160800
```

## 调度规则示例

PolarDBXBackupSchedule 对象的 `spec.schedule` 字段表示调度规则，遵循标准cron表达式的格式要求，下表是一些调度规则的示例：

| 调度规则 | 规则含义 |
| ----- | ------ |
| */20 * * * * | 每20分钟发起备份 |
| 0 * * * * | 每小时发起备份 |
| 0 0 * * 1 | 每周一的0点发起备份 |
| 0 2 * * 1,4 | 周一和周四的2点发起备份 |
| 0 2 */2 * * | 每两天的2点发起备份 |
