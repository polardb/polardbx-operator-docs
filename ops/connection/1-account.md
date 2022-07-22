PolarDB-X 默认的 root 账号都是: polardbx_root，您在登录后可以通过[权限管理语句](https://help.aliyun.com/document_detail/313296.html) 修改密码或者创建新的账号供业务访问。

polardbx_root 账号的密码随机生成，执行下面的命令获取 PolarDB-X root 账号的密码：

```shell
kubectl get secret {PolarDB-X 集群名} -o jsonpath="{.data['polardbx_root']}" | base64 -d - | xargs echo "Password: "
```

期望输出：

```shell
Password:  *******
```
