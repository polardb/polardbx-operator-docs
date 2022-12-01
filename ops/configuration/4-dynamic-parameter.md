## 动态参数
PolarDB-X Operator从1.3.0版本开始支持动态参数功能

在实例运行时，可以通过指定动态参数文件来修改CN和DN的参数.

动态参数需要通过yaml文件的形式进行配置。

```shell
kubectl apply -f {动态参数文件名称}.yaml
```

### 动态参数说明

动态参数在应用时需要指定基础的参数模板和实例的名称，当名称不存在时，会验证失败。
此外，动态参数需要通过参数模板中属性的校验，否则也会验证失败。

注：由于部分参数在修改后需要重启实例，所以需要指定重启方式，包括直接重启(restart)和滚动重启(rollingRestart)两种，目前DN只支持滚动重启。

在参数列表中，每个参数需要指定2个属性，包括：
- name(名称)
    - 参数名称
- value(取值)
    - 参数的取值，格式为字符串

动态参数的样例如下：

```yaml
# 添加动态参数
apiVersion: polardbx.aliyun.com/v1
kind: PolarDBXParameter
metadata:
  name: test-param
  labels:
    parameter: dynamic
spec:
  # 实例名称
  clusterName: pxc
  # 参数模板名称
  templateName: product
  nodeType:
    cn:
      name: cn-parameter
      # 重启方式
      restartType: rollingRestart
      # 参数列表
      paramList:
        - name: CONN_POOL_MAX_POOL_SIZE
          value: "1000"
    dn:
      name: dn-parameter
      restartType: rollingRestart
      paramList:
        - name: autocommit
          value: "OFF"
        - ...
```

### 查看动态参数

可以通过如下命令查看已配置的所有动态参数。

```shell
kubectl get PolarDBXParameter
# 或者可用简称
kubectl get pxp
```