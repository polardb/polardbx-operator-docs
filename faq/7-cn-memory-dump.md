#### 登录对应的计算节点

```bash
kubectl exec -it <pod-name> -- bash
```

#### dump 内存

```bash
# 通过 JPS 获取进程 ID
jps |grep TDDLLauncher

# dump 内存
jmap -dump:live,format=b,file=heap.bin <pid>
```

#### 拷贝文件到本地

```bash
# 推迟计算节点 Pod
exit

# 拷贝内存文件
kubectl cp <pod-name>:<dump-file-path-and-name> <local-file-name>
```
