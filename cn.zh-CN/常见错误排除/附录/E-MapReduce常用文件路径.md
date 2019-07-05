# E-MapReduce常用文件路径 {#concept_1012525 .concept}

E-MapReduce 是阿里云上的开源大数据平台，用户可以登录集群主节点查看相关安装路径。 登录后也可以使用 env |grep xxx 查看。

## 大数据组件目录 {#section_f1f_s73_wvx .section}

软件安装目录在 /usr/lib/xxx 下，例如：

-   Hadoop：/usr/lib/hadoop-current
-   Spark ：/usr/lib/spark-current
-   Hive：/usr/lib/hive-current
-   Flink：/usr/lib/flink-current

## 日志目录 {#section_j47_l4l_503 .section}

组件日志目录在 /mnt/disk1/log/xxx 下，例如：

-   Yarn ResourceManager 日志：master 节点 /mnt/disk1/log/hadoop-yarn
-   Yarn NodeNanager 日志：slave 节点 /mnt/disk1/log/hadoop-yarn
-   HDFS NameNode 日志：master 节点 /mnt/disk1/log/hadoop-hdfs
-   HDFS DataNode 日志：slave 节点 /mnt/disk1/log/hadoop-yarn
-   Hive 日志：Master 节点 /mnt/disk1/log/hive

## 配置文件 {#section_x1f_vjq_ose .section}

配置文件目录在 /etc/ecm/xxx 下，如果仅登录节点修改集群中的配置文件，配置文件不会生效。

**说明：** 此处的配置文件只能浏览配置文件的参数，如需修改参数，请到 EMR 控制台操作。

-   Hadoop：/etc/ecm/hadoop-conf/
-   Spark：/etc/ecm/spark-conf/
-   Hive：/etc/ecm/hive-conf/

