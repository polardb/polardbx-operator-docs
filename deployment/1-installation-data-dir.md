修改数据目录
========
通过以下命令来在安装时指定宿主机：

- 数据目录 `/polardbx/data` （默认值为 /data）
- 日志目录 `/polardbx/log`   (默认值为/data-log)
- 传输目录 `/polardbx/filestream` （默认值为 /filestream）

```bash
helm install --namespace polardbx-operator-system --set node.volumes.data=/polardbx/data polardbx-operator polardbx/polardbx-operator
```

或者你也可以准备一个 values.yaml 文件，然后通过下面的命令来指定：

```bash
helm install --namespace polardbx-operator-system -f values.yaml polardbx-operator polardbx/polardbx-operator
```

其中 values.yaml 包含以下内容：

```yaml
node:
  volumes:
    data: /polardbx/data
    log: /polardbx/log
    filestream: /polardbx/filestream
```

> 除了上述目录，容器运行文件目录(通常默认为/var/lib/docker)和k8s根目录(通常默认为/var/lib/kubelet)，需要在安装docker或者k8s的时候挂载到合适的目录，防止出现磁盘满的问题。
