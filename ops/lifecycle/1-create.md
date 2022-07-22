前言：完整的 PolarDBXCluster 定义参考[这里](../../api/polardbxcluster.md) 。

首先准备一个描述 PolarDBXCluster 的 yaml 文件：

```yaml
apiVersion: polardbx.aliyun.com/v1			# API 组 / 版本
kind: PolarDBXCluster										# API 名称
metadata:																# 对象元数据
  name: polardbx-test										# 对象名字
  namespace: default										# 所在命名空间
  labels:																# 对象标签集合
    kind: test									
spec:																		# Spec
  topology:															# 拓扑定义
    nodes:															# 节点规格和数量
      cn:
        replicas: 2	
        template:
          image: polardbx/galaxysql:latest
          resources:
            limits:
              cpu: 4
              memory: 16Gi
      dn:
        replicas: 2
        template:
          image: polardbx/galaxyengine:latest
          resources:
            limits:
              cpu: 4
              memory: 16Gi
      cdc:
        replicas: 2
        template:
          image: polardbx/galaxycdc:latest
          resources:
            limits:
              cpu: 4
              memory: 16Gi
```

使用下面的命令创建 PolarDBXCluster 对象：

```bash
kubectl create -f polardbx-test.yaml
```

使用下面的命令观察 PolarDBXCluster 对象的状态：

```bash
kubectl get pxc polardbx-test
NAME            GMS   CN    DN    CDC   PHASE      DISK   AGE
polardbx-test   0/1   0/2   0/2   0/2   Creating          5s
```

当状态中 `PHASE` 为 `Running` 时，PolarDB-X 集群就创建完成了。

```bash
kubectl get pxc polardbx-test
NAME            GMS   CN    DN    CDC   PHASE      DISK   AGE
polardbx-test   1/1   2/2   2/2   2/2   Running    6.2Gi  63s
```
