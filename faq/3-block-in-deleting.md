卡在 Deleting 状态，一定是因为存在未处理的 finalizer，首先查看 PolarDBXCluster 是否有这样的 finalizer

```bash
kubectl get pxc {PolarDBX 名} -o jsonpath='{.metadata.finalizers}'
["polardbx/finalizer"]
```

通常只有 `polardbx/finalizer` 这一个，应当会由 polardbx-operator 进行处理。如果长时间未处理，需要

- 确定是否 operator 还存活
- 查看 operator 日志来确定原因

如果存在其他 finalizer，需要确定是否有对应组件会处理，

- 如果有，则需要对应组件排查原因
- 否则，使用 `kubectl edit`手动删除对应的 finalizer

## 批量操作
如果 xstore 或者 cn 的数量较多，可以通过如下命令批量操作（操作前建议阅读命令格式，通过标签过滤的方式筛选出需要删除的对象）：

```shell
for i in $(kubectl get xstore -o jsonpath='{.items[*].metadata.name}'); do echo $i; kubectl get xstore $i -o json | jq '.metadata.finalizers = null' | kubectl apply -f -; done
```
