## 查询 XStore 列表
执行如下命令查询所有DN 的列表：

```shell
kubectl get xstore -l polardbx/name={实例名}
```

得到如下输出：

```shell
NAME                  LEADER                       READY   PHASE     DISK       VERSION   AGE
tunan-oss-drsg-dn-0   tunan-oss-drsg-dn-0-cand-1   3/3     Running   11.7 GiB   8.0.18    20d
tunan-oss-drsg-dn-1   tunan-oss-drsg-dn-1-cand-1   3/3     Running   11.0 GiB   8.0.18    20d
```

PHASE 显示的是 每个 DN 的状态，LEADER 显示的是当前 DN 的 Leader pod。

## 查看 DN Pod
如果想查询PolarDB-X DN 的所有 pod，执行如下命令：

```shell
kubectl get pod -l polardbx/name={实例名},polardbx/role=dn
```

得到如下结果：

```shell
NAME                         READY   STATUS    RESTARTS   AGE
tunan-oss-drsg-dn-0-cand-0   3/3     Running   0          20d
tunan-oss-drsg-dn-0-cand-1   3/3     Running   0          20d
tunan-oss-drsg-dn-0-log-0    3/3     Running   0          20d
tunan-oss-drsg-dn-1-cand-0   3/3     Running   0          20d
tunan-oss-drsg-dn-1-cand-1   3/3     Running   0          20d
tunan-oss-drsg-dn-1-log-0    3/3     Running   0          20d
```

如果想查看每个dn pod的角色，执行如下命令：

```shell
kubectl get pod -l polardbx/name={实例名},polardbx/role=dn --show-labels
```

得到如下输出, 其中xstore/role=follower 表示的就是pod的role。
> 注：如果xstore/role 该标签没有值，说明 DN 正在进行选主或者选主出现了问题

```shell
NAME                         READY   STATUS    RESTARTS   AGE   LABELS
tunan-oss-drsg-dn-0-cand-0   3/3     Running   0          20d   polardbx/dn-index=0,polardbx/name=tunan-oss,polardbx/rand=drsg,polardbx/role=dn,xstore/generation=2,xstore/name=tunan-oss-drsg-dn-0,xstore/node-role=candidate,xstore/node-set=cand,xstore/pod=tunan-oss-drsg-dn-0-cand-0,xstore/port-lock=16148,xstore/role=follower
```

## 查看特定角色的 DN pod
查看所有的 leader pod：

```shell
kubectl get pod -l polardbx/name={实例名},polardbx/role=dn,xstore/role=leader
```

查看所有的 follower pod：

```shell
kubectl get pod -l polardbx/name={实例名},polardbx/role=dn,xstore/role=follower
```

查看所有的 logger pod

```shell
kubectl get pod -l polardbx/name={实例名},polardbx/role=dn,xstore/role=logger
```
