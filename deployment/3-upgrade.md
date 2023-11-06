升级 PolarDB-X Operator
========

由于 Helm 不会更新 CRD, 因此 PolarDB-X Operator 的升级分为如下两个步骤：
1. 更新 CRD
2. 升级 Operator


### 更新 CRD

1. 请拉取版本对应的 [CRD 文件](https://github.com/polardb/polardbx-operator/tree/main/charts/polardbx-operator/crds)。CRD 文件的拉取可以直接拉取源码，也可以下载 PolarDB-X Operator 对应版本的 [Release 包](https://github.com/polardb/polardbx-operator/releases)，解压后获取。
2. 执行如下命令更新 CRD:
```shell
kubectl apply -f polardbx-operator/crds
```

### 更新本地helm仓库

```bash
helm repo update
```

### 查看仓库中的版本列表

```bash
helm search repo polardbx -l
```

### 升级 Operator

```bash
helm upgrade --namespace polardbx-operator-system polardbx-operator polardbx/polardbx-operator
```

可以同时指定 values.yaml：

```bash
helm upgrade --namespace polardbx-operator-system polardbx-operator -f values.yaml polardbx/polardbx-operator
```