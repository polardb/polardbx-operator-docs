## 只读实例创建
对于PolarDB-X Operator 1.3.0及以上的版本，您可以创建只读实例，并指定其所属的 PolarDB-X 主实例。

只读实例的存储节点通过增加 Learner 副本的方式来保证物理资源隔离，提供了读写分离的特性，同时能够基于全局时钟来确保只读查询的强一致性。

您可以通过以下两种方法创建只读实例：

### 1. 为已有主实例添加只读实例
创建独立的 PolarDBXCluster yaml 配置文件，令`spec.readonly`为`true`，并指定`spec.primaryCluster`为其所属的主实例名，示例如下：
  ``` yaml
  # readonly.yaml
  apiVersion: polardbx.aliyun.com/v1
  kind: PolarDBXCluster
  metadata:
    name: pxc-readonly
  spec:
    readonly: true
    primaryCluster: pxc-master # 主实例名
    topology:
      nodes:
        cn:
          replicas: 1
          template:
            resources:
              limits:
                cpu: 2
                memory: 4Gi
            image: polardbx/polardbx-sql:latest
            imagePullPolicy: Always
        dn:
          # DN replicas 会自动与主实例的 DN replicas 保持同步，无需显式指定
          template:
            resources:
              limits:
                cpu: 2
                memory: 4Gi
            image: polardbx/polardbx-engine:latest
            imagePullPolicy: IfNotPresent
    config:
      cn:
        static:
          AttendHtap: true # 是否支持 HTAP
  ```
### 2. 同时创建主实例与只读实例
在创建主实例时，在主实例 PolarDBXCluster yaml 配置文件的`spec.initReadonly`字段中添加附属只读实例的信息。这种方法创建出的只读实例规格和参数与主实例相同，示例如下：
  ``` yaml
  # pxc-with-readonly.yaml
  apiVersion: polardbx.aliyun.com/v1
  kind: PolarDBXCluster
  metadata:
    name: pxc
  spec:
    initReadonly:
      - cnReplicas: 1 # 只读实例 CN 数
        name: readonly # 只读实例后缀名，本例中将生成名为 "pxc-readonly" 的只读实例，不填则会生成随机后缀
        extraParams:
          AttendHtap: "true" # 是否支持 HTAP
    topology:
      nodes:
        cn:
          replicas: 1
          template:
            resources:
              limits:
                cpu: 2
                memory: 4Gi
            image: polardbx/polardbx-sql:latest
            imagePullPolicy: Always
        dn:
          replicas: 1
          template:
            resources:
              limits:
                cpu: 2
                memory: 4Gi
            image: polardbx/polardbx-engine:latest
            imagePullPolicy: IfNotPresent
  ```

### 3. 连接只读实例

您可以直接连接只读实例，连接方法同主实例，见[连接 PolarDB-X 数据库](../connection/README.md)，对应的集群实例名称填入只读实例名即可。