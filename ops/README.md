# PolarDB-X 运维指南

PolarDB-X 集群有 4 个部分组成：元数据服务（GMS）、计算节点（CN）、存储节点（DN）和日志节点（CDC）。每个部分都包含一个或多个计算资源，在 Kubernetes 中以 Pod 的形式呈现。基于 PolarDB-X Operator，我们可以定制集群每一个部分，比如创建 100 个计算节点，或是将 100 个节点分散在 A 和 B 两个可用区来保证高可用等等。

## 标签 (Labels)

在组成 PolarDB-X 集群时，operator 为每个组件赋予了不同的标签，下表展示了一些常用的标签。

| 标签 | 含义 | 可选值 | 示例 | 
| :--- | :--- | :--- | :--- |
| polardbx/name | 资源所属的 PolarDBXCluster 资源的名字 |  | quick-start |
| polardbx/role | 资源的角色 | gms,cn,dn,cdc | cn |

组合这些标签可以选择不同的资源，例如列举 quick-start 集群下的所有 Pod：

```bash
$ kubectl get pods -l polardbx/name=quick-start
NAME                                            READY   STATUS    RESTARTS       AGE
quick-start-ml92-cdc-default-77979c6699-5dfgg   2/2     Running   0              10m
quick-start-ml92-cn-default-6d5956d4f4-jdzr4    3/3     Running   1 (7m9s ago)   10m
quick-start-ml92-dn-0-single-0                  3/3     Running   0              10m
quick-start-ml92-gms-single-0                   3/3     Running   0              10m 
```

或是列举所有的 CN：

```bash
$ kubectl get pods -l polardbx/name=quick-start,polardbx/role=cn
NAME                                           READY   STATUS    RESTARTS       AGE
quick-start-ml92-cn-default-6d5956d4f4-jdzr4   3/3     Running   1 (9m1s ago)   12m
```

## 部署 -- 集群拓扑

为了方便本机测试，[[快速上手](../deployment/0-quickstart.md)] 中展示的集群预先定义了集群的规格和拓扑，将整体资源压缩在 4c8g 以下。

如果想要部署更适合使用的模式，需要自定义集群的拓扑和规格。[[PolarDBXCluster API](../api/polardbxcluster.md)] 中详细解释 PolarDBXCluster 中可配置字段的含义和可选值，你可以参考它进行配置。当然，配置项是比较多且复杂的，这里给出几个简单的例子以供参考：

+ 经典集群 -- 16c64g (2 CN + 2 DN)

```yaml
apiVersion: polardbx.aliyun.com/v1
kind: PolarDBXCluster
metadata:
  name: classic
spec:
  topology:
    nodes:
      cn:
        replicas: 2
        template:
          resources:
            limits:
              cpu: 16
              memory: 64Gi
      dn:
        replicas: 2
        template:
          resources:
            limits:
              cpu: 16
              memory: 64Gi
```

通常建议不设置 resources 的 requests 以使 Kubernetes 能够使 Pod 独享计算资源，你可以参考 [Kubernetes 的文档](https://kubernetes.io/zh/docs/tasks/configure-pod-container/quality-service-pod/) 来了解 Pod 的服务质量的概念。Operator 默认配置中没有为每个容器都指定资源，如需要确保 Pod 是 Guaranteed 的服务质量，需要打开 EnforceQoSGuaranteed 的门特性，可以参考 [[PolarDB-X 安装部署](../deployment/README.md)] 进行配置。

在 Kubernetes 集群资源允许的前提下，可以配置规格更大、节点更多的 PolarDB-X 集群。

## 集群生命周期管理

参考 [[生命周期管理]](./lifecycle/README.md) 对 PolarDB-X 集群全生命周期进行管理，包括创建、升级、扩缩容、删除等。

## 组件管理

参考 [[组件管理]](./component/README.md) 对 PolarDB-X CN、DN 和 CDC 组件进行管理。

## 访问

参考 [[连接 PolarDB-X 数据库]](./connection/README.md) 选择合适的访问方式。

## 配置

参考 [[数据库参数设置]](./configuration/README.md) 来设置和修改配置。

## 监控

参考 [[监控]](./monitor/README.md) 为 PolarDB-X 集群开启监控功能。 