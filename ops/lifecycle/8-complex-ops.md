复杂操作指并不是单纯的升级、升配、扩缩容，而是混合了部分或者所有意图的操作方式。Kubernetes 的声明式 API 要求我们高效地实现这样的操作，因此 operator 也支持了。

举个例子，我们将同时

1. 修改 CN 的镜像为 polardbx/galaxysql:v2.0 
2. 修改 CDC 的配置为 8C32G
3. 增加 DN 的节点，到 3 个

同样，我们可以用 `kubectl edit` 和 `kubectl patch` 的方式实现，这里演示 `kubectl patch` 的方式。

首先准备一个 patch 文件，

```yaml
spec:
  topology:
    nodes:
      cn:
        template:
          image: polardbx/galaxysql:v2.0
      dn:
        replicas: 3
      cdc:
        template:
          resources:
            limits:
              cpu: 8
              memory: 32Gi
```

执行下面的命令来进行上述操作：

```bash
kubectl patch pxc polardbx-test --patch-file patch.yaml
```

此时我们将同时观察到 CN、DN、CDC 的变化和数据搬迁：

```bash
kubectl get pxc polardbx-test -o wide
NAME           PROTOCOL   GMS   CN    DN    CDC   PHASE     DISK       STAGE   					REBALANCE   VERSION                            AGE
galaxy-junqi   8.0        1/1   1/2   2/3   1/2   Upgrading 22.6 GiB                                8.0.3-PXC-5.4.13-20220418/8.0.18   35d
```

```bash
kubectl get pxc polardbx-test -o wide
NAME           PROTOCOL   GMS   CN    DN    CDC   PHASE     DISK       STAGE   					REBALANCE   VERSION                            AGE
galaxy-junqi   8.0        1/1   2/2   3/3   2/2   Upgrading 22.6 GiB   RebalanceWatch   50%         8.0.3-PXC-5.4.13-20220418/8.0.18   35d
```

注：如《[不可中断的情况](./7-rollback-exception.md) 》中所说，数据搬迁中不可中断。
