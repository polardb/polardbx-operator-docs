## 同城三机房

```yaml
spec:
  topology:
    rules:
      selectors:
      - name: zone-a
        ...
      - name: zone-b
        ...
      - name: zone-c
        ...
    components:
      cn:
      - name: zone-a
        replicas: 1 / 3
        selector:
          reference: zone-a
      - name: zone-b
        replicas: 1 / 3
        selector:
          reference: zone-b
      - name: zone-c
        replicas: 1 / 3
        selector:
          reference: zone-c
      cdc:
      - name: zone-a
        replicas: 1 / 3
        selector:
          reference: zone-a
      - name: zone-b
        replicas: 1 / 3
        selector:
          reference: zone-b
      - name: zone-c
        replicas: 1 / 3
        selector:
          reference: zone-c
      dn:
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
## 两地三中心

```yaml
spec:
  topology:
    rules:
      selectors:
      - name: region-1-zone-a
        ...
      - name: region-1-zone-b
        ...
      - name: region-2-zone-c
        ...
    components:
      cn:
      - name: region-1-zone-a
        replicas: 1 / 3
        selector:
          reference: region-1-zone-a
      - name: region-1-zone-b
        replicas: 1 / 3
        selector:
          reference: region-1-zone-b
      - name: region-2-zone-c
        replicas: 1 / 3
        selector:
          reference: region-2-zone-c
      cdc:
      - name: region-1-zone-a
        replicas: 1 / 3
        selector:
          reference: region-1-zone-a
      - name: region-1-zone-b
        replicas: 1 / 3
        selector:
          reference: region-1-zone-b
      - name: region-2-zone-c
        replicas: 1 / 3
        selector:
          reference: region-2-zone-c
      dn:
        nodeSets:
        - name: cand-region-1-zone-a
          role: Candidate
          replicas: 1
          selector:
            reference: region-1-zone-a
        - name: cand-region-2-zone-c
          role: Candidate
          replicas: 1
          selector:
            reference: region-2-zone-c
        - name: region-1-zone-b
          role: Voter
          replicas: 1
          selector:
            reference: region-1-zone-b
```

## 三地五中心

```yaml
spec:
  topology:
    rules:
      selectors:
      - name: region-1-zone-a
        ...
      - name: region-1-zone-b
        ...
      - name: region-2-zone-c
        ...
      - name: region-2-zone-d
        ...
      - name: region-3-zone-e
        ...
    components:
      cn:
      - name: region-1-zone-a
        replicas: 1 / 5
        selector:
          reference: region-1-zone-a
      - name: region-1-zone-b
        replicas: 1 / 5
        selector:
          reference: region-1-zone-b
      - name: region-2-zone-c
        replicas: 1 / 5
        selector:
          reference: region-2-zone-c
      - name: region-2-zone-d
        replicas: 1 / 5
        selector:
          reference: region-2-zone-d
      - name: region-3-zone-e
        replicas: 1 / 5
        selector:
          reference: region-3-zone-e
      cdc:
      - name: region-1-zone-a
        replicas: 1 / 5
        selector:
          reference: region-1-zone-a
      - name: region-1-zone-b
        replicas: 1 / 5
        selector:
          reference: region-1-zone-b
      - name: region-2-zone-c
        replicas: 1 / 5
        selector:
          reference: region-2-zone-c
      - name: region-2-zone-d
        replicas: 1 / 5
        selector:
          reference: region-2-zone-d
      - name: region-3-zone-e
        replicas: 1 / 5
        selector:
          reference: region-3-zone-e
      dn:
        nodeSets:
        - name: cand-region-1-zone-a
          role: Candidate
          replicas: 1
          selector:
            reference: region-1-zone-a
        - name: cand-region-1-zone-b
          role: Candidate
          replicas: 1
          selector:
            reference: region-1-zone-b
        - name: cand-region-3-zone-c
          role: Candidate
          replicas: 1
          selector:
            reference: region-3-zone-c
        - name: cand-region-4-zone-d
          role: Candidate
          replicas: 1
          selector:
            reference: region-4-zone-d
        - name: region-3-zone-e
          role: Voter
          replicas: 1
          selector:
            reference: region-3-zone-e
```
