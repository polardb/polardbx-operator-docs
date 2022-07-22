数据库参数修改详见：[《创建数据库参数操作对象》](../ops/configuration/1-cn-variable-load-at-runtime-create-db.md)
## 关闭私有协议
通过 pxcknobs 修改如下参数即可：

```shell
CONN_POOL_XPROTO_STORAGE_DB_PORT：-1 // DN 的私有协议，-1为关闭，0为自动获取配置
CONN_POOL_XPROTO_META_DB_PORT: -1    // Meta db 的私有协议开关，-1为关闭，0为自动获取配置
```

```yaml
apiVersion: polardbx.aliyun.com/v1
kind: PolarDBXClusterKnobs
metadata:
  name: polardbx-xcluster
  namespace: development
spec:
  ## PolarDB-X 的实例名
  clusterName: "polardbx-xcluster"
  knobs:    
    CONN_POOL_XPROTO_STORAGE_DB_PORT: -1
    CONN_POOL_XPROTO_META_DB_PORT: -1 
```

## 开启私有协议
配置如下 pxcknobs 参数即可：

```shell
apiVersion: polardbx.aliyun.com/v1
kind: PolarDBXClusterKnobs
metadata:
  name: polardbx-xcluster
  namespace: development
spec:
  ## PolarDB-X 的实例名
  clusterName: "polardbx-xcluster"
  knobs:    
    CONN_POOL_XPROTO_STORAGE_DB_PORT: 0
    CONN_POOL_XPROTO_META_DB_PORT: 0 
```
