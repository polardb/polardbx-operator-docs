## 获取用户名密码
用户名默认是：admin
密码通过如下命令获取：

```shell
kubectl get secret {dn 名} -o jsonpath={.data.admin} | base64 -d - | xargs echo "Password"
```

## 获取连接串
执行如下命令获取 clusterIp：

```shell
kubectl get svc {dn 名}
```

期望得到如下输出：

```shell
NAME                  TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)              AGE
tunan-oss-drsg-dn-0   ClusterIP   192.168.192.73   <none>        3306/TCP,31306/TCP   21d
```

如果在 K8s 集群内部直接通过 cluster-ip + 3306 端口即可访问。

如果在 K8s 外部，执行如下命令将端口转发到本地访问：

```shell
kubectl port-forward svc/{dn 名} 3306:3306
```
