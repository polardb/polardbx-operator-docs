集群创建卡在 Creating 状态有几种可能的原因

- 组件的 Pod 始终无法 ready，可能的状态可能有 ImagePullBackOff，Pending，CrashBackLoopOff 等
- GMS 中 metadb 的元数据无法准备完成
- 无法从 CN 处获取版本
- ...

排查思路主要是两个：

1. 查看本集群 Pod 状态，看是否有异常状态的 Pod
1. [查看 polardbx-operator 日志](./1-log.md) ，查看是否有对应集群的 ERROR 日志

```bash
kubectl get pods -l polardbx/name={集群名}
```

| Pod 状态                                                                 | 可能的原因                                        | 排查 & 解决思路                                                                                                                                                                          |
|------------------------------------------------------------------------|----------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <ul><li>READY    STATUS</li><li>0/ 3          ImagePullBackOff</li></ul> | 镜像拉取失败 <ul><li>镜像写错了</li><li>私有仓库，没有权限</li></ul> | 使用 `kubectl describe` 进一步确定 <ul><li>镜像写错了，更新 PolarDBXCluster 的 spec</li><li>私有仓库，需要[添加权限](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/)</li></ul> |
| <ul><li>READY    STATUS</li><li>0/ 3          Pending</li></ul> | 资源不足                                         | 使用 `kubectl describe` 进一步确定 <ul><li>添加节点</li><li>腾挪资源</li></ul>                                                                                                                    |
| <ul><li>READY    STATUS</li><li>2/ 3          CrashBackLoopOff</li></ul> | <ul><li>容器反复 crash</li><li>cn 进程挂了</li></ul> | 使用 `kubectl describe` 进一步确定 <ul><li>具体问题具体分析</li><li>describe 看不到错误信息，可以通过[关闭探活](../ops/component/cn/2-liveness.md) 的方式让pod先起来，进入pod 查看相关的日志。</li></ul>                            |

