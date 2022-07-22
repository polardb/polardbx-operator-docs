示例：

```yaml
spec:
  topology:
    rules:
      components:
        # **Optional**
        #
        # CN 部署规则，同样按组划分 CN 节点
        cn:
        - name: zone-a
          # 合法值：数字、百分比、(0, 1] 分数，不填写为剩余 replica（只能有一个不填写）
          # 总和不能超过 .topology.nodes.cn.replicas
          replicas: 1
          selector:
            reference: zone-a
        - name: zone-b
          replicas: 1 / 3
          selector:
            reference: zone-b
        - name: zone-c
          replicas: 34%
          selector:
            reference: zone-c
```
