## 登录 Pod
如果 CN 处于 ready 状态，执行如下命令即可登录 CN Pod：

```shell
kubectl exec -it {pod 名} bash
```

如果CN pod 因为探活失败处于 Crash 状态，可以通过如下命令关闭探活，让pod 处于 ready 状态后再执行上述命令登录 pod。

```shell
kubectl annotate pod {pod 名} runmode=debug
```

## 登录 CN
登录 pod 后，执行 myc 命令即可进入 MySQL 命令行。
