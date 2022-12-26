## CN
拉取 PolarDB-X SQL 代码，执行docker_build.sh 即可。
[https://github.com/polardb/polardbx-sql/blob/main/docker_build.sh](https://github.com/polardb/polardbx-sql/blob/main/docker_build.sh)

## DN

```dockerfile
FROM centos:7

# Install essential utils
RUN yum update -y && \
    yum install sudo hostname telnet net-tools vim tree less libaio numactl-libs python3 -y && \
    yum clean all && rm -rf /var/cache/yum && rm -rf /var/tmp/yum-*

# Remove localtime to make mount possible.
RUN rm -f /etc/localtime

# Create user "mysql" and add it into sudo group
RUN useradd -ms /bin/bash mysql && \
    echo "mysql:mysql" | chpasswd && \
    echo "mysql    ALL=(ALL)    NOPASSWD: ALL" >> /etc/sudoers

# Install polardbx engine's rpm, use URL to reduce the final image size.
ARG POLARDBX_ENGINE_RPM_URL=<url-to-dn-rpm-package>.rpm

RUN yum install -y ${POLARDBX_ENGINE_RPM_URL} && \
    yum clean all && rm -rf /var/cache/yum && rm -rf /var/tmp/yum-* # && \
    # mv /u01/xcluster80_current/* /opt/galaxy_engine/ && rm -rf /u01

# Target to polardbx engine home.
WORKDIR /opt/galaxy_engine

# Setup environment variables.
ENV POLARDBX_ENGINE_HOME=/opt/galaxy_engine
ENV PATH=$POLARDBX_ENGINE_HOME/bin:$PATH

ENTRYPOINT mysqld
```

1. 把上面 Dockerfile 存下来 
2. 打包 rpm（文档待更新）
3. 执行

```bash
docker build --build-arg POLARDBX_ENGINE_RPM_URL=${POLARDBX_ENGINE_RPM_URL} -t polardbx-engine .
```


## CDC
拉取仓库代码，执行 build.sh 即可。
详见：[https://github.com/polardb/polardbx-cdc/blob/main/docker/build.sh](https://github.com/polardb/polardbx-cdc/blob/main/docker/build.sh)
