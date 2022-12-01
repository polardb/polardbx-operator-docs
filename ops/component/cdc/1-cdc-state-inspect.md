检查日志节点状态
=====
执行如下命令获取 cdc 的 pod 列表：

```shell
 kubectl get pods -l polardbx/role=cdc

```
期望得到如下结果，通过 READY , STATUS 字段判断 cdc pod 是否正常。

```shell
NAME                                            READY   STATUS    RESTARTS   AGE
tunan-oss-drsg-cdc-default-57d97f5bc8-8z4mz     2/2     Running   0          14d
tunan-oss-drsg-cdc-default-57d97f5bc8-qhvnq     2/2     Running   0          14d
```

