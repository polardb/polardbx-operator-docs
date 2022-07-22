除了是修改资源配置以外，其余同[《3. 升级》](./3-update.md) 。

```bash
kubectl patch pxc polardbx-test -p '{"spec": {"topology": {"nodes": {"cn": {"template": {"resources": {"limits": {"cpu": 4, "memory": "16Gi"}}}}}}}}'
```

同样 `PHASE`从 `Running`进入 `Upgrading`，然后再回到 `Running`。
