## 非 CN Pod 访问
如果你在 K8s 集群内的pod上访问 PolarDB-X，可以直接通过 cluster-ip 访问。 创建 PolarDB-X 集群时，PolarDB-X Operator 同时会为集群创建用于访问的服务，默认是 ClusterIP 类型。使用下面的命令查看用于访问的服务：

```shell
$ kubectl get svc {PolarDB-X 集群名}
```

期望输出：

```shell
NAME          TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)             AGE
quick-start   ClusterIP   10.110.214.223   <none>        3306/TCP,8081/TCP   5m25s
```

如果您是在 K8s 集群内进行访问，可以直接使用上面输出的 Cluster-IP 即可。PolarDB-X 服务默认的端口都是 3306.
> ClusterIP 是通过 K8s 集群的内部 IP 暴露服务，选择该访问方式时，只能在集群内部访问

执行如下命令，输入上面获取的密码后，即可连接 PolarDB-X：

```shell
mysql -h10.110.214.223 -P3306 -upolardbx_root -p
```

> **说明: **
>    - 此处**-P**为大写字母，默认端口为3306。
>    - 为保障密码安全，**-p**后请不要填写密码，会在执行整行命令后提示您输入密码，输入后按回车即可登录。

## CN Pod 内访问
直接在cn pod 内输入 myc 命令即可登录
