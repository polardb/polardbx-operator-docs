## Delete pod 
执行如下命令直接删除 对应的 dn pod，k8s 会自动重建该 pod：

```shell
kubectl delete pod {dn pod名}
```


## Graceful Shutdown 
部分场景下我们需要对 DN 进行graceful的 shutdown然后重启，可先执行如下命令关闭dn的探活：

```shell
kubectl annotate pod {dn pod 名} runmode=debug
```

然后直接登录对应的 dn pod，输入myc 命令进入 MySQL 命令行，执行如下命令：

```shell
mysql> shutdown;
```

即可重启 DN 的进程，此时 dn 的pod 不会重建。

搞完后记得执行如下命令打开探活：

```shell
kubectl annotate --overwrite pod {dn pod 名} runmode-
```

