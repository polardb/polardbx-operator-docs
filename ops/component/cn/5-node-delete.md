重建所有的 cn 节点，执行如下命令：

```shell
kubectl delete pod -l polardbx/name={实例名},polardbx/role=cn
```

重建单个 cn 节点，执行如下命令：

```shell
kubectl delete pod {pod 名} 
```
