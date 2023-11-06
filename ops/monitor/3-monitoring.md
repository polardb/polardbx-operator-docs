## 访问 Grafana Dashboard
默认情况下执行如下命令将 Grafana 端口转发到本地：

```bash
kubectl port-forward svc/grafana -n polardbx-monitor 3000
```

在浏览器中输入: [http://localhost:3000](http://localhost:3000), 即可访问到 PolarDB-X 的Dashboard，默认的用户名和密码都是 admin。PolarDB-X 企业版的监控信息在 PolarDB-X Overview 中查看，PolarDB-X 标准版的监控信息在 XStore Overview 中查看。
> 注：由于 Grafana 的配置存储在 ConfigMap 中，您在 Grafana 中修改的密码或者新增的 Dashboard 不会被持久化，一旦 Grafana Pod 重建，这部分配置会丢失，请注意提前保存。


![](./polardb-x-dashboard.png)
如果您的 K8s 集群中支持 LoadBalancer，你可以为 Grafana 的 Service 配置 LoadBalancer 进行访问，参考：

如果您的 K8s 集群内有多个 PolarDB-X Cluster，可以通过 Grafana 页面上面的下拉框切换 Namespace 和 PolarDB-X Cluster。

## 访问 Prometheus
默认情况下执行如下命令将 Prometheus 端口转发到本地：

```bash
kubectl port-forward svc/prometheus-k8s -n polardbx-monitor 9090
```

在浏览器中输入: [http://localhost:9090](http://localhost:9090), 即可访问到 Prometheus页面。

如果您的 K8s 集群中支持 LoadBalancer，你可以为 Prometheus 的 Service 配置 LoadBalancer 进行访问，详见：[配置 Prometheus 和 Grafana](./4-prom-config.md)

