
1. 执行如下命令，查看实际拉取的镜像是否存在
```shell
kubectl describe pod {报错的 pod}
```
2. 确认是否有镜像仓库的拉取权限。PolarDB-X 默认的镜像仓库是无需权限的，如果使用内部镜像仓库，需要配置鉴权信息，参考文档：[Pull an Image from a Private Registry](https://kubernetes.io/docs/tasks/configure-pod-container/pull-image-private-registry/)
3. 第2步确认完成后，删除报错的 pod，让其重建即可。



