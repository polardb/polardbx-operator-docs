## 系统组件
1. 指定系统组件镜像 tag 为 v1.0.1：

```bash
helm install --namespace polardbx-operator-system --set imageTag=v1.0.1 polardbx-operator polardbx/polardbx-operator
```

2. 指定拉取策略为 `Always`：

```bash
helm install --namespace polardbx-operator-system --set imagePullPolicy=Always polardbx-operator polardbx/polardbx-operator
```

## 数据库集群

1. 指定所有组件的默认 tag 为 `v1`：

```bash
helm install --namespace polardbx-operator-system --set clusterDefaults.version=v1 polardbx-operator polardbx/polardbx-operator
```

2. 覆盖组件默认 tag，例如指定 `galaxysql`的 tag 为 `v2`（其余组件仍然为 `clusterDefaults.version`的配置）：

```bash
helm install --namespace polardbx-operator-system --set clusterDefaults.galaxysql=galaxysql:v2 polardbx-operator polardbx/polardbx-operator
```

3. 覆盖组件默认 repo，例如指定 `galaxysql`的 repo 为 `registry:5000`（其余组件仍然为 `imageRepo`的配置）：

```bash
helm install --namespace polardbx-operator-system --set clusterDefaults.galaxysql=registry:5000/galaxysql polardbx-operator polardbx/polardbx-operator
```
