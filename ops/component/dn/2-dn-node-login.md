## 登录 Pod
找到需要登录的 DN 的 pod，如果pod 处于 3/3 ready的状态，执行如下命令即可：

```shell
kubectl exec -it {pod 名} bash
```

如果 DN 的 pod 因为探活失败被不停的重启，执行如下命令关闭探活后再登录：

```shell
kubectl annotate pod -l polardbx/role=dn runmode=debug
```

## 进入 MySQL 命令行
执行 `myc` 命令即可
