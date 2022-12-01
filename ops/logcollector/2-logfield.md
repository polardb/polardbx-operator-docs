# 日志字段说明
本文介绍如何在logstash上报的日志内容中，各字段的说明。
## SQL日志
### 字段说明
| **字段组** | **字段名称** | **描述**                                                             |
| --- | --- |--------------------------------------------------------------------|
| fields | instance_id | 实例名称。                                                              |
|  | node_name | 计算节点pod所在的node名称。                                                  |
|  | log_type | 日志类型。                                                              |
|  | pod_name | 计算节点pod名称。                                                         |
| message | log_time | 日志打印时间戳。                                                           |
|  | physical_affected_rows | 物理影响行数。                                                            |
|  | total_physical_get_connection_time_cost | 物理连接获取耗时,  单位ns。                                                   |
|  | sql | 被执行的SQL语句。                                                         |
|  | fetched_rows | 从存储拉取的记录行数。                                                        |
|  | total_physical_time_cost | 物理执行总耗时，包括物理SQL耗时与物理结果集耗时，单位ns。                                    |
|  | total_physical_sql_execution_time_cost | 物理SQL执行的总耗时之和，单位ns。                                                |
|  | schema | 数据库。                                                               |
|  | logical_time_cost | 逻辑层执行耗时（即DRDS层的消耗的CPU时间），单位ns。                                     |
|  | affected_rows | 若执行的是DML，表示受影响的行数；若执行的是查询语句，表示返回结果的行数。                             |
|  | logical_optimizer_time_cost | 从sql接收到生成Plan的时间，即SQL在优化器的总耗时，单位ns。                                |
|  | logical_executor_time_cost | 完整执行整个plan的总逻辑耗时（这个指标已去除了物理层总耗时），单位ns。                             |
|  | workload_type | SQL执行时的负载类型，取值范围如下：TP：事务类型的负载；AP：分析类型的负载。                          |
|  | response_time | 响应时间，单位：微秒。                                                        |
|  | template_id | 模板SQL的哈希值。                                                         |
|  | user | 执行SQL的用户名。                                                         |
|  | trace_id | SQL执行的TRACE ID。                                                    |
|  | extra_info | 额外信息。包括客户端地址(ipport)，prepare语句编号(stmt_id)，事物策略(trx)，负载类型(wt)，内核版本(ver) |


### 例子
```json
{
  "_index": "cn_sql_log-2022.11.16",
  "_type": "_doc",
  "_id": "oz1rf4QB-sddlgYFeymE",
  "_version": 1,
  "_score": null,
  "_source": {
    "@version": "1",
    "@timestamp": "2022-11-16T07:50:58.507Z",
    "host": {
      "name": "filebeat-vtggm"
    },
    "fields": {
      "pod_name": "busu-pxchostnet-ql8p-cn-default-84cdc67d84-4w8ww",
      "log_type": "cn_sql_log",
      "instance_id": "busu-pxchostnet",
      "node_name": "cn-beijing.192.168.1.250"
    },
    "message": {
      "total_physical_sql_execution_time_cost": 680097,
      "total_physical_get_connection_time_cost": 4544,
      "fetched_rows": 0,
      "trace_id": "153a4ba63d007000",
      "extra_info": "ipport=192.168.0.3:58888 wt=TP ver=5.4.13-20220621",
      "affected_rows": 0,
      "template_id": "3e4e0512",
      "logical_time_cost": -3315586127,
      "user": "polardbx_root",
      "response_time": 1203,
      "physical_affected_rows": 0,
      "schema": "polardbx",
      "logical_optimizer_time_cost": 27655,
      "log_time": "2022-11-16 15:50:58.509",
      "sql": "SELECT engine, external_endpoint, file_uri, access_key_id, access_key_secret FROM metadb.file_storage_info",
      "total_physical_time_cost": 686243,
      "logical_executor_time_cost": -3315613782
    },
    "tags": [
      "beats_input_codec_plain_applied"
    ]
  },
  "fields": {
    "@timestamp": [
      "2022-11-16T07:50:58.507Z"
    ]
  },
  "sort": [
    1668585058507
  ]
}
```

## 慢日志
### 字段说明
| **字段组** | **字段名称** | **描述** |
| --- | --- | --- |
| fields | instance_id | 实例名称。 |
|  | log_type | 日志类型。 |
|  | node_name | 计算节点pod所在的node名称。 |
|  | pod_name | 计算节点pod名称。 |
| message | log_time | 日志打印时间。 |
|  | time | 执行时间，单位ms。 |
|  | host | 客户端ip。 |
|  | port | 客户端端口。 |
|  | sql | 执行SQL语句。 |
|  | affected_rows | 影响行数。 |
|  | trace_id | 跟踪编号。 |
|  | server_version | 内核版本。 |
|  | user | 用户名。 |
|  | schema | 数据库名。 |

### 例子
```json
{
  "_index": "cn_slow_log-2022.08.09",
  "_type": "_doc",
  "_id": "CxdugYIB4-sIO7p8dvv-",
  "_version": 1,
  "_score": null,
  "_source": {
    "fields": {
      "instance_id": "busu-pxchostnet",
      "node_name": "cn-beijing.192.168.0.207",
      "log_type": "cn_slow_log",
      "pod_name": "busu-pxchostnet-ldcw-cn-default-c754df994-xqhhj"
    },
    "@version": "1",
    "message": {
      "log_time": "2022-08-09 15:07:55.720",
      "time": "2001",
      "host": "127.0.0.1",
      "port": "35812",
      "sql": "select sleep(2)",
      "affected_rows": "1",
      "trace_id": "14bacc6508402000",
      "server_version": "5.4.13-16534775",
      "user": "polardbx_root",
      "schema": "busudb"
    },
    "host": {
      "name": "filebeat-wg47m"
    },
    "@timestamp": "2022-08-09T07:07:55.720Z",
    "tags": [
      "beats_input_codec_plain_applied"
    ]
  },
  "fields": {
    "@timestamp": [
      "2022-08-09T07:07:55.720Z"
    ]
  },
  "highlight": {
    "message.schema": [
      "@kibana-highlighted-field@busudb@/kibana-highlighted-field@"
    ]
  },
  "sort": [
    1660028875720
  ]
}
```

## 错误日志
### 字段说明
| **字段组** | **字段名称** | **描述** |
| --- | --- | --- |
| fields | instance_id | 实例名称。 |
|  | log_type | 日志类型。 |
|  | node_name | 计算节点pod所在的node名称。 |
|  | pod_name | 计算节点pod名称。 |
|  / | logger | 打印者名称。 |
|  | loglevel | 日子级别 |
|  | message | 错误内容 |
|  | thread | 线程名称 |

### 例子
```json
{
  "_index": "cn_tddl_log-2022.08.09",
  "_type": "_doc",
  "_id": "3oWGgYIBMBS_DyGstwZ2",
  "_version": 1,
  "_score": null,
  "_source": {
    "loglevel": "WARN",
    "thread": "ManagerExecutor-14-thread-160",
    "logger": " com.alibaba.polardbx.manager.ManagerConnection",
    "host": {
      "name": "filebeat-wg47m"
    },
    "message": "[user=polardbx_root,host=127.0.0.1,port=37150,schema=null] Index: 17, Size: 17\njava.lang.IndexOutOfBoundsException: Index: 17, Size: 17\n\tat java.util.ArrayList.rangeCheck(ArrayList.java:659)\n\tat java.util.ArrayList.get(ArrayList.java:435)\n\tat com.alibaba.polardbx.net.packet.RowDataPacket.getPacketLength(RowDataPacket.java:111)\n\tat com.alibaba.polardbx.net.packet.RowDataPacket.write(RowDataPacket.java:85)\n\tat com.alibaba.polardbx.manager.response.ShowHtc.execute(ShowHtc.java:130)\n\tat com.alibaba.polardbx.manager.handler.ShowHandler.handle(ShowHandler.java:93)\n\tat com.alibaba.polardbx.manager.ManagerQueryHandler.query(ManagerQueryHandler.java:68)\n\tat com.alibaba.polardbx.net.handler.QueryHandler.queryRaw(QueryHandler.java:29)\n\tat com.alibaba.polardbx.net.FrontendConnection.query(FrontendConnection.java:474)\n\tat com.alibaba.polardbx.net.handler.FrontendCommandHandler.handle(FrontendCommandHandler.java:65)\n\tat com.alibaba.polardbx.manager.ManagerConnection.lambda$handleData$0(ManagerConnection.java:62)\n\tat java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)\n\tat java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)\n\tat java.lang.Thread.run(Thread.java:855)\n\tat com.alibaba.wisp.engine.WispTask.runOutsideWisp(WispTask.java:299)\n\tat com.alibaba.wisp.engine.WispTask.runCommand(WispTask.java:274)\n\tat com.alibaba.wisp.engine.WispTask.access$100(WispTask.java:53)\n\tat com.alibaba.wisp.engine.WispTask$CacheableCoroutine.run(WispTask.java:241)\n\tat java.dyn.CoroutineBase.startInternal(CoroutineBase.java:62)",
    "fields": {
      "instance_id": "busu-pxchostnet",
      "node_name": "cn-beijing.192.168.0.207",
      "log_type": "cn_tddl_log",
      "pod_name": "busu-pxchostnet-ldcw-cn-default-c754df994-xqhhj"
    },
    "@version": "1",
    "tags": [
      "beats_input_codec_plain_applied"
    ],
    "@timestamp": "2022-08-09T07:34:16.157Z"
  },
  "fields": {
    "@timestamp": [
      "2022-08-09T07:34:16.157Z"
    ]
  },
  "sort": [
    1660030456157
  ]
}
```