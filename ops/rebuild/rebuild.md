备库重搭
===========================
PolarDB-X的DN由三个节点组成，角色为leader、follower、logger。当三个节点的其中一个由于软件或者硬件原因而无法继续提供服务的时候，我们需要对问题节点进行重建，即备库重搭。
本文主要介绍，针对存储节点的备库重搭功能。

# 备库重搭任务

备库重搭任务被定义为类型名为XStoreFollower一个Custom Resource Definition对象，用户可通过书写CRD yaml文件，发起一个备库重搭任务，等任务成功后再删除备库重搭任务。

## 重搭流程
一般的备库重搭流程为：从主节点上发起一次流式备份，将备份集直接发送到目标机器上，然后在目标机器上进行恢复流程，最后进行必要的参数配置，恢复出一个备节点来。
具体流程如下表所示：

| 步骤                                     | learner/follower是否需要 | logger是否需要 |
|----------------------------------------|----------------------|------------|
| 校验参数                                | 是                    | 是          |
| 确定源节点                               | 是                    | 否          |
| 创建一个临时POD目标节点                    | 跨机重搭时需要                    | 跨机重搭时需要    |
| 从源头节点上流式备份到目标节点上              | 是                    | 否          |
| 在目标节点上做备份恢复                      | 是                    | 否          |
| 刷三节点元数据，包括三节点ip和binlog启始位点     | 是                    | 是          |
| 删除临时pod，将正式的pod创建到临时pod所在的节点  | 是                    | 是          |
| 清理数据，包括：原pod的数据和备份集数据        | 是                    | 是          |


## 参数介绍

| 参数                  | 描述                                  | 是否必填  | 字段类型 | 默认值                            |
|---------------------|-------------------------------------|---------|------|--------------------------------|
| .metadata.name      | 备库重搭任务名称                            | 是      | 字符串  |                                |
| .spec.fromPodName   | 源POD名称，即对这个POD发起流式备份                | 否    | 字符串  | 从xstore实例上选择一个健康的节点            |
| .spec.targetPodName | 被重搭的POD名称                           | 是       | 字符串  ||
| .spec.local         | 是否本机重搭                              | 是       | 布尔值  | 默认为跨机重搭                        |
| .spec.XStoreName    | 源POD的XStore实例名称                     | 是       | 字符串  ||
| .spec.NodeName      | 可指定重搭到某个NODE上                       | 否      | 字符串  | 本机重搭为目标POD的NODE名称；跨机重搭，则依据调度策略 |


## 例子
名称xstore-xxx的xstore实例下的一个POD名称为xstore-xxx-pod-1的节点数据损坏服务继续提供服务，我们使用以下的yaml创建出一个跨机备库重搭任务，并等待重搭任务执行成功。
```yaml
apiVersion: polardbx.aliyun.com/v1
kind: XStoreFollower
metadata:
  name: rebuildjob
spec:
  local: false
  targetPodName: xstore-xxx-pod-1
  xStoreName: xstore-xxx
```