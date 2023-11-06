## 参数模板
PolarDB-X Operator从1.3.0版本开始支持参数模板功能

在实例初始化时，可以指定参数模板文件，对CN和DN指定一系列需要的模板参数。

参数模板需要通过yaml文件的形式进行配置。

注：参数模板需要配置在PolarDBXCluster所在的namespace中

```shell
kubectl apply -f {参数模板文件名称}.yaml --namespace={PolarDBXCluster所在namespace}
```

### 参数模板说明

注：在参数列表中，每个参数需要指定7个不同的属性，包括：
 - name(名称)
   - 参数名称
 - defaultValue(默认值)
   - 参数的默认值，格式为字符串
 - mode(修改模式)
   - 参数的模式，包含 read-only 和 read-write
 - restart(是否重启)
   - 参数修改后是否需要重启实例
 - unit(单位)
   - 参数的单位，包含 INT, DOUBLE, STRING, TZ(Time Zone), HOUR_RANGE
 - divisibilityFactor(整除因子)
   - 单位为INT的参数需要设置整除因子，其他单位默认为0
 - optional(取值范围)
   - 单位为INT, DOUBLE, HOUR_RANGE的参数，取值范围是一段范围，如："[1000-60000]"
   - 单位为STRING或TZ的参数，取值范围是一些可选项，如："[ON|OFF]"

参数模板的样例如下：

```yaml
## 参数模板示例
apiVersion: polardbx.aliyun.com/v1
kind: PolarDBXParameterTemplate
metadata:
  name: parameterTemplate
spec:
  nodeType:
    cn:
      # 参数列表
      paramList:
        - name: CONN_POOL_BLOCK_TIMEOUT
          defaultValue: "5000"
          mode: read-write
          restart: false
          unit: INT
          divisibilityFactor: 1
          optional: "[1000-60000]"
        - ...
    dn:
      name: dnTemplate
      paramList:
        - name: innodb_use_native_aio
          defaultValue: "OFF"
          mode: readonly
          restart: false
          unit: STRING
          divisibilityFactor: 0
          optional: "[ON|OFF]"
        - ...
    gms: ...
```

### 查看参数模板

实例会默认在polardbx-operator-system namespace中应用[8.0版本的参数模板](https://github.com/polardb/polardbx-operator/blob/main/charts/polardbx-operator/templates/parameter-template-product.yaml)，如果想在其他namespace创建实例，需要在相应的的namespace中创建参数模板对象。

可以通过如下命令查看已配置的所有参数模板。

```shell
kubectl get PolarDBXParameterTemplate --namespace=polardbx-operator-system
# 或者可用简称
kubectl get pxpt --namespace=polardbx-operator-system
```

### PolarDBXCluster配置

配置好参数模板后，就可以在启动PolarDBXCluster的yaml文件中指定需要的参数模板

```yaml
# 添加参数模板
apiVersion: polardbx.aliyun.com/v1
kind: PolarDBXCluster
metadata:
  name: pxc
spec:
  ...
  ...
  # 需要配置的参数模板
  parameterTemplate:
    name: product
```

注：
- 实例在应用参数模板后，会对CN或DN中默认的参数根据参数模板中的默认值进行调整。此外，configmap中my.cnf.overwrite字段的参数具有更高优先级，不会被参数模板修改。
- 目前不支持对配置了参数模板的实例修改参数模板，也不支持对运行的实例添加参数模板。若想修改运行中实例的参数，需使用动态参数功能。