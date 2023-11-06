# polardbx.aliyun.com/v1 XStore

使用 XStore 可以自由定义 PolarDB-X 标准版的拓扑、配置信息。

以下是可配置项及相关的字段的含义：

```yaml
apiVersion: polardbx.aliyun.com/v1
kind: XStore
metadata:
  name: full
  namespace: default
spec:
  #
  # XStore 相关配置项
  #
  config:
    # Operator 相关配置项
    controller:
      # XStore RPC 协议版本
      # Required， 取值范围[1, 2]
      # 默认值：1
      RPCProtocolVersion: 1
      # 清理 Binlog的间隔
      # 默认值：0s，表示不清理binlog
      logPurgeInterval: 0s
    # **Optional**
    # XStore 引擎相关配置项
    #
    engine:
      # **Optional**
      # mycnf 中的相关配置项
      #
      override:
        value: |+
          loose_new_rpc = off

  # **Optional**
  # Xstore 引擎类型
  # 默认值：galaxy
  engine: galaxy

  # **Optional**
  # XStore 的参数模板名
  # 对应PolarDBXParameterTemplate对象的名称
  parameterTemplate: {}

  # **Optional**
  # XStore 只读实例的名称，默认为空
  # 如果当前XStore 是只读实例，需要填
  primaryXStore: classic-8c9z-dn-0

  # **Optional**
  # XStore 是不是只读实例
  # false：主实例， true：只读实例
  # 默认值：false
  readonly: false


  # **Optional**
  #
  # PolarDB-X 集群在 Kubernetes 内对外暴露的服务类型，默认为 ClusterIP
  # 可选值参考 Service 的类型
  #
  # 注：云上的 Kubernetes 集群可使用 LoadBalancer 来绑定 LB
  serviceType: ClusterIP

  # PolarDB-X 标准版集群拓扑
  topology:

    # PolarDB-X 标准版节点信息配置，支持 Candidate，Voter，Learner
    # Leader节点在 Candidate 角色的节点中选出
    # Voter 表示日志节点
    # Learner 表示只读节点
    nodeSets:

      # **Required**
      # Candidate 节点的配置
      - name: cand

        # **Required**
        # Candidate 节点的数量，
        # 默认值为2，表示1个Leader和1个Follower
        # 如果填5，表示1个Leader和4个Follower 
        replicas: 2

        # **Required**
        # 节点集合的Role，支持 Candidate，Voter，Learner
        # 详细的节点配置如上所示
        role: Candidate

        # **Required**
        # Candidate 节点集的配置
        template:

          # **Optional**
          # Candidate 节点集的元信息
          metadata:
            annotations:
              cpuset-scheduler: "true"
            labels:
              polardbx/dn-index: "0"
              polardbx/name: classic
              polardbx/rand: 8c9z
              polardbx/role: dn


          # Candidate 节点集的Spec
          spec:

            # affinity信息
            affinity:
              nodeAffinity: {}
              podAntiAffinity:
                preferredDuringSchedulingIgnoredDuringExecution:
                  - podAffinityTerm:
                      labelSelector:
                        matchLabels:
                          polardbx/name: classic
                      namespaces:
                        - default
                      topologyKey: kubernetes.io/hostname
                    weight: 100

            # XStore Candidate Pod 是否适用宿主机网络，默认为 false
            hostNetwork: false

            # XStore Candidate Pod 的镜像名
            image: polardbx/polardbx-engine-2.0:latest

            # XStore Candidate Pod 的资源信息
            resources:
              limits:
                cpu: "2"
                memory: 4Gi

      # **Required**
      # Voter（Logger) 节点的配置
      - name: log

        # **Required**
        # Voter 节点的数量
        # 在三副本模式下，值为1，表示1 Leader，1 Follower，1 Logger
        # 在两地三中心五副本模式下，值为0，表示无 Logger 节点
        replicas: 1

        # **Required**
        # 节点集合的Role，支持 Candidate，Voter，Learner
        # 详细的节点配置如上所示
        role: Voter

        template:

          # **Optional**
          # Candidate 节点集的元信息
          metadata:
            annotations:
              cpuset-scheduler: "true"
            labels:
              polardbx/dn-index: "0"
              polardbx/name: classic
              polardbx/rand: 8c9z
              polardbx/role: dn

          # Voter 节点集的Spec
          spec:
            affinity:
              nodeAffinity: {}
              podAntiAffinity:
                preferredDuringSchedulingIgnoredDuringExecution:
                  - podAffinityTerm:
                      labelSelector:
                        matchLabels:
                          polardbx/name: classic
                      namespaces:
                        - default
                      topologyKey: kubernetes.io/hostname
                    weight: 100


            # XStore Voter Pod 是否适用宿主机网络，默认为 false
            hostNetwork: false

            # XStore Voter Pod 的镜像名
            image: polardbx/polardbx-engine-2.0:latest

            # XStore Voter Pod 的资源信息
            # 因 Voter仅存储日志，不存储数据，消耗资源量相对于Candidate更小，可以采用更低的规格
            resources:
              limits:
                cpu: "2"
                memory: 4Gi

    template:
      metadata: {}
      spec:
        hostNetwork: false

  # **Optional**
  # XStore pod的升级策略，同 Kubenetes 的 upgradeStrategy
  # 默认值：BestEffort
  upgradeStrategy: BestEffort
```