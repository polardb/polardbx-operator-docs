集群恢复
======
PolarDB-X Operator 从 1.3.0 版本开始支持全量备份恢复功能。本文介绍如何通过已有的备份集恢复出 PolarDB-X 集群。

## 恢复 PolarDB-X 集群

```yaml
apiVersion: polardbx.aliyun.com/v1
kind: PolarDBXCluster
metadata:
  name: pxc-restore   # 恢复出的集群名字
spec:
  topology:                         # 集群规格
    nodes:
      cn:
        template:
          image: polardbx/galaxysql:latest
      dn:
        template:
          image: polardbx/galaxyengine:latest
  restore:                          # 指定集群的创建方式是恢复
    backupset: pxcbackup-test       # 使用的备份集的名字
```

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

- restore.backupset表示备份集对应的PolarDBXBackup对象，恢复时须确保该对象存在
- 快速的恢复操作只需在yaml文件中指定希望使用的镜像即可，否则将会使用默认的镜像，更多的规格配置可以参考[集群创建](../lifecycle/1-create.md)
- 目前的恢复功能只支持同构恢复，暂不支持节点数量的变更