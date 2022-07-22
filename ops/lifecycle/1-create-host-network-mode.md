
Kubernetes 中容器默认是在 Kubernetes 的容器网络中的，此时网络通信会增加一定的代价和延迟。Kubernetes 支持将容器放到宿主机的网络空间中，这种方式在 Pod 上表现为 `hostNetwork` 为 true。

PolarDBXCluster 也支持将节点的容器放到宿主机网络中，但有几个限制：

- 节点需要监听的端口是随机生成的，不保证不冲突
- 节点升级中可能会遇到端口冲突起不来的情况，需要手动处理

每个组件的`hostNetwork`都在对应的 `template`字段中，可以分别指定

- `spec.topology.gms.template.hostNetwork`
- `spec.topology.cn.template.hostNetwork`
- `spec.topology.dn.template.hostNetwork`
- `spec.topology.cdc.template.hostNetwork`
