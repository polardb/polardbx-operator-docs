同[《5. 扩容》](./5-scale-out.md) 一样，除了是缩减节点。同样，数据会自动进行搬迁。

```bash
kubectl patch pxc polardbx-test -p '{"spec": {"topology": {"nodes": {"dn": {"replicas": 1}}}}}'
```
