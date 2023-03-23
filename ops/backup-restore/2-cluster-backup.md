集群备份
======

PolarDB-X Operator 从 1.3.0 版本开始支持全量备份恢复功能。本文介绍如何对 PolarDB-X 进行全量备份。

## 前置条件
1. PolarDB-X Operator 升级到 1.3.0 及以上版本
2. 完成备份存储方式配置，参见文档：[备份存储配置](./1-backup-storage-configure.md)


## 发起全量备份

下面介绍如何通过 PolarDBXBackup 对象为 PolarDB-X 进行全量备份。

### 创建 PolarDBXBackup 对象

1. 参照如下示例编写 pxc-backup.yaml 文件:
```yaml
apiVersion: polardbx.aliyun.com/v1  
kind: PolarDBXBackup
metadata:                   
  name: pxcbackup-test
spec:
  cluster:
    name: polardbx-test             
  retentionTime: 240h
  storageProvider:
    storageName: sftp
    sink: default
  preferredBackupRole: follower
```

参数说明：
* cluster.name: 待备份的目标 PolarDB-X 集群名称
* retentionTime: 备份集保留时间，单位小时
* storageProvider.storageName: 备份集存储方式，支持 sftp 和 oss
* storageProvider.sink: 备份集存储配置的名称，对应[备份存储配置](./1-backup-storage-configure.md)中的 name 字段
* preferredBackupRole( 该参数仅适用于 1.4.0 及后续版本 ): 进行备份的节点角色，可选择 `follower` 和 `leader`，默认为 `follower`；**若使用 `leader` 发起备份，可能会对业务造成影响，请谨慎配置**

2.使用下面的命令创建 PolarDBXBackup 对象，触发全量备份：
```bash
kubectl create -f pxc-backup.yaml
```

### 查看全量备份进度

您可以使用以下指令查看全量备份的进度：
```bash
kubectl get pxb
```

当进度中的`PHASE`变为`Finished`后备份即表示全量备份完成。
```bash
NAME             CLUSTER         START                  END                    RESTORE_TIME           PHASE      AGE
pxcbackup-test   polardbx-test   2022-10-21T04:56:38Z   2022-10-21T04:58:21Z   2022-10-21T04:57:23Z   Finished   4m15s
```
其中，进度里的`RESTORE_TIME`字段表示该备份集可以恢复到的最新的时间点。

### 注意事项

- PolarDBXBackup 对象的metadata.name字段表示备份集的名称，多次构建备份集需要修改该字段
  
## 备份集查阅

全量备份完成后，备份集存放在如下路径，您可以在 SFTP 配置的主机或者 OSS bucket 中查看对应的备份集文件。

```
{root_path}/polardbx-backup/{pxc_name}/{pxc_backup_name}-{timestamp}
```

- root_path取决于存储配置
  - 若采用sftp作为存储，则该值为sink.rootPath
  - 若采用oss作为存储，则该值为sink.bucket
- polardbx-backup为固定字段
- pxc_name是待备份的集群的名字
- pxc_backup_name是备份集的名字
- timestamp是备份开始的时间戳(UTC+0)