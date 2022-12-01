修改默认镜像仓库
========
修改默认镜像仓库为 `registry:5000`：

```bash
helm install --namespace polardbx-operator-system --set imageRepo=registry:5000 polardbx-operator polardbx/polardbx-operator
```
