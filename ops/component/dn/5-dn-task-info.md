注：只有在升级的时候有用。

使用下面的命令查看任务信息，

```bash
kubectl get cm {xstore}-task -o yaml
```

结构为:

```go
type ExecutionContext struct {
	// Topologies in uses.
	Topologies map[int64]*xstore.Topology `json:"topologies,omitempty"`

	// Generation.
	Generation int64 `json:"generation,omitempty"`

	// Current running nodes.
	Running map[string]model.PaxosNodeStatus `json:"running,omitempty"`

	// Tracking nodes. This is the tracking set of the paxos node configuration.
	Tracking map[string]model.PaxosNodeStatus `json:"tracking,omitempty"`

	// Expected nodes.
	Expected map[string]model.PaxosNode `json:"expected,omitempty"`

	// Current usable volumes.
	Volumes map[string]model.PaxosVolume `json:"volumes,omitempty"`

	// Plan.
	Plan *plan.Plan `json:"plan,omitempty"`

	// StepIndex of the plan.
	StepIndex int `json:"step_index,omitempty"`

	PodFactory factory.ExtraPodFactory `json:"-"`
}
```
