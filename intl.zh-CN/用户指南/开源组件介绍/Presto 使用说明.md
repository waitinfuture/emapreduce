# Presto 使用说明 {#concept_rkf_f1x_y2b .concept}

2.0.0 以上版本支持 [presto](https://prestodb.io/)，选择支持 presto 的镜像并勾选 presto 软件即可在 E-MapReduce 中使用 presto。

集群创建后，登录 master，presto 软件被安装在/usr/lib/presto-current 目录，可以 jps 看到 PrestoServer 进程。

presto 服务进程分为 coordinator 和 worker，master 上（HA 集群为 hostname 以emr-header-1 开头的 master 节点）启动 coordinator，core 节点启动 worker 进程。服务进程的配置在 /usr/lib/presto-current/etc 目录下，其中 coordinator 使用 coordinator-config.properties，worker 使用 worker-config.preperties,其他配置文件公用。web 端口设置为 9090。

presto 服务默认设置了 Hive 的支持，连接集群 hive 的 metastore，可以读取 Hive 的表信息，进行查询。集群预装了 presto cli，可以直接执行以下命令：

```
presto --server localhost: 9090 --catalog hive --schema default --user hadoop --execute "show tables"
```

查看 Hive 的表。需要注意同步 Hive 表会有几秒的延时。

