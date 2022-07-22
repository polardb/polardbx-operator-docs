有状态节点规则是针对元数据、存储节点的内部节点，有两种形式：

- nodeSet，每个 GMS、DN 都遵从 nodeSet 的规则来部署内部节点
- rolling，只针对 DN，会将内部节点按照堆叠的方式部署在 Kubernetes 集群内的所有可用的节点之上（用于测试），从而最大化资源利用

```yaml
spec:
  topology:
    rules:
      components:
        #  **Optional**
        #
        # GMS 部署规则，默认和 DN 一致
        gms:
          # 堆叠部署结构，operator 尝试在节点选择器指定的节点中，堆叠部署
          # 每个存储节点的子节点以达到较高资源利用率的方式，仅供测试使用
          rolling:
            replicas: 3
            selector:
              reference: zone-a
          # 节点组部署结构，可以指定每个 DN 的子节点的节点组和节点选择器，
          # 从而达成跨区、跨城等高可用部署结构
          nodeSets:
          - name: cand-zone-a
            role: Candidate
            replicas: 1
            selector:
              reference: zone-a
          - name: cand-zone-b
            role: Candidate
            replicas: 1
            selector:
              reference: zone-b
          - name: log-zone-c
            role: Voter
            replicas: 1
            selector:
              reference: zone-c
```
