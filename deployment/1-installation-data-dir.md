修改数据目录
========
通过以下命令来在安装时指定宿主机数据目录到 `/polardbx/data`：

```bash
helm install --namespace polardbx-operator-system --set node.volumes.data=/polardbx/data polardbx-operator polardbx/polardbx-operator --create-namespace
```

或者你也可以准备一个 values.yaml 文件，然后通过下面的命令来指定：

```bash
helm install --namespace polardbx-operator-system -f values.yaml polardbx/polardbx-operator --create-namespace
```

其中 values.yaml 包含以下内容：

```yaml
node:
  volumes:
    data: /polardbx/data
```
