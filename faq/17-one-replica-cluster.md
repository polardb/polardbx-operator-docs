在yaml 中添加 .spec.topology.rules.components 配置gms 和 dn 的 nodesets即可，如下所示：

```yaml
apiVersion: polardbx.aliyun.com/v1
kind: PolarDBXCluster
metadata:
  name: pxc-demo
spec:
  topology:
    rules:
      components:
        gms:
          ## 配置nodeSets即可
          nodeSets:
          - name: cands
            role: Candidate
            replicas: 1
        dn:
          ## 配置nodeSets即可
          nodeSets:
          - name: cands
            role: Candidate
            replicas: 1
    nodes:
      gms:
        template:
          resources:
            limits:
              cpu: 2
              memory: 4Gi
      cn:
        replicas: 1
        template:
          resources:
            requests:
              cpu: 1
              memory: 4Gi
            limits:
              cpu: 2
              memory: 4Gi
      dn:
        replicas: 1
        template:
          resources:
            limits:
              cpu: 2
              memory: 4Gi
      cdc:
        replicas: 1
        template:
          resources:
            limits:
              cpu: 2
              memory: 4Gi
```
