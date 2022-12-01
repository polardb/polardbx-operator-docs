存活性、可用性配置
=======
为了保证服务的高可用，如果发现某个组件（CN，DN，GMS，CDC）探活失败，K8s 会自动重启对应的pod以恢复服务。
但是在部分场景下我们需要重启cn 进程（修改了某个参数）或者排查进程挂的原因，此时我们不希望K8s 因为探活失败把cn 的pod 删除，那么可以执行如下命令可以关闭所有 CN 节点的探活：

```bash
kubectl annotate pod -l polardbx/role=cn runmode=debug
```

或者只关闭某个cn pod的探活：

```bash
kubectl annotate pod {pod 名} runmode=debug
```

重新打开 cn 的探活：

```bash
kubectl annotate --overwrite pod {pod 名} runmode-
```