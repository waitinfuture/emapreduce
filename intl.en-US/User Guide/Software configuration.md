# Software configuration {#concept_ctp_kkn_y2b .concept}

Hadoop, Hive, Pig, and other relevant software contain numerous configurations that can be changed through the software configuration function. For example, the number of service threads in HDFS server dfs.namenode.handler.count is 10 by default and will be increased to 50, and the size of HDFS file block dfs.blocksize is 128 MB by default and will be decreased to 64 MB because the system contains only small files.

## Purpose of software configuration {#section_bd3_ds5_y2b .section}

The function can only be performed once during the startup of a cluster.

## Procedure {#section_tsc_2s5_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/).
2.  Select the region and the created cluster associated with the region is listed.
3.  Click **Create Cluster** to enter the cluster creation page.
4.  All contained software and corresponding versions can be seen in the software configuration of cluster creation. Change the configuration of the cluster by selecting a corresponding json format configuration file in the \(optional\) software configuration box. Then, override or add to the defaulted cluster parameters. The sample of a .json file is as follows:

    ```
    {
     "configurations": [
         {
             "classification": "core-site",
             "properties": {
                 "Fs. Trash. Interval": "61"
             }
         },
         {
             "classification": "hadoop-log4j",
             "properties": {
                 "hadoop.log.file": "hadoop1.log",
                 "hadoop.root.logger": "INFO",
                 "a.b.c": "ABC"
             }
         },
         {
             "classification": "hdfs-site",
             "properties": {
                 "dfs.namenode.handler.count": "12"
             }
         },
         {
             "classification": "mapred-site",
             "properties": {
                 "mapreduce.task.io.sort.mb": "201"
             }
         },
         {
             "classification": "yarn-site",
             "properties": {
                 "Hadoop. Security. Groups. cache. secs": 251 ",
                 "yarn.nodemanager.remote-app-log-dir": "/tmp/logs1"
             }
         },
         {
             "classification": "httpsfs-site",
             "properties": {
                 "a.b.c.d": "200"
             }
         },
         {
             "classification": "capacity-scheduler",
             "properties": {
                 "yarn.scheduler.capacity.maximum-am-resource-percent": "0.2"
             }
         },
         {
             "classification": "hadoop-env",
             "properties": {
                 "BC":"CD"
             },
             "configurations":[
                 {
                     "classification":"export",
                     "properties": {
                         "AB":"${BC}",
                         "HADOOP_CLIENT_OPTS":"\"-Xmx512m -Xms512m $HADOOP_CLIENT_OPTS\""
                     }
                 }
             ]
         },
         {
             "classification": "httpfs-env",
             "properties": {
             },
             "configurations":[
                 {
                     "classification":"export",
                     "properties": {
                         "HTTPFS_SSL_KEYSTORE_PASS":"passwd"
                     }
                 }
             ]
         },
         {
             "classification": "mapred-env",
             "properties": {
             },
             "configurations":[
                 {
                     "classification":"export",
                     "properties": {
                         "HADOOP_JOB_HISTORYSERVER_HEAPSIZE":"1001"
                     }
                 }
             ]
         },
         {
             "classification": "yarn-env",
             "properties": {
             },
             "configurations":[
                 {
                     "classification":"export",
                     "properties": {
                         "HADOOP_YARN_USER":"${HADOOP_YARN_USER:-yarn1}"
                     }
                 }
             ]
         },
         {
             "classification": "pig",
             "properties": {
                 "pig.tez.auto.parallelism": "false"
             }
         },
         {
             "classification": "pig-log4j",
             "properties": {
                 "log4j.logger.org.apache.pig": "error, A"
             }
         },
         {
             "classification": "hive-env",
             "properties": {
                 "BC":"CD"
             },
             "configurations":[
                 {
                     "classification":"export",
                     "properties": {
                         "AB":"${BC}",
                         "HADOOP_CLIENT_OPTS1":"\"-Xmx512m -Xms512m $HADOOP_CLIENT_OPTS1\""
                     }
                 }
             ]
         },
         {
             "classification": "hive-site",
             "properties": {
                 "hive.tez.java.opts": "-Xmx3900m"
             }
         },
         {
             "classification": "hive-exec-log4j",
             "properties": {
                 "log4j.logger.org.apache.zookeeper.ClientCnxnSocketNIO": "INFO,FA"
             }
         },
         {
             "classification": "hive-log4j",
             "properties": {
                 "log4j.logger.org.apache.zookeeper.server.NIOServerCnxn": "INFO,DRFA"
             }
         }
     ]
    }
    ```


The classification parameter designates the configuration file to change. The parameter properties stores the key-value pair that requires changes. When the default configuration file has a corresponding key, override the value, otherwise, add the corresponding key-value pair.

The correspondence between configuration file and classification is shown in the following table.

-   Hadoop

    |Filename|Classification|
    |:-------|:-------------|
    |core-site.xml|core-site|
    |log4j.properties|Hadoop-log4j|
    |Hdfs-site.xml|hdfs-site|
    |mapred-site.xml|mapred-site|
    |yarn-site.xml|yarn-site|
    |httpsfs-site.xml|httpsfs-site|
    |capacity-scheduler.xml|capacity-scheduler|
    |hadoop-env.sh|hadoop-env|
    |httpfs-env.sh|httpfs-env|
    |mapred-env.sh|mapred-env|
    |yarn-env.sh|yarn-env|

-   Pig

    |Filename|classification|
    |:-------|:-------------|
    |pig.properties|pig|
    |log4j.properties|pig-log4j|

-   Hive

    |Filename|classification|
    |:-------|:-------------|
    |hive-env.sh|hive-env|
    |hive-site.xml|hive-site|
    |hive-exec-log4j.properties|hive-exec-log4j|
    |hive-log4j.properties|hive-log4j|


The core-site and other flat XML files only have one layer. All configurations are put in properties. The hadoop-en v and other sh files may have two layers of structures and can be set in the embedded configurations mode. See hadoop-env in the example where -Xmx512m -Xms512m setting is added for HADOOP\_CLIENT\_OPTS property of export.

After setting, confirm and click **Next** step.

