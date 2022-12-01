查看计算节点日志
========
1. 参考 《[3. 登录计算节点容器](./3-cn-pod-login.md) 》进入CN 的容器
2. 进入 /home/admin/drds-server/logs 目录下查看需要的日志
3. 如果需要拷贝日志文件到本地，可以通过如下命令：

```shell
kubectl cp {pod 名}:{pod 内的日志文件} {本地目录}
```

