## 准备工作
开始之前，请确保满足以下前置要求：

+ 已经准备了一个运行中的 Kubernetes 集群，并确保
    + 集群版本 >= 1.18.0
    + 至少有 2 个可分配的 CPU
    + 至少有 4GB 的可分配内存
    + 至少有 30GB 以上的磁盘空间
+ 已经安装了 kubectl 可以访问 Kubernetes 集群
+ 已经安装了 [Helm 3](https://helm.sh/docs/intro/install/)


首先创建一个叫 `polardbx-operator-system` 的命名空间，

```bash
$ kubectl create namespace polardbx-operator-system
```

执行以下命令安装 PolarDB-X Operator。

```bash
$ helm install --namespace polardbx-operator-system polardbx-operator https://github.com/ApsaraDB/galaxykube/releases/download/v1.2.2/polardbx-operator-1.2.2.tgz
```

您也可以通过 PolarDB-X 的 Helm Chart 仓库安装:

```bash
helm repo add polardbx https://polardbx-charts.oss-cn-beijing.aliyuncs.com
helm install --namespace polardbx-operator-system polardbx-operator polardbx/polardbx-operator
```

期望看到如下输出：

```bash
NAME: polardbx-operator
LAST DEPLOYED: Sun Oct 17 15:17:29 2021
NAMESPACE: polardbx-operator-system
STATUS: deployed
REVISION: 1
TEST SUITE: None
NOTES:
polardbx-operator is installed. Please check the status of components:

    kubectl get pods --namespace polardbx-operator-system

Now have fun with your first PolarDB-X cluster.

Here's the manifest for quick start:

```yaml
apiVersion: polardbx.aliyun.com/v1
kind: PolarDBXCluster
metadata:
  name: quick-start
  annotations:
    polardbx/topology-mode-guide: quick-start
```

## 安装选项
Helm 安装通常可以指定一些配置选项的值，用于覆盖默认的安装选项。这里介绍几个常见的选项和安装模式：

- node.volume.data，用来指定宿主机上使用的数据目录，参考 [安装-修改数据目录](./1-installation-data-dir.md) ；
- images、imageTag、useLatestImage 和 clusterDefaults，用来修改系统组件和数据库集群默认的镜像集合，参考[安装-修改默认镜像](./1-installation-default-image.md) ；
- imageRepo，用来修改默认的镜像仓库，参考[安装-修改默认镜像仓库](./1-installation-default-image-repo.md) ;

所有安装选项可通过如下命令获取：

```shell
helm show values --namespace polardbx-operator-system  https://github.com/ApsaraDB/galaxykube/releases/download/v1.2.2/polardbx-operator-1.2.2.tgz
```

## 系统检查
### 运行情况

用以下命令查看系统组件的运行情况：

```bash
kubectl -n polardbx-operator-system get pods
```

通常你将看到以下 Pod：

```bash
NAME                                          READY   STATUS    RESTARTS   AGE
NAME                                           READY   STATUS    RESTARTS   AGE
polardbx-controller-manager-6c858fc5b9-zrhx9   1/1     Running   0          66s
polardbx-hpfs-d44zd                            1/1     Running   0          66s
polardbx-tools-updater-459lc                   1/1     Running   0          66s
```

其中：

- polardbx-controller-manager 是 operator 和 webhook 所在的 Pod，由一个 Deployment 控制创建
- polardbx-hpfs 是宿主机远程文件服务所在的 Pod，由一个 DaemonSet 控制创建，因此每个节点会有一个
- polardbx-tools-updater 是宿主机上一些公共工具脚本的更新程序，同样由一个 DaemonSet 控制创建

### 动态配置
Operator/Webhook 在运行时加载了一组动态配置，这组配置在 Kubernetes 中存放在 ConfigMap 中。

用下面的命令查看配置的内容：

```bash
kubectl -n polardbx-operator-system get configmap polardbx-controller-manager-config -o yaml
```

通常你将看到这样的内容：

```yaml
apiVersion: v1
data:
  config.yaml: |-
    images:
      repo: polardbx
      common:
        prober: probe-proxy:v1.2.0
        exporter: polardbx-exporter:v1.2.0
      compute:
        init: polardbx-init:v1.2.0
        engine: registry.cn-zhangjiakou.aliyuncs.com/drds_pre/galaxysql:20220330-2
      cdc:
        engine: registry.cn-zhangjiakou.aliyuncs.com/drds_pre/galaxycdc:20220408
      store:
        galaxy:
          engine: galaxyengine@sha256:a1cf4aabf3e0230d6a63dd9afa125e58baa2a925462a59968ac3b918422bf521
          exporter: prom/mysqld-exporter:master
    scheduler:
      enable_master: true
    cluster:
      enable_exporters: true
      enable_aliyun_ack_resource_controller: true
      enable_debug_mode_for_compute_nodes: false
      enable_privileged_container: false
    store:
      enable_privileged_container: false
      host_paths:
        tools: /data/cache/tools/xstore
        volume_data: /data/xstore
      hpfs_endpoint: polardbx-hpfs:6543
  webhook.yaml: |-
    validator:

    default:
      protocol_version: 8
      storage_engine: galaxy
      service_type: ClusterIP
      upgrade_strategy: RollingUpgrade
kind: ConfigMap
metadata:
  annotations:
    meta.helm.sh/release-name: polardbx-operator
    meta.helm.sh/release-namespace: polardbx-operator-system
  creationTimestamp: "2022-04-01T08:09:55Z"
  labels:
    app.kubernetes.io/managed-by: Helm
  name: polardbx-controller-manager-config
  namespace: polardbx-operator-system
  resourceVersion: "2475601453"
  uid: 585844ea-4b87-4407-98f2-520d02d8cffd
```
