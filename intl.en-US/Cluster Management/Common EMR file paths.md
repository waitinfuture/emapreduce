# Common EMR file paths

This topic describes the paths of frequently used files in E-MapReduce \(EMR\). You can log on to the master node of your cluster to view the file paths.

## Big data software

Big data software is installed in the /usr/lib/xxx directory. Examples:

-   Hadoop: /usr/lib/hadoop-current
-   Spark: /usr/lib/spark-current
-   Hive: /usr/lib/hive-current
-   Flink: /usr/lib/flink-current
-   Flume: /usr/lib/flume-current

You can also log on to the master node of your cluster and run the env \|grep xxx command to view a software installation directory.

For example, run the following command to view the installation directory of Hadoop:

```
[root@emr-header-1 ~]# env |grep hadoop
```

The following information is returned. `/usr/lib/hadoop-current` is the installation directory of Hadoop.

```
HADOOP_LOG_DIR=/var/log/hadoop-hdfs
HADOOP_HOME=/usr/lib/hadoop-current
YARN_PID_DIR=/usr/lib/hadoop-current/pids
HADOOP_PID_DIR=/usr/lib/hadoop-current/pids
HADOOP_MAPRED_PID_DIR=/usr/lib/hadoop-current/pids
JAVA_LIBRARY_PATH=/usr/lib/hadoop-current/lib/native:
PATH=/usr/lib/sqoop-current/bin:/usr/lib/spark-current/bin:/usr/lib/hive-current/hcatalog/bin:/usr/lib/hive-current/bin:/usr/lib/datafactory-current/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/usr/lib/b2monitor-current//bin:/usr/lib/b2smartdata-current//bin:/usr/lib/b2jindosdk-current//bin:/usr/lib/flow-agent-current/bin:/usr/lib/hadoop-current/bin:/usr/lib/hadoop-current/sbin:/usr/lib/hadoop-current/bin:/usr/lib/hadoop-current/sbin:/root/bin
HADOOP_CLASSPATH=/usr/lib/hadoop-current/lib/*:/usr/lib/tez-current/*:/usr/lib/tez-current/lib/*:/etc/ecm/tez-conf:/opt/apps/extra-jars/*:/usr/lib/spark-current/yarn/spark-2.4.5-yarn-shuffle.jar
HADOOP_CONF_DIR=/etc/ecm/hadoop-conf
YARN_LOG_DIR=/var/log/hadoop-yarn
HADOOP_MAPRED_LOG_DIR=/var/log/hadoop-mapred
```

## Logs

Component logs are stored in the /mnt/disk1/log/xxx directory. Examples:

-   YARN ResourceManager logs: /mnt/disk1/log/hadoop-yarn in the master node
-   YARN NodeManager logs: /mnt/disk1/log/hadoop-yarn in a core node
-   HDFS NameNode logs: /mnt/disk1/log/hadoop-hdfs in the master node
-   HDFS DataNode logs: /mnt/disk1/log/hadoop-yarn in a core node
-   Hive logs: /mnt/disk1/log/hive in the master node

## Configuration files

Configuration files are stored in the /etc/ecm/xxx directory. Examples:

-   Hadoop: /etc/ecm/hadoop-conf/
-   Spark: /etc/ecm/spark-conf/
-   Hive: /etc/ecm/hive-conf/
-   Flink: /etc/ecm/flink-conf/
-   Flume: /etc/ecm/flume-conf/

You can only view the parameter settings in configuration files by using SSH. If you want to modify parameters, log on to the EMR console and perform the modification.

