CN 的动态参数支持直接在 PolarDBXCluster 对象的 yaml 中修改，详见 .spec.config.cn.dynamic。不过这种配置方式也存在一些问题：

- 集群配置项过多，集群定义过长掩盖其他细节
- PolarDBXCluster 不仅需要负责集群（容器）的维护，还需要负责配置项的维护，逻辑复杂且容易出错
- 单向同步导致其他途径（比如 set global）设置的参数失效

因此开源版本中，支持了通过 Knobs 对象修改 CN 的动态参数。

Knobs 对象的 yaml 定义如下：

```yaml
apiVersion: polardbx.aliyun.com/v1
kind: PolarDBXClusterKnobs
metadata:
  name: polardbx-xcluster
  namespace: development
spec:
  ## PolarDB-X 的实例名
  clusterName: "polardbx-xcluster"
  # 创建时不需要指定
  knobs:
    ## 参数列表
    CONN_POOL_MAX_POOL_SIZE: 100
    RECORD_SQL: "true"
    
```

>注：CN 的动态参数列表详见：[https://help.aliyun.com/document_detail/316576.html](https://help.aliyun.com/document_detail/316576.html)
>
>注：布尔参数值需要用字符串传入。

编辑好上述的 yaml 文件后 执行即可

```shell
kubectl apply -f {knbos yaml 文件}
```

执行如下 命令查看 knobs的列表：

```shell
kubectl get pxcknobs
```

