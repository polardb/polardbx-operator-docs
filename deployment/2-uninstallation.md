卸载 PolarDB-X Operator
========
## 卸载系统组件
使用下面的命令卸载：

```bash
helm uninstall --namespace polardbx-operator-system polardbx-operator
```

使用下面的命令删除命名空间：

```bash
kubectl delete namespace polardbx-operator-system
```

注意事项：

- 卸载后，所有的 `PolarDBXCluster`、`XStore`等相关定制资源无法再自动维护
- 出于资源保护的目的，数据库组件的 Pod 上通常有 finalizer 保护，卸载系统组件意味着删除时无法自动移除 finalizer 和回收资源（例如宿主机磁盘）等，需要手工进行，请谨慎操作

## 卸载定制资源定义 CRD
Helm 的卸载不会同时移除集成的 CRD，如果需要彻底卸载，需要手动移除：

```bash
kubectl get crds | grep -E "polardbx.aliyun.com" | cut -d ' ' -f 1 | xargs kubectl delete crds
```
