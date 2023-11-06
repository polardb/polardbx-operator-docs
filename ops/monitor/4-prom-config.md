# 配置 Prometheus 和 Grafana
PolarDB-X Monitor 的 helm chart 采用了默认的 Prometheus 和 Grafana 配置，如果您想修改相关配置，可以使用如下的命令安装或者升级 PolarDB-X Monitor，通过 values.yaml 覆盖默认的配置。

```shell
helm install --namespace polardbx-monitor polardbx-monitor polardbx-monitor-1.5.0.tgz -f values.yaml
```

或者：

```shell
helm upgrade --namespace polardbx-monitor polardbx-monitor polardbx-monitor-1.5.0.tgz -f values.yaml
```

values.yaml 文件包含了 Prometheus 和 Grafana 的相关配置项，下面针对常见的几种场景给出配置示例，详细的配置列表详见：[values.yaml](https://github.com/polardb/polardbx-operator/blob/main/charts/polardbx-monitor/values.yaml) 。

### 配置 LoadBalancer
如果您的 K8s 集群支持 LoadBalancer，可以在安装或者升级 PolarDB-X Monitor 的时候通过 -f 参数指定如下配置：

```yaml
monitors:
  grafana:
    serviceType: LoadBalancer
  prometheus:
    serviceType: LoadBalancer
```

### 持久化监控数据
默认配置创建的 Prometheus 集群的监控数据是不持久化的，存在数据丢失的风险，您可以通过如果的values.yaml 指定数据持久化的目录:

```yaml
monitors:
  prometheus:
    persist: true
	# K8s 集群内支持的 storage class
    storageClassName: ssd
	# 存储空间的大小
    storageRequest: 100G
```

### 配置 Prometheus 和 Grafana 规格
默认配置中，Prometheus 集群包含1个节点，每个节点限定8C16G资源，Grafana包含1个节点，每个节点限定4C8G的资源，您可以通过如下配置项修改 Prometheus 和 Grafana集群的规格和节点数量：

```yaml
monitors:
  grafana:
    resources:
      requests:
        cpu: 1000m
        memory: 2Gi
      limits:
        cpu: 2000m
        memory: 8Gi
  prometheus:
    resources:
      requests:
        cpu: 1000m
        memory: 2Gi
      limits:
        cpu: 2000m
        memory: 8Gi
```

