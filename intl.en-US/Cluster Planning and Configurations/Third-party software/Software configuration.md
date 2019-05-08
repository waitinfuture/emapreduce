# Software configuration {#concept_ctp_kkn_y2b .concept}

Hadoop, Hive, and Pig require large amounts of configurations. You can use the software configuration feature to modify the software configurations. Currently, you can only perform software configurations when creating a cluster. For example, you want to increase the number of HDFS server handler threads \(dfs.namenode.handler.count\) from 10 to 50. Or you want to decrease the HDFS block size from 128 MB \(default size\) to 64 MB.

## \[DO NOT TRANSLATE\] {#section_bd3_ds5_y2b .section}

\[DO NOT TRANSLATE\]

## Procedure {#section_tsc_2s5_y2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://partners-intl.console.aliyun.com/#/emr).
2.  In the navigation bar, select a region for creating a cluster.
3.  Click **Create Cluster** to go to the Custom Purchase page.
4.  In the Software Configuration step, you can view all services and the corresponding versions. You can turn on **Enable Custom Setting** and enter JSON text in the editor to add configurations or to overwrite the default configurations. A JSON file example is shown as follows.

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

    -   The argument of FileName does not include the file extension.
    -   The argument of ServiceName is required to be capitalized.

ConfigKey refers to the name of a configuration item and ConfigValue refers to the value of the configuration item.

The configuration files of the services are shown as follows.

-   Hadoop

    Filename:

    -   core-site.xml
    -   log4j.properties
    -   Hdfs-site.xml
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

After the settings are complete, click **Next: Hardware Configuration**.

