### 通过 LoadBalancer 访问
若运行在有 LoadBalancer 的环境，比如阿里云平台，建议使用云平台的 LoadBalancer 特性。在创建 PolarDB-X 集群时指定 `.spec.serviceType` 为 LoadBalancer，operator 将会自动创建类型为 LoadBalancer 的服务（Service），此时当云平台支持时 Kubernetes 会自动为该服务配置，如下所示：

```bash
NAME                 TYPE           CLUSTER-IP       EXTERNAL-IP   PORT(S)                         AGE
xxxxxxxxx            LoadBalancer   192.168.247.39   8.209.29.16   3306:30612/TCP,8081:30370/TCP   28h
```

此时可使用 EXTERNAL-IP 所示的 IP 进行访问：

```bash
mysql -h8.209.29.16 -P3306 -upolardbx_root -p
```
### 通过机器的公网 IP 访问
如果您为K8s 集群内的部分机器开启了公网ip 地址，可以通过 port-foward 的方式将实例的访问端口映射到有公网ip的机器上。

在有公网ip的机器上执行如下命令进行端口转发：

```shell
kubectl port-forward svc/{PolarDB-X 集群名} 3306 --address=0.0.0.0
```

注意：--address=0.0.0.0 需要加上，允许外部ip 访问

配置机器对应的安全组或者防火墙，允许3306 端口被外部机器访问。之后通过机器的公网IP + 3306 端口访问即可。
