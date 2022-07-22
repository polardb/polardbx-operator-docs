如果 PolarDB-X Cluster 存在 Knobs对象的话，直接edit改对象，添加/修改/删除对应的参数即可，执行如下命令：

```shell
kubectl edit pxcknobs {pxcknobs 名称}
```

如果 knobs 不存在，直接创建一个新的 knobs 对象，参考：[创建数据库参数操作对象](./1-cn-variable-load-at-runtime-create-db.md)
