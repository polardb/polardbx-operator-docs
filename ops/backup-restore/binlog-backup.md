增量日志备份
======

PolarDB-X Operator 从 1.4.0 版本开始支持增量日志备份功能。本文介绍如何对 PolarDB-X 进行增量日志备份。
> 此处的增量日志为，DN节点上生成的一致性日志（类似mysql binlog日志），默认在DN容器内的/data/mysql/log目录中

## 前置条件
1. PolarDB-X Operator 升级到 1.4.0 及以上版本
2. 完成备份存储方式配置，参见文档：[备份存储配置](./1-backup-storage-configure.md)


## 发起增量日志备份

下面介绍如何通过 PolarDBXBackupBinlog 对象为 PolarDB-X 进行增量日志备份。

### 创建 PolarDBXBackupBinlog 对象

1. 参照如下示例编写 pxc-backup-binlog.yaml 文件:

```yaml
apiVersion: polardbx.aliyun.com/v1                 # API 组/版本
kind: PolarDBXBackupBinlog                         # API 名称
metadata:
  name: backupbinlogforpolardb-x                    #增量日志备份任务名称
spec:
  pxcName: polardb-x                                # 待备份的目标 PolarDB-X 集群名称
  pxcUid: 8f634de1-5a4e-4e1c-b2dc-e8763384d83a      # 待备份的目标 PolarDB-X 集群名称UID
  remoteExpireLogHours: 168                         # 在远程端(OSS或者SFTP)上的保存小时数
  localExpireLogHours: 7                            # 在数据节点本地的保存小时数
  maxLocalBinlogCount: 60                           # 在数据节点本地保存的增量日志文件保存个数
  pointInTimeRecover: true                          # 是否支持指定时间点恢复
  binlogChecksum: CRC32                             # 增量日志校验码
  storageProvider:                                   
    storageName: oss                                # 存储方式，支持 sftp 和 oss
    sink: osssink                                   # 存储配置项的名称
```

参数说明：
* pxcName: 待备份的目标 PolarDB-X 集群名称, 必填字段
* pxcUid:  待备份的目标 PolarDB-X 集群名称UID，可选字段，一般不填
* remoteExpireLogHours: 在远程端(OSS或者SFTP)上的保存小时数，可选字段，默认值为 168
* localExpireLogHours: 在数据节点本地的保存小时数，可选字段，默认值为 7
* maxLocalBinlogCount： 在数据节点本地保存的增量日志文件保存个数，可选字段，默认值为 60
* pointInTimeRecover： 是否支持指定时间点恢复，可选字段，默认值为 true
* binlogChecksum： 增量日志校验码，可选字段，默认值为 CRC32
* storageProvider.storageName: 备份集存储方式，支持 sftp 和 oss，必填字段
* storageProvider.sink: 备份集存储配置的名称，对应[备份存储配置](./1-backup-storage-configure.md)中的 name 字段，必填字段

2.使用下面的命令创建 PolarDBXBackupBinlog 对象，开启增量日志备份：
```bash
kubectl create -f pxc-backup-binlog.yaml
```
3.查看增量日志备份运行阶段是否为`running`
```bash
kubectl get pxcblog
```

## 增量日志备份查阅

增量日志备份文件存放在如下路径，您可以在 SFTP 配置的主机或者 OSS bucket 中查看对应的文件。

增量日志的元数据文件
```
{root_path}/polardbx-binlogbackup/{namespace}/{pxc_name}/{pxc_uid}/{xstore_name}/{xstore_uid}/{pod_name}/{version}/{batch_name}/binlog-meta/mysql_bin.{number}.txt
```
增量日志文件
```
{root_path}/polardbx-binlogbackup/{namespace}/{pxc_name}/{pxc_uid}/{xstore_name}/{xstore_uid}/{pod_name}/{version}/{batch_name}/binlog-file/mysql_bin.{number}
```

- root_path取决于存储配置
  - 若采用sftp作为存储，则该值为sink.rootPath
  - 若采用oss作为存储，则该值为sink.bucket
- polardbx-binlogbackup为固定字段
- namespace是目标PolarDB-X Cluster所在namesapce
- pxc_name是目标PolarDB-X Cluster的名称
- pxc_uid是目标PolarDB-X Cluster的UID
- xstore_name是备份文件所属的xstore名称
- xstore_uid是备份文件所属的xstore的uid
- pod_name是备份文件所属的pod的名称
- version是备份文件所属的pod的版本号
- batch_name是批目录名称，1000个文件为一批
- binlog-file和binlog-meta为固定字段
- number为增量日志文件的序号