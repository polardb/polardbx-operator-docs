除了是增加节点以外，其余同[《3. 升级》](./3-update.md) 。

```bash
kubectl patch pxc polardbx-test -p '{"spec": {"topology": {"nodes": {"dn": {"replicas": 3}}}}}'
```

同样 `PHASE`从 `Running`进入 `Upgrading`，然后再回到 `Running`。

```bash
kubectl get pxc polardbx-test
NAME            GMS   CN    DN    CDC   PHASE      DISK   AGE
polardbx-test   1/1   1/2   2/3   2/2   Upgrading  6.2Gi  93s
```

但你会看到 DN 的数量预期变为了 `2/3` 和 `3/3`。同时，operator 会自动进行数据的均衡，因此还可能涉及到数据的搬迁。你可以通过如下方式查看数据搬迁进度：

```bash
kubectl get pxc polardbx-test -o wide
NAME           PROTOCOL   GMS   CN    DN    CDC   PHASE     DISK       STAGE   					REBALANCE   VERSION                            AGE
polardbx-test  8.0        1/1   2/2   3/3   2/2   Upgrading 22.6 GiB   RebalanceWatch   50%         8.0.3-PXC-5.4.13-20220418/8.0.18   35d
```


