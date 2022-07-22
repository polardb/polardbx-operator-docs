在 `topology.rules.nodeSelectors`中，你可以定义一组预置的节点选择器，然后在后面的 `topology.rules.components`中引用它们。关于节点选择器的定义方法和含义，参考[官方文档](https://kubernetes.io/docs/concepts/scheduling-eviction/assign-pod-node/) 。

```yaml
spec:
  topology:
    rules:
      selectors:
      - name: zone-a
        nodeSelector:
          nodeSelectorTerms:
          - matchExpressions:
            - key: topology.kubernetes.io/zone
              operator: In
              values:
              - cn-hangzhou-a
      - name: zone-b
        nodeSelector:
          nodeSelectorTerms:
          - matchExpressions:
            - key: topology.kubernetes.io/zone
              operator: In
              values:
              - cn-hangzhou-b
      - name: zone-c
        nodeSelector:
          nodeSelectorTerms:
          - matchExpressions:
            - key: topology.kubernetes.io/zone
              operator: In
              values:
              - cn-hangzhou-c
```

节点选择器可以帮助我们控制部署实例的拓扑，例如两地三中心，三地五中心等，具体的使用可以参考：[容灾部署示例](./1-create-ha-example.md) 。
