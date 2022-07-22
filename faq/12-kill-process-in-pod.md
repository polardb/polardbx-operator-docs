三种情况：

1. kill 了 1 号进程
1. kill 进程后，1 号进程退出了
1. kill 进程后 `liveness`probe 连续失败超过阈值
