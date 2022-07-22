1. 执行`perf`命令确认是否安装，若未安装则执行(CentOS)：`sudo yum install perf`
2. 实时查看热点函数：`perf top --call-graph dwarf -p {PID}`，检查是否能看到mysqld的函数栈，类似AHI的问题比较容易看出来
3. 绘制火焰图

```shell
# 如果mysqld在容器中运行
# 则拷贝mysqld的二进制文件到宿主机的相同运行路径下
docker cp {ContainerId}

# 找到mysqld进程号
ps -ef | grep mysqld

# 采样40s
perf record -F 99 -p {pid} -g --call-graph dwarf -- sleep 40

# 将二进制的 perf.data 转化为文本形式
perf script > out.perf

# 绘制火焰图
# 火焰图工具下载见文末
./FlameGraph-master/stackcollapse-perf.pl out.perf > out.folded
./FlameGraph-master/flamegraph.pl out.folded > mysqld.svg
```

火焰图工具：[FlameGraph-master.zip](./FlameGraph-master.zip)

