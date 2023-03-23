# 为存量集群开启监控
PolarDB-X 集群的监控采集功能默认是关闭的，需要为您需要监控的 PolarDBXCluster 创建 PolarDBXMonitor对象进行开启。

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


