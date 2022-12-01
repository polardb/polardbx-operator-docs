1. K8S 官方尚未提供从 stopped/completed Pod中拷贝文件的功能，[参考这里](https://github.com/kubernetes/kubectl/issues/454) 。不过可以通过如下命令获取上一个 Pod 的日志信息：

```shell
kubectl logs <podname> -n <namespace> --previous
```
2. 如果通过查看日志判断 pod 无法达到Running状态是因为探活（Probe容器）失败，可以通过如下命令关闭 Pod 的探活，让 Pod 达到 Running 状态后，进入 Pod 查看文件或者拷贝文件。

```shell
kubectl annotate pod {pod 名} runmode=debug
```
