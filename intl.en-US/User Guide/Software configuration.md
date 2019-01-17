# Software configuration {#concept_ctp_kkn_y2b .concept}

Software such as Hadoop, Hive, and Pig contain a large number of configurations that can be modified using the software configuration function. For example, you may want to increase the number of service threads in the dfs.namenode.handler.count HDFS server from 10 to 50, or decrease the default size of HDFS file block dfs.blocksize from 128 MB to 64 MB because the system only contains small files. The software configuration function can only be performed once when a cluster is started.

## Procedure {#section_tsc_2s5_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/).
2.  Select a region. The cluster associated with this region is listed.
3.  Click **Create Cluster** to enter the cluster creation page.
4.  All of the software and the corresponding versions can be seen in the Software Configuration step during cluster creation. If you want to change the configuration of the cluster, select a corresponding .json configuration file in the **Enable Custom Setting** box. You can then override or add to the default cluster parameters. An example of a .json file is as follows:

    ```
    [
        {
            "ServiceName":"YARN",
            "FileName":"yarn-site",
            "ConfigKey":"yarn.nodemanager.resource.cpu-vcores",
            "ConfigValue":"8"
        },
        {
            "ServiceName":"YARN",
            "FileName":"yarn-site",
            "ConfigKey":"aaa",
            "ConfigValue":"bbb"
        }
    ]
    ```


**Note:** 

-   The suffix of the actual incoming FileName parameter needs to be removed.
-   Capitalize all letters of the ServiceName parameter name.

ConfigKey is the name of a configuration, whereas ConfigValue is a specific value.

Configuration files of all services are as follows:

-   Hadoop

    Filename:

    -   core-site.xml
    -   log4j.properties
    -   hdfs-site.xml
    -   mapred-site.xml
    -   yarn-site.xml
    -   httpsfs-site.xml
    -   capacity-scheduler.xml
    -   hadoop-env.sh
    -   httpfs-env.sh
    -   mapred-env.sh
    -   yarn-env.sh
-   Pig

    Filename:

    -   pig.properties
    -   log4j.properties
-   Hive

    Filename:

    -   hive-env.sh
    -   hive-site.xml
    -   hive-exec-log4j.properties
    -   hive-log4j.properties

After setting configuration files, click **Next** to complete software configurations.

