执行如下命令查看 PolarDB-X 集群CN的总体状态：

```shell
kubectl get pxc
```

期望得到如下输出，可以查看CN的数目以及当前ready的数目

```shell
NAME      GMS   CN    DN    CDC   PHASE     DISK       AGE
classic   1/1   2/2   3/3   1/1   Running   40.3 GiB   4d2h
```


执行如下命令获取 cn 的 pod 列表：

```shell
 kubectl get pods -l polardbx/role=cn
```

期望得到如下结果，通过 READY , STATUS 字段判断 cn pod 是否正常。

```shell
NAME                                       READY   STATUS    RESTARTS   AGE
classic-4gss-cn-default-5488d667fd-74lz2   3/3     Running   0          4d2h
classic-4gss-cn-default-5488d667fd-hn7fj   3/3     Running   1          4d2h
```
