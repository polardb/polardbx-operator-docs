使用下面的命令删除 PolarDBXCluster 集群（对象），其中 `polardbx-test` 是 PolarDBXCluster 对象名

```bash
kubectl delete pxc polardbx-test
```

此时查看对象状态可能会看到 `PHASE`在 `Deleting`，

```bash
kubectl get pxc polardbx-test
NAME            GMS   CN    DN    CDC   PHASE      DISK   AGE
polardbx-test   1/1   2/2   2/2   2/2   Deleting   6.2Gi  2m1s
```

或者报错对象已经不存在

```bash
kubectl get pxc polardbx-test
Error from server (NotFound): polardbxclusters.polardbx.aliyun.com "polardbx-test" not found
```

当 PolarDBXCluster 主实例被删除时，其附属的只读实例也会随之删除
