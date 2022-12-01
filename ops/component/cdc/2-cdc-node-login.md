登录日志节点
======
执行如下命令登录日志节点容器：

```shell
kubectl exec -it {pod 名} bash
```

cdc 的日志在 /home/admin/logs/ 下面

CDC 的容器内会有三个 java 进程，daemon，dumper，final，日志分别在对应的目录下。
