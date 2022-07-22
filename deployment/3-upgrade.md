使用下面的命令升级系统组件：

```bash
helm upgrade --namespace polardbx-operator-system polardbx/polardbx-operator
```

可以同时指定 values.yaml：

```bash
helm upgrade --namespace polardbx-operator-system -f values.yaml polardbx/polardbx-operator
```

注意，CRD 不会更新，请拉取版本对应的 [CRD 文件](https://github.com/ApsaraDB/galaxykube/tree/main/charts/polardbx-operator/crds) 自行应用。
