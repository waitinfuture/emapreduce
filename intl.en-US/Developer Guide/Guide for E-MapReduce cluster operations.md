# Guide for E-MapReduce cluster operations {#concept_f2p_134_hfb .concept}

This topic describes multiple operation methods for EMR clusters, which make it easier for you to independently perform operations on components.

**Note:** For later cluster versions \(3.2 and later\), all operations on components can be performed through the Cluster Management page in the console. We recommend that you manage components using web pages.

## Some general environment variables {#section_lg5_hj4_hfb .section}

Enter the env command to see environment variable configurations that are similar to those in the following example. The actual configurations can be different. This example is for reference only.

```
JAVA_HOME=/usr/lib/jvm/java
HADOOP_HOME=/usr/lib/hadoop-current
HADOOP_CLASSPATH=/usr/lib/hbase-current/lib/*:/usr/lib/tez-current/*:/usr/lib/tez-current/lib/*:/etc/emr/tez-conf:/usr/lib/hbase-current/lib/*:/usr/lib/tez-current/*:/usr/lib/tez-current/lib/*:/etc/emr/tez-conf:/opt/apps/extra-jars/*:/opt/apps/extra-jars/*
HADOOP_CONF_DIR=/etc/emr/hadoop-conf
SPARK_HOME=/usr/lib/spark-current
SPARK_CONF_DIR=/etc/emr/spark-conf
HBASE_HOME=/usr/lib/hbase-current
HBASE_CONF_DIR=/etc/emr/hbase-conf
HIVE_HOME=/usr/lib/hive-current
HIVE_CONF_DIR=/etc/emr/hive-conf
PIG_HOME=/usr/lib/pig-current
PIG_CONF_DIR=/etc/emr/pig-conf
TEZ_HOME=/usr/lib/tez-current
TEZ_CONF_DIR=/etc/emr/tez-conf
ZEPPELIN_HOME=/usr/lib/zeppelin-current
ZEPPELIN_CONF_DIR=/etc/emr/zeppelin-conf
HUE_HOME=/usr/lib/hue-current
HUE_CONF_DIR=/etc/emr/hue-conf
PRESTO_HOME=/usr/lib/presto-current
PRESTO_CONF_DIR=/etc/emr/presot-conf
```

## Start and stop a component using the Web UI {#section_jwd_zj4_hfb .section}

You can use the Web UI of the E-MapReduce service to start, stop, and restart the component that runs on the specified ECS instance. Operations on different components are similar, for example the HDFS service. Follow these steps to start, stop, and restart the DataNode component for the emr-worker-1 instance.

1.  On the Cluster list page, click **Manage** in the **Actions** column for the cluster to perform operations on.
2.  On the Clusters and Services page, click the **HDFS** link to go to the HDFS service page.
3.  Click the Component Topology tab to see the list of components that run on all instances in the cluster.
4.  Click **Start** in the **Actions** column for the DataNode component that runs on the emr-worker-1 instance. Enter the commit record in the dialog box and click **OK**. Refresh the page after 10 seconds. The value in the **Components Status** column for DataNode switches from **STOPPED** to **STARTED**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17993/154752350813238_en-US.png)

5.  After the component is started, click **Restart** in the **Actions** column. Enter the commit record in the dialog box and click **OK**.

    Refresh the page after 40 seconds. The value in the Components Status column for DataNode switches from **STOPPED** to **STARTED**.

6.  Click **Stop** in the **Actions** column. Enter the commit record in the dialog box and click **OK**.

    Refresh the page after 10 seconds. The value of the **Components Status** column for DataNode switches from **STARTED** to **STOPPED**.


## Perform bulk operations on components using the Web UI {#section_ggn_m44_hfb .section}

You can perform bulk operations on components for multiple ECS instances instead of a single specified ECS instance. The HDFS service as an example. Follow these steps to restart the DataNode components for all instances.

1.  On the Cluster list page, click **Manage** in the **Actions** column for the cluster to perform operations on.
2.  On the Clusters and Services page, click **Actions** for **HDFS** in the Services list.
3.  Select **RESTART DataNode** from the Actions drop-down list. Enter the commit record in the dialog box and click **OK**.

    Click **HDFS** and click the Component Topology tab. View the status of each process on the Component Topology tab page.

    **Note:** Console reports an error if you restart nodes manually after performing a rolling restart of the cluster.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17993/154752350813240_en-US.png)


## Start and stop a component using CLI {#section_aps_pr4_hfb .section}

-   YARN

    Account: hadoop

    -   ResourceManager \(a master component\)

        ```
        //Starts a component.
        /usr/lib/hadoop-current/sbin/yarn-daemon.sh start resourcemanager
        //Stops a component.
        /usr/lib/hadoop-current/sbin/yarn-daemon.sh stop resourcemanager
        ```

    -   NodeManager \(a core component\)

        ```
        //Starts a component.
        /usr/lib/hadoop-current/sbin/yarn-daemon.sh start nodemanager
        //Stops a component.
        /usr/lib/hadoop-current/sbin/yarn-daemon.sh stop nodemanager
        ```

    -   JobHistoryServer \(a master component\)

        ```
        //Starts a component.
        /usr/lib/hadoop-current/sbin/mr-jobhistory-daemon.sh start historyserver
        //Stops a component.
        /usr/lib/hadoop-current/sbin/mr-jobhistory-daemon.sh stop historyserver
        ```

    -   JobHistoryServer \(a master component\)

        ```
        //Starts a component.
        /usr/lib/hadoop-current/sbin/mr-jobhistory-daemon.sh start historyserver
        //Stops a component.
        /usr/lib/hadoop-current/sbin/mr-jobhistory-daemon.sh stop historyserver
        ```

    -   WebProxyServer \(a master component\)

        ```
        //Starts a component.
        /usr/lib/hadoop-current/sbin/yarn-daemon.sh start proxyserver
        //Stops a component.
        /usr/lib/hadoop-current/sbin/yarn-daemon.sh stop proxyserver
        ```

-   HDFS

    Account: hdfs

    -   NameNode \(a master component\)

        ```
        //Starts a component.
        /usr/lib/hadoop-current/sbin/hadoop-daemon.sh start namenode
        //Stops a component.
        /usr/lib/hadoop-current/sbin/hadoop-daemon.sh stop namenode
        ```

    -   DataNode \(a core component\)

        ```
        //Starts a component.
        /usr/lib/hadoop-current/sbin/hadoop-daemon.sh start datanode
        //Stops a component.
        /usr/lib/hadoop-current/sbin/hadoop-daemon.sh stop datanode
        ```

-   Hive

    Account: hadoop

    -   MetaStore \(a master component\)

        ```
        //Starts a component. You can set the HADOOP_HEAPSIZE environment variable to a greater value for a larger maximum memory.
        HADOOP_HEAPSIZE=512 /usr/lib/hive-current/bin/hive --service metastore >/var/log/hive/metastore.log 2>&1 &
        ```

    -   HiveServer2 \(a master component\)

        ```
        //Starts a component.
        HADOOP_HEAPSIZE=512 /usr/lib/hive-current/bin/hive --service hiveserver2 >/var/log/hive/hiveserver2.log 2>&1 &
        ```

-   HBase

    Operational account: hdfs

    Note: The following example is based on the HBase service. An error occurs when you start a component without corresponding configurations.

    -   HMaster \(a master component\)

        ```
        //Starts a component.
        /usr/lib/hbase-current/bin/hbase-daemon.sh start master
        //Restarts a component.
        /usr/lib/hbase-current/bin/hbase-daemon.sh restart master
        //Stops a component.
        /usr/lib/hbase-current/bin/hbase-daemon.sh stop master
        ```

    -   HRegionServer \(a core component\)

        ```
        //Starts a component.
        /usr/lib/hbase-current/bin/hbase-daemon.sh start regionserver
        //Restarts a component.
        /usr/lib/hbase-current/bin/hbase-daemon.sh restart regionserver
        //Stops a component.
        /usr/lib/hbase-current/bin/hbase-daemon.sh stop regionserver
        ```

    -   ThriftServer \(a master component\)

        ```
        //Starts a component.
        /usr/lib/hbase-current/bin/hbase-daemon.sh start thrift -p 9099 >/var/log/hive/thriftserver.log 2>&1 &
        //Stops a component.
        /usr/lib/hbase-current/bin/hbase-daemon.sh stop thrift
        ```

-   Hue

    Account: hadoop

    ```
    //Starts a component.
    su -l root -c "${HUE_HOME}/build/env/bin/supervisor >/dev/null 2>&1 &"
    //Stops a component.
    ps aux | grep hue     //Lists information about the currently running Hue processes.
    kill -9 huepid        //Kills the Hue process by its PID.
    ```

-   Zeppelin

    Account: hadoop

    ```
    //Starts a component. You can set the HADOOP_HEAPSIZE environment variable to a greater value for a larger maximum memory.
    su -l root -c "ZEPPELIN_MEM=\"-Xmx512m -Xms512m\" ${ZEPPELIN_HOME}/bin/zeppelin-daemon.sh start"
    //Stops a component.
    su -l root -c "${ZEPPELIN_HOME}/bin/zeppelin-daemon.sh stop"
    ```

-   Presto

    Account: hadoop

    -   PrestoServer \(a master component\)

        ```
        //Starts a component.
        /usr/lib/presto-current/bin/launcher --config=/usr/lib/presto-current/etc/coordinator-config.properties start
        //Stops a component.
        /usr/lib/presto-current/bin/launcher --config=/usr/lib/presto-current/etc/coordinator-config.properties stop
        ```

    -   \(a core component\)

        ```
        //Starts a component.
        /usr/lib/presto-current/bin/launcher --config=/usr/lib/presto-current/etc/worker-config.properties start
        //Stops a component.
        /usr/lib/presto-current/bin/launcher --config=/usr/lib/presto-current/etc/worker-config.properties stop
        ```


## Perform bulk operations on components using CLI {#section_nvb_zs4_hfb .section}

Write script commands to perform bulk operations for all worker \(core\) nodes. In an EMR cluster, all worker nodes that run in the hadoop account and the hdfs account are accessible from the master node using SSH.

For example, you can run the following commands to stop the NodeManager components for all worker nodes. Assume that the number of worker nodes is 10.

```
for i in `seq 1 10`;do ssh emr-worker-$i /usr/lib/hadoop-current/sbin/yarn-daemon.sh stop nodemanager;done
```

