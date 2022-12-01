备份存储方式配置
==========

PolarDB-X Operator 从 1.3.0 版本开始支持全量备份恢复功能。在开启集群的备份恢复之前，需要对备份集的存储方式进行配置。

您可以通过如下方式完成备份存储方式的配置。

## 配置备份存储

### 支持的存储方式

目前支持的存储方式如下所示：

* SFTP
* Aliyun OSS

更多存储方式会在后续支持。

### 配置 SFTP 为备份集存储

1. 执行如下命令修改 ConfigMap：
```shell
kubectl -n polardbx-operator-system edit configmap polardbx-hpfs-config
```
在sinks数组中添加自己的sftp配置，如下所示：
```yaml
data:
  config.yaml: |-
    sinks:
      - name: default
        type: sftp
        host: 127.0.0.1
        port: 22
        user: admin
        password: admin
        rootPath: /backup
```
2. 保存之后执行以下命令使配置生效：
```shell
kubectl -n polardbx-operator-system rollout restart daemonsets polardbx-hpfs
```

配置项解释：
- name: 配置项名称，多个 sftp 配置通过 name 区分
- type: 配置项类型(具体参照[支持的存储](#支持的存储)), 取值范围：sftp, oss
- host: 备份机器ip
- port: 备份机器端口
- user: 备份机器账户名
- password: 备份机器密码
- rootPath: 备份集存放的根目录

### 配置阿里云 OSS 为备份集存储

1. 执行如下命令修改 ConfigMap：
```shell
kubectl -n polardbx-operator-system edit configmap polardbx-hpfs-config
```
在sinks数组中添加自己的oss配置，
```yaml
data:
  config.yaml: |-
    sinks:
      - name: default
        type: oss
        endpoint: endpoint
        accessKey: ak
        accessSecret: sk
        bucket: bucket
```
2. 保存之后执行以下命令使配置生效：
```shell
kubectl -n polardbx-operator-system rollout restart daemonsets polardbx-hpfs
```

配置项解释：
- name: 配置项名称，多个 oss 配置通过 name 区分
- type: 配置项类型(具体参照[支持的存储](#支持的存储)), 取值范围：sftp, oss
- endpoint: oss访问域名
- accessKey: oss访问id
- accessSecret: oss访问密钥
- bucket: oss存储空间
> 具体介绍可参考：[OSS产品文档](https://help.aliyun.com/document_detail/31827.html)


## 注意事项

- sinks可以配置多种存储类型，不同类型的配置的name允许重复；每种存储类型也支持多组存储配置，但同一类型下的name不允许重复。
- operator可以在未配置存储的情况下正常运行，但需要使用备份恢复时须添加对应的存储配置。
