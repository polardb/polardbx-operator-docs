使用下面的命令获取存储节点 / 元数据节点的 leader 节点：

```bash
kubectl get xstore wuzhe-test2-shmr-dn-3
NAME                    LEADER                          READY   PHASE     DISK      VERSION                                        AGE
wuzhe-test2-shmr-dn-3   wuzhe-test2-shmr-dn-3-cands-0   1/1     Running   1.1 GiB   5.7.14-AliSQL-X-Cluster-1.6.1.1-20220520-log   3m11s
```

其中 `LEADER`列的信息就是 leader 节点所在的 Pod。

如果该列为空，则表示未发现 leader 节点，需要进一步判断是那种情况：

1. [排查是否存储节点对应的 Pod 不在运行中](../ops/component/dn/1-dn-node-state-inspect.md)
1. [排查 Pod 内部的日志](../ops/component/dn/3-dn-log.md)
1. [排查 operator 的日志](./1-log.md)

存储节点的 leader 是通过以下容器内命令来获取的：

```bash
kubectl exec -it wuzhe-test2-shmr-dn-3-cands-0 bash
kubectl exec [POD] [COMMAND] is DEPRECATED and will be removed in a future version. Use kubectl exec [POD] -- [COMMAND] instead.
Defaulted container "engine" out of: engine, exporter, prober

[root@iZ8vb9igdh4szqgoyfjt03Z /]
#xsl consensus role --report-leader
leader
wuzhe-test2-shmr-dn-3-cands-0
```
