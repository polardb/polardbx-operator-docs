备库健康检查
==============
本文讲解，如何对备库的健康度进行检查，来判断是否要发起一次备库重搭。

## POD检查
查看有没有不在Running状态的DN POD
```bash
kubectl get pods -l xstore/name --show-labels | grep -v Running
```
检查不健康的POD所在xstore是否在做正常的任务，比如变配、升降级等预期内的任务，如果不是，且该POD一直无法恢复ready状态，可以考虑发起一个备库重搭任务。

## 复制线程和延迟检查
由于程序bug，可能导致备库上的复制线程发生中断。通过在备库上执行如下语句，可查看复制状态。
```sql
show slave status
```
如果Slave_SQL_Running属性为No，且Last_Error属性不为空，则表示备库复制任务出现问题，这时候首先明确导致复制中断原因，之后发起备库重搭来恢复备库。

由于某些原因（备库挂掉了很长时间、备库宿主机有问题等），导致备库延迟过大，追上主库需要非常多的时间，比如需要几十个小时，这时可选择发起一次备库重搭。

### 如何查看备库延迟？
1. 在备库上执行 show slave status，查看Seconds_Behind_Master属性；
2. 在主库上执行 select * from information_schema.alisql_cluster_global 比较备库是主库上APPLIED_INDEX属性值上的差异，主备库上的APPLIED_INDEX属性值的增长速度，可预估出需要多长时间才能追上主库的日志。