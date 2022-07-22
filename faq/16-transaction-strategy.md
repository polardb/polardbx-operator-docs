事务策略是 CN 的动态参数，如何修改可以参考文档：[《创建数据库参数操作对象》](../ops/configuration/1-cn-variable-load-at-runtime-create-db.md)

## 操作步骤

1. 配置knobs.yaml 如下所示：

```yaml
apiVersion: polardbx.aliyun.com/v1
kind: PolarDBXClusterKnobs
metadata:
  name: kunan-oss
spec:
  clusterName: "tunan-oss"
  knobs:
    TRANSACTION_POLICY: XA
```

增加TRANSACTION_POLICY 参数，填写需要的事务策略即可，支持 XA|TSO|TSO_READONLY。

2. 登录CN，执行如下SQL，检查配置是否生效：

```mysql
begin; show variables like "drds_transaction_policy"; rollback;
```
