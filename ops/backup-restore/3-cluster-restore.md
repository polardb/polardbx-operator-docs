集群恢复
======
PolarDB-X Operator 从 1.3.0 版本开始支持全量备份恢复功能。本文介绍如何通过已有的备份集恢复出 PolarDB-X 集群。

## 恢复 PolarDB-X 集群

PolarDB-X 备份集恢复支持两种方式：

* 指定备份集对象进行恢复
* 指定备份集文件进行恢复

### 指定备份集对象进行恢复

这一种方式必须确保备份集对应的`PolarDBXBackup`对象仍然留存在K8S集群中，并且保证远程存储中仍然保存着备份文件。

```yaml
apiVersion: polardbx.aliyun.com/v1
kind: PolarDBXCluster
metadata:
  name: pxc-restore   
spec:
  topology:                         
    nodes:
      cn:
        template:
          image: polardbx/polardbx-sql:latest
      dn:
        template:
          image: polardbx/polardbx-engine:latest
  restore:                          
    backupset: pxcbackup-test
    syncSpecWithOriginalCluster: false
```

参数说明
* topology: 实例规格，可参照[实例创建](../lifecycle/1-create.md)
* restore.backupset: 备份集(备份对象)名称
* restore.syncSpecWithOriginalCluster( 该参数仅适用于 1.4.0 及后续版本 ): 是否保持实例规格和原实例一致，默认取值为`false`，不保持一致；**目前不支持集群异构恢复，这意味着数据节点数会强制与原实例保持一致**


### 指定备份集文件进行恢复

这一种恢复方式仅支持 1.4.0 版本以后产出的备份集，只须保证远程存储中仍然保存着备份文件即可。

```yaml
apiVersion: polardbx.aliyun.com/v1
kind: PolarDBXCluster
metadata:
  name: pxc-restore 
spec:
  topology:
    nodes:
      cn:
        template:
          image: polardbx/polardbx-sql:latest
      dn:
        template:
          image: polardbx/polardbx-engine:latest
  restore:                  
    from:
      backupSetPath: /polardbx/backup/pxcbackup-test
    storageProvider:  
      storageName: sftp
      sink: default
    syncSpecWithOriginalCluster: false
```

参数说明
* topology: 实例规格，可参照[实例创建](../lifecycle/1-create.md)
* restore.from.backupSetPath: 备份集的远程存储路径
* restore.storageProvider: 备份使用的存储配置，可参照[集群备份](./2-cluster-backup.md)
* restore.syncSpecWithOriginalCluster( 该参数仅适用于 1.4.0 及后续版本 ): 是否保持实例规格和原实例一致，默认取值为`false`，不保持一致；**目前不支持集群异构恢复，这意味着数据节点数会强制与原实例保持一致**

参照上述示例编写恢复用的yaml文件，这里需要注意指定创建方式是`restore`，通过以下命令进行恢复：

```bash
kubectl apply -f pxc-restore.yaml
```

可通过以下命令观察恢复进度：

```bash
kubectl get pxc
```

当状态中的`PHASE`变为`RUNNING`后整个恢复流程就完成了

```bash
NAME          GMS   CN    DN    CDC   PHASE     DISK       AGE
pxc-restore   1/1   1/1   2/2   1/1   Running   20.3 GiB   22m
```

## 注意事项

- 快速的恢复操作只需在yaml文件中指定希望使用的镜像即可，否则将会使用默认的镜像，更多的规格配置可以参考[集群创建](../lifecycle/1-create.md)
- 目前的恢复功能只支持同构恢复，暂不支持节点数量的变更