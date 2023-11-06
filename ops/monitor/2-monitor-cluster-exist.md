# 为存量集群开启监控
PolarDB-X 企业版的监控采集功能默认是关闭的，需要配置开启

## 为存量 PolarDB-X 企业版开启监控
本节介绍如何为 PolarDB-X 企业版集群开启监控。执行如下命令, 您需要监控的 PolarDBXCluster 创建 PolarDBXMonitor对象,

```bash
kubectl apply -f polardbx-monitor.yaml
```

其中 polardbx-monitor.yaml 的yaml 描述如下:

```yaml
apiVersion: polardbx.aliyun.com/v1
kind: PolarDBXMonitor
metadata:
  name: quick-start-monitor
spec:
  clusterName: quick-start
  monitorInterval: 30s
  scrapeTimeout: 10s
```

- spec.clusterName: 需要开启监控的 PolarDB-X 集群名称
- spec.monitorInterval: 监控数据采集频率，默认30s
- spec.scrapeTimeout: 监控数据采集的超时时间，默认10s。注意：scrapeTimeout 的值需要小于 monitorInterval

## 为存量 PolarDB-X 标准版开启监控
本节介绍如何为 PolarDB-X 标准版集群开启监控。执行如下命令，为您的XStore对象配置 ServiceMonitor，以开启监控：

```bash
kubectl apply -f polardbx-s-monitor.yaml
```

其中 polardbx-s-monitor.yaml 内容如下:

```yaml
apiVersion: monitoring.coreos.com/v1
kind: ServiceMonitor
metadata:
  # ServiceMonitor对象的名称，自行定义即可
  name: polardbx-s-monitor
spec:
  selector:
    matchLabels:
      # 需要开启监控的PolarDB-X 标准版（XStore）名称
      xstore/name: polardbx-s
      # metrics service, 无需修改
      xstore/service: metrics
  # 以下内容采用默认配置，无需修改
  podTargetLabels:
    - xstore/name
    - xstore/role
  endpoints:
    - port: metrics
      path: /metrics
      interval: 30s
      relabelings:
         - sourceLabels: [xstore_name]
           targetLabel: polardbx_name
         - targetLabel: polardbx_role
           replacement: "dn"
           action: replace
```

注意：上述yaml文件中，修改spec.selector.matchLabels.xstore/name和metadata.name，将值修改为需要开启监控的 xstore对象名称即可，其它内容无需修改。


