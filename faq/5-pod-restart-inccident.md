Pod 的重启计数在任何一个容器被重启时都会增加，因此首先需要确定是哪个容器重启：使用 `kubectl describe pod {name}` 查看是哪个容器最近在重启。

重启的原因通常需要排查日志来得知，通常有几种原因：

1. 容器的 liveness probe 失败超过阈值（通常为 3），这个需要排查进程是否存活以及相关的日志来排除问题
1. 容器 1 号进程意外退出，例如在容器内执行了 `killall`

日志排查合集：

1. [CN 日志](../ops/component/cn/4-cn-log.md)
1. [GMS/DN 日志](../ops/component/dn/3-dn-log.md)
1. [CDC 日志](../ops/component/cdc/2-cdc-node-login.md)
