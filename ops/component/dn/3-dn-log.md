## 容器可以登录
如果 DN 的 pod 能够正常（或者关闭探活后）登录，则登录后进入 /data/mysql/log/目录，重点查看 alert.log 即可。

## 容器无法登录
通过如下命令查看 dn  engine 的启动日志：

```shell
kubectl logs {pod 名} engine
```

如果发现日志中有报错，显示 mysql 初始化失败，则需要通过如下的方式前往 dn pod 在主机上的目录查看 alert.log

 1. 加上 -o wide 参数，找到dn pod 所在的机器：

```shell
kubectl get pod {pod 名} -o wide
```

得到如下输出：

```shell
NAME                         READY   STATUS    RESTARTS   AGE   IP             NODE                          NOMINATED NODE   READINESS GATES
tunan-oss-drsg-dn-0-cand-1   3/3     Running   0          20d   172.16.0.129   cn-zhangjiakou.172.16.0.129   <none>           <none>
```

其中 NODE 即 该pod 调度的机器。

2. 执行如下命令获取 dn pod 在宿主机上的实际目录：

```shell
kubectl get pod {pod 名} -o json | grep "/data/xstore/default"
```

期望得到如下输出：

```shell
"path": "/data/xstore/default/tunan-oss-drsg-dn-0-cand-1"
```

3. 前往该机器的上述目录即可查看日志。

