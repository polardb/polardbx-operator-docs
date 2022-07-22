直接编辑 PolarDBXCluster的yaml，这部分参数修改因为需要重启才能生效，所以会自动触发cn pod 重建。通过如下命令修改：

```shell
kubectl edit pxc {pxc 名称}
```

修改：.spec.config.cn.static，添加需要修改的参数，如下所示：

```yaml
# 静态配置，修改会导致 CN 集群重建
static:
  # 启用协程, OpenJDK 暂不支持，需使用 dragonwell
  EnableCoroutine: false
  # 启用备库一致读
  EnableReplicaRead: false
  # 启用 JVM 的远程调试
  EnableJvmRemoteDebug: false
  # 自定义 CN 静态配置，key-value 结构
  # value 的值类型为 int 或 string，因此 bool 类型需要手动写为 string，例如 "true"、"false"
  ServerProperties:
    processors: 8
```
