PolarDB-X目前包含企业版和标准版两个系列:

企业版：分布式架构集群，支持更大数据量。面向具备企业级超高并发、大规模数据复杂查询、加速分析的业务场景。 可大幅提升海量数据下复杂查询、报表分析的执行效率。
标准版：采用一主一备一日志的三节点架构，规格丰富，性价比高，通过多副本同步复制，确保数据的强一致性。面向具备超高并发、复杂查询及轻量分析的在线业务场景。 可有效提升对于在线业务的多表关联、聚合排序等复杂查询的执行效率。

本文介绍如何创建一个完整的 PolarDB-X 标准版集群。 首先准备一个描述PolarDB-X 标准版资源（XStore） 的 yaml 文件：
> 完整的 XStore 定义参考[这里](../../api/xstore.md) 。

```yaml
apiVersion: polardbx.aliyun.com/v1			# API 组 / 版本
kind: XStore							    # API 名称
metadata:									# 对象元数据
  name: polardbx-s						    # 对象名字
  namespace: default						# 所在命名空间
  labels:									# 对象标签集合
    kind: test									
spec:										# Spec
  config:                                   
    controller:
      RPCProtocolVersion: 2                 # RPC 协议版本，默认为2
  topology:
    nodeSets:
      - name: cand                          # Leader与Follower配置
        replicas: 2
        role: Candidate
        template:
          spec:
            image: polardbx/polardbx-engine-2.0:latest
            resources:
              limits:
                cpu: "2"
                memory: 4Gi
      - name: log                          # Logger 配置
        replicas: 1
        role: Voter
        template:
          spec:
            image: polardbx/polardbx-engine-2.0:latest
            resources:
              limits:
                cpu: "1"
                memory: 2Gi
```

使用下面的命令创建 XStore 对象：

```bash
kubectl create -f polardbx-standard.yaml
```

使用下面的命令观察 XStore 对象的状态：

```bash
kubectl get xstore polardbx-s
NAME         LEADER   READY   PHASE      DISK   VERSION   AGE
polardbx-s            0/3     Creating   0 B              28s
```

当状态中 `PHASE` 为 `Running` 时，PolarDB-X 集群就创建完成了。

```bash
kubectl get pxc polardbx-test
NAME         LEADER                   READY   PHASE     DISK      VERSION   AGE
polardbx-s   polardbx-s-tpdj-cand-1   3/3     Running   3.6 GiB   8.0.18    85s
```
字段说明：
* LEADER: PolarDB-X 标准版 Leader POD 名
* READY：XStore POD的ready 状态
* PHASE：XStore 集群的状态
* DISK： XStore 占用的磁盘空间
* VERSION：XStore 的内核版本
