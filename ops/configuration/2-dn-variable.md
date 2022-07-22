## 修改 PXC YAML
直接修改 pxc yaml 的 .spec.config.dn即可，添加相关的mysql 参数，如下图所示：

```shell
# DN 相关配置
    dn:
      # DN my.cnf 配置，覆盖模板部分
      mycnfOverwrite: |-
        loose_binlog_checksum: crc32
      logPurgeInterval: 5m
```

注意：如果部分my.cnf 参数需要重启后才能生效，需要手动重启 DN 的 mysql 进程。

非 my.cnf 参数目前只支持设置 binlog的清理时间，修改 .spec.config.dn.logPurgeInterval 即可。

## Set Global 指令
除了修改yaml 外，也可以通过 CN 的 set global 指令修改 DN 参数，登录CN，执行如下SQL：

```shell
set ENABLE_SET_GLOBAL = true;			-- 开启 set global 功能
set global {dn 参数};		-- 关闭 sql日志
```
