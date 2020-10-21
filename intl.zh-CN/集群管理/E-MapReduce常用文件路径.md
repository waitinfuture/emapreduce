# E-MapReduce常用文件路径

本文为您介绍E-MapReduce中常用文件的路径。您可以登录Master节点查看常用文件的安装路径。

## 大数据组件目录

软件安装目录在/usr/lib/xxx下，例如：

-   Hadoop：/usr/lib/hadoop-current
-   Spark ：/usr/lib/spark-current
-   Hive：/usr/lib/hive-current
-   Flink：/usr/lib/flink-current
-   Flume：/usr/lib/flume-current

您也可以通过登录Master节点，执行env \|grep xxx命令查看软件的安装目录。

例如，执行以下命令，查看Hadoop的安装目录。

```
[root@emr-header-1 ~]# env |grep hadoop
```

返回如下信息，其中`/usr/lib/hadoop-current`为Flume的安装目录。

```
HADOOP_LOG_DIR=/var/log/hadoop-hdfs
HADOOP_HOME=/usr/lib/hadoop-current
YARN_PID_DIR=/usr/lib/hadoop-current/pids
HADOOP_PID_DIR=/usr/lib/hadoop-current/pids
HADOOP_MAPRED_PID_DIR=/usr/lib/hadoop-current/pids
JAVA_LIBRARY_PATH=/usr/lib/hadoop-current/lib/native:
PATH=/usr/lib/sqoop-current/bin:/usr/lib/spark-current/bin:/usr/lib/hive-current/hcatalog/bin:/usr/                                                                                                  lib/hive-current/bin:/usr/lib/flume-current/bin:/usr/lib/flink-current/bin:/usr/lib/datafactory-cur                                                                                                  rent/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/usr/lib/b2monitor-current//bin:/usr/lib                                                                                                  /b2smartdata-current//bin:/usr/lib/b2jindosdk-current//bin:/usr/lib/flow-agent-current/bin:/usr/lib                                                                                                  /hadoop-current/bin:/usr/lib/hadoop-current/sbin:/usr/lib/hadoop-current/bin:/usr/lib/hadoop-curren                                                                                                  t/sbin:/root/bin
HADOOP_CLASSPATH=/usr/lib/hadoop-current/lib/*:/usr/lib/tez-current/*:/usr/lib/tez-current/lib/*:/e                                                                                                  tc/ecm/tez-conf:/opt/apps/extra-jars/*:/usr/lib/spark-current/yarn/spark-2.4.5-yarn-shuffle.jar
HADOOP_CONF_DIR=/etc/ecm/hadoop-conf
YARN_LOG_DIR=/var/log/hadoop-yarn
HADOOP_MAPRED_LOG_DIR=/var/log/hadoop-mapred
```

## 日志目录

组件日志目录在/mnt/disk1/log/xxx 下，例如：

-   Yarn ResourceManager日志：Master节点/mnt/disk1/log/hadoop-yarn
-   Yarn NodeNanager日志：Slave节点/mnt/disk1/log/hadoop-yarn
-   HDFS NameNode日志：Master节点/mnt/disk1/log/hadoop-hdfs
-   HDFS DataNode日志：Slave节点/mnt/disk1/log/hadoop-yarn
-   Hive日志：Master节点/mnt/disk1/log/hive

## 配置文件

配置文件目录在/etc/ecm/xxx下，例如：

-   Hadoop：/etc/ecm/hadoop-conf/
-   Spark：/etc/ecm/spark-conf/
-   Hive：/etc/ecm/hive-conf/
-   Flink：/etc/ecm/flink-conf/
-   Flume：/etc/ecm/flume-conf/

如果您需要修改配置文件中的参数，请登录E-MapReduce控制台操作，通过SSH方式只能浏览配置文件中的参数。

