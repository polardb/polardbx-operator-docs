除了[几个特殊情况](./7-rollback-exception.md) ，在任意一个变更过程中，你都可以再次变更对象的 `.spec` 字段来触发新的操作，operator 会及时响应以达到预期效果。

因此，中断/回滚上一次操作的方式是再进行一次操作，将 `.spec`改回之前的状态，例如：

```bash
kubectl patch pxc polardbx-test -p '{"spec": {"topology": {"nodes": {"dn": {"replicas": 3}}}}}'
```

之后立刻将 `replicas`改回 2

```bash
kubectl patch pxc polardbx-test -p '{"spec": {"topology": {"nodes": {"dn": {"replicas": 2}}}}}'
```

那么 PolarDB-X 集群将继续稳定运行。
