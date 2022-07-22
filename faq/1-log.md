## polardbx-operator

执行下面的命令查看 polardbx-operator 所在的 Pod

```bash
kubectl -n polardbx-operator-system get pods -l app.kubernetes.io/component=controller-manager
NAME                                          READY   STATUS    RESTARTS   AGE
polardbx-controller-manager-597685578-kj4rj   1/1     Running   0          10d
```

使用 `kubectl logs` 命令来查看日志

```bash
kubectl -n polardbx-operator-system logs polardbx-controller-manager-597685578-kj4rj
...
2022-05-24T02:44:52.140Z	INFO	controller.xstore	control/context.go:155	Executing command	{"namespace": "default", "xstore": "pxc-demo-q2gq-gms", "engine": "galaxy", "phase": "Running", "stage": "", "trace": "e8ec86c3-f1f8-4baf-b3df-59cf8197ebcb", "action": "ReconcileConsensusRoleLabels", "step": 2, "pod": "default", "container": "engine", "command": ["/tools/xstore/current/venv/bin/python3", "/tools/xstore/current/cli.py", "consensus", "role", "--report-leader"], "timeout": "10s"}
2022-05-24T02:44:52.405Z	INFO	controller.xstore	instance/consensus.go:62	Be aware of pod's role and current leader.	{"namespace": "default", "xstore": "pxc-demo-q2gq-gms", "engine": "galaxy", "phase": "Running", "stage": "", "trace": "e8ec86c3-f1f8-4baf-b3df-59cf8197ebcb", "action": "ReconcileConsensusRoleLabels", "step": 2, "pod": "pxc-demo-q2gq-gms-cand-1", "role": "leader", "leader-pod": "pxc-demo-q2gq-gms-cand-1"}
2022-05-24T02:44:52.405Z	INFO	controller.xstore	instance/consensus.go:218	Leader not changed.	{"namespace": "default", "xstore": "pxc-demo-q2gq-gms", "engine": "galaxy", "phase": "Running", "stage": "", "trace": "e8ec86c3-f1f8-4baf-b3df-59cf8197ebcb", "action": "ReconcileConsensusRoleLabels", "step": 2, "leader-pod": "pxc-demo-q2gq-gms-cand-1"}
2022-05-24T02:44:52.405Z	INFO	controller.xstore	instance/volumes.go:193	Not time to update sizes, skip.	{"namespace": "default", "xstore": "pxc-demo-q2gq-gms", "engine": "galaxy", "phase": "Running", "stage": "", "trace": "e8ec86c3-f1f8-4baf-b3df-59cf8197ebcb", "action": "UpdateHostPathVolumeSizesPer1m0s", "step": 6}
2022-05-24T02:44:52.405Z	INFO	controller.xstore	instance/common.go:117	Update observed generation.	{"namespace": "default", "xstore": "pxc-demo-q2gq-gms", "engine": "galaxy", "phase": "Running", "stage": "", "trace": "e8ec86c3-f1f8-4baf-b3df-59cf8197ebcb", "action": "UpdateObservedGeneration", "step": 10, "previous-generation": 2, "current-generation": 2}
2022-05-24T02:44:52.405Z	INFO	controller.xstore	instance/common.go:108	Update observed topology and config.	{"namespace": "default", "xstore": "pxc-demo-q2gq-gms", "engine": "galaxy", "phase": "Running", "stage": "", "trace": "e8ec86c3-f1f8-4baf-b3df-59cf8197ebcb", "action": "UpdateObservedTopologyAndConfig", "step": 11, "current-generation": 2}
2022-05-24T02:44:52.405Z	INFO	controller.xstore	control/common.go:62	Loop while running	{"namespace": "default", "xstore": "pxc-demo-q2gq-gms", "engine": "galaxy", "phase": "Running", "stage": "", "trace": "e8ec86c3-f1f8-4baf-b3df-59cf8197ebcb", "action": "RetryAfter10s", "step": 12}
2022-05-24T02:44:52.405Z	INFO	controller.xstore	instance/status.go:158	Display status updated!	{"namespace": "default", "xstore": "pxc-demo-q2gq-gms", "engine": "galaxy", "phase": "Running", "stage": "", "trace": "e8ec86c3-f1f8-4baf-b3df-59cf8197ebcb", "defer_exec": true, "action": "UpdateDisplayStatus", "step": 13}
2022-05-24T02:44:52.405Z	INFO	controller.xstore	instance/status.go:52	Object not changed.	{"namespace": "default", "xstore": "pxc-demo-q2gq-gms", "engine": "galaxy", "phase": "Running", "stage": "", "trace": "e8ec86c3-f1f8-4baf-b3df-59cf8197ebcb", "defer_exec": true, "action": "PersistentXStore", "step": 14}
2022-05-24T02:44:52.405Z	INFO	controller.xstore	instance/status.go:41	Status not changed.	{"namespace": "default", "xstore": "pxc-demo-q2gq-gms", "engine": "galaxy", "phase": "Running", "stage": "", "trace": "e8ec86c3-f1f8-4baf-b3df-59cf8197ebcb", "defer_exec": true, "action": "PersistentStatus", "step": 15}
```
