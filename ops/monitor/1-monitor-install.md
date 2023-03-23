本文介绍如何在 K8s 集群中为 PolarDB-X 数据库开启监控功能。
## 安装 PolarDB-X Monitor
PolarDB-X 通过 Prometheus 和 Grafana 来监控 PolarDB-X 集群。PolarDB-X Monitor 集成了  [kube-promethus](https://github.com/prometheus-operator/kube-prometheus) 组件栈，通过安装 PolarDB-X Monitor 即可一键部署监控所需的资源和组件。
### 前置要求

1. 已经准备了一个运行中的 K8s 集群，并确保集群版本 >= 1.18.0 
2. 已经安装了 [Helm 3](https://helm.sh/docs/intro/install/)
3. 已经安装 PolarDB-X Operator 1.2.0 及以上的版本

### Helm 包安装
首先创建一个名为 polardbx-monitor 的命名空间:

```bash
kubectl create namespace polardbx-monitor
```

安装 PolarDBXMonitor CRD:
> 注意：如果您的 PolarDB-X Operator 1.2.0及以上 是通过 helm install 直接安装的，PolarDBXMonitor 的 CRD 会默认安装，可以跳过这步。如果您的 PolarDB-X Operator 是 从1.1.0 及以下的低版本通过 helm upgrade 升级而来，需要执行如下命令手工安装：

```bash
kubectl apply -f https://raw.githubusercontent.com/polardb/polardbx-operator/v1.4.0/charts/polardbx-operator/crds/polardbx.aliyun.com_polardbxmonitors.yaml
```

执行如下命令安装 PolarDB-X Monitor：

```bash
 helm install --namespace polardbx-monitor polardbx-monitor polardbx-monitor-1.4.0.tgz
```

您也可以通过 PolarDB-X 的 Helm Chart 仓库安装:

```bash
helm repo add polardbx https://polardbx-charts.oss-cn-beijing.aliyuncs.com
helm install --namespace polardbx-monitor polardbx-monitor polardbx/polardbx-monitor
```

> 注：通过这种方式安装 Prometheus 和 Grafana 采用的都是默认配置便于快速体验。如果部署在生产集群，你可以参考: [配置 Prometheus + Grafana](./4-prom-config.md)
> 
> 注：如果您是在 minikube 上安装 PolarDB-X Monitor, 可能会因为资源不够导致组件无法创建，可以参考: [配置 Prometheus + Grafana](./4-prom-config.md)


期望看到如下输出：

```shell
polardbx-operator monitor plugin is installed. Please check the status of components:

    kubectl get pods --namespace {{ .Release.Namespace }}

Now start to monitor your polardbx cluster.
```

PolarDB-X Monitor 安装完成后，会在您 K8s 集群的 polardbx-monitor 命名空间下创建 prometheus 和 grafana 等组件，以此来监控 K8s 内的 PolarDB-X，通过如下命令检查相关组件是否正常，确认所有的 pod 都处于 Running 状态。

```bash
kubectl get pods -n polardbx-monitor
```


