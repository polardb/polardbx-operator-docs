注：本文升级指修改某个或某几个组件的镜像，实际操作中你可以同时进行升级、升配、扩缩容动作。

以前文[《1. 创建》](./1-create.md) 中的 yaml 为例，假设我们想要更新 CN 的镜像为 `polardbx/galaxysql:v2.0`，那么可以使用 `kubectl edit` 或是 `kubectl patch` 的方式修改 `.spec` 下的镜像字段，这里演示 `kubectl patch`的方式：

```bash
kubectl patch pxc polardbx-test -p '{"spec": {"topology": {"nodes": {"cn": {"template": {"image": "polardbx/galaxysql:v2.0"}}}}}}'
```

稍后观察集群状态，`PHASE`会进入 `Upgrading` 状态，表明正在升级中：

```bash
kubectl get pxc polardbx-test
NAME            GMS   CN    DN    CDC   PHASE      DISK   AGE
polardbx-test   1/1   1/2   2/2   2/2   Upgrading  6.2Gi  93s
```

当 `PHASE`重新变为 `Running`时，升级完成。
