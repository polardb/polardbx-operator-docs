修改默认镜像
========
## 系统组件
1. 指定系统组件镜像 tag 为 v1.0.1：

```bash
helm install --namespace polardbx-operator-system --set imageTag=v1.0.1 polardbx-operator polardbx/polardbx-operator --create-namespace
```

2. 指定拉取策略为 `Always`：

```bash
helm install --namespace polardbx-operator-system --set imagePullPolicy=Always polardbx-operator polardbx/polardbx-operator --create-namespace
```

## 数据库集群

1. 指定所有组件的默认 tag 为 `v1`：

```bash
helm install --namespace polardbx-operator-system --set clusterDefaults.version=v1 polardbx-operator polardbx/polardbx-operator --create-namespace
```

2. 覆盖组件默认 tag，例如指定 CN 镜像的 tag 为 `v2`（其余组件仍然为 `clusterDefaults.version`的配置）：

```bash
helm install --namespace polardbx-operator-system --set clusterDefaults.galaxysql=polardbx-sql:v2 polardbx-operator polardbx/polardbx-operator --create-namespace
```

3. 覆盖组件默认 repo，例如指定 CN 的镜像 repo 为 `registry:5000`（其余组件仍然为 `imageRepo`的配置）：

```bash
helm install --namespace polardbx-operator-system --set clusterDefaults.galaxysql=registry:5000/polardbx-sql polardbx-operator polardbx/polardbx-operator --create-namespace
```
