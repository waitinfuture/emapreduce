# Use Hive jobs to process Tablestore data

This topic describes how to run Hive jobs to process Tablestore data in the E-MapReduce \(EMR\) console.

A Tablestore instance is deployed in the same VPC as an EMR cluster.

## Step 1: Create a Hadoop cluster

1.  Log on to the [Alibaba Cloud EMR console](https://emr.console.aliyun.com).

2.  Create a Hadoop cluster. For more information, see [Create a cluster](/intl.en-US/Quick Start/Create a cluster.md).

    ![Create a cluster](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4307136061/p52748.png)


## Step 2: Obtain required JAR packages and upload them to the Hadoop cluster

1.  Obtain the JAR packages described in the following table.

    |JAR package|How to obtain|
    |-----------|-------------|
    |emr-tablestore-X.X.X.jar|Download the [emr-tablestore](https://mvnrepository.com/artifact/com.aliyun.emr/emr-tablestore) package from the Maven repository.|
    |hadoop-lzo-X.X.X-SNAPSHOT.jar|Log on to the emr-header-1 node of the Hadoop cluster. Obtain the JAR package from the /opt/apps/ecm/service/hadoop/x.x.x-x.x.x/package/hadoop-x.x.x-x.x.x/lib/ directory.|
    |hive-exec-X.X.X.jar|Log on to the emr-header-1 node of the Hadoop cluster. Obtain the JAR package from the /opt/apps/ecm/service/hive/x.x.x-x.x.x/package/apache-hive-x.x.x-x.x.x-bin/lib/ directory.|
    |joda-time-X.X.X.jar|Log on to the emr-header-1 node of the Hadoop cluster. Obtain the JAR package from the /opt/apps/ecm/service/hive/x.x.x-x.x.x/package/apache-hive-x.x.x-x.x.x-bin/lib/ directory.|
    |tablestore-X.X.X-jar-with-dependencies.jar|Download the [tablestore](https://mvnrepository.com/artifact/com.aliyun.openservices/tablestore) package related to EMR SDK.|

    **Note:**

    -   X.X.X indicates the version number of a JAR package. x.x.x-x.x.x indicates the directory for storing a JAR package. The version number of a JAR package depends on the directory name. For example, if the directory is 3.1.1-1.1.6, the name of the JAR package is hive-exec-3.1.1.jar.

        ![jar](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/5084152751/p60923.png)

    -   For more information about how to log on to the emr-header-1 node of the Hadoop cluster, see [substep 2 of Step 2](#step_yxi_yke_s9h) to [substep 4 of Step 2](#step_7dt_7ua_jkn).
2.  Go to the **Cluster Management** page. Click the ID of the created Hadoop cluster.

3.  In the left-side navigation pane, click **Instances**. View the IP address of the emr-header-1 node in the Hadoop cluster.

4.  Open a new command window in the SSH client and log on to the emr-header-1 node of the Hadoop cluster.

5.  Upload all the JAR packages to a directory of the emr-header-1 node.

6.  Copy all the JAR packages to the /opt/apps/extra-jars/ directory of the emr-header-1 node.

7.  Run the scp command to copy all the JAR packages to the /tmp directory of each core node.

    ```
    su hadoop
    scp <file> emr-worker-1:/tmp
    ```

8.  Log on to a core node and copy all the JAR packages to the /opt/apps/extra-jars/ directory.

    **Note:** This operation is required on all the core nodes.


## Step 3: Restart the Hive service

1.  Return to the [Alibaba Cloud EMR console](https://emr.console.aliyun.com).

2.  Go to the **Cluster Management** page. Click the ID of the created Hadoop cluster.

3.  In the **Services** section of the Clusters and Services page, find the Hive service, click the ![more](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/6084152751/p60751.png) icon next to Hive, and select **Restart All Components**.

    ![restart](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/6084152751/p60752.png)


## Step 4: Create a table in Tablestore

1.  Create a table in Tablestore. For more information, see [Create tables](/intl.en-US/Quick Start/Create tables.md).

    The following figure shows the created table.

    ![table](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/6084152751/p60922.png)


## Step 5: Process Tablestore data

1.  Create a table.

    ```
    CREATE EXTERNAL TABLE pet(name STRING, owner STRING, species STRING, sex STRING, birth STRING, death STRING)
    STORED BY 'com.aliyun.openservices.tablestore.hive.TableStoreStorageHandler'
    WITH SERDEPROPERTIES(
        "tablestore.columns.mapping"="name,owner,species,sex,birth,death")
    TBLPROPERTIES (
        "tablestore.endpoint"="https://XXX.cn-beijing.ots-internal.aliyuncs.com",
        "tablestore.access_key_id"="LTAIbUa7OYIO****",
        "tablestore.access_key_secret"="u2yOdykvSNXiPChyoqbCoawZnt****",
        "tablestore.table.name"="pet");
    ```

    -   If `OK` appears in the command output, the table is created.

    -   If a failure occurs, check whether the configuration information is correct:

-   Yes: [submit a ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).
-   No: Check whether all the JAR packages are copied to the /opt/apps/extra-jars/ directory of each core node. If yes, submit a ticket. For more information, see [submit a ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex). If no, follow [substep 1 of Step 2](#step_8qa_b67_8ku) to copy all the JAR packages.
2.  Insert data to the table.

    ```
    INSERT INTO pet VALUES("Fluffy", "Harold", "cat", "f", "1993-02-04", null);
    ```

    If `OK` appears in the command output, the data is inserted to the table.

3.  Run the following command to query data:

    ```
    hive> SELECT * FROM pet;
    ```

    ```
    OK
    Fluffy  Harold  cat  f  1993-02-04  NULL
    Time taken: 0.23 seconds, Fetched: 3 row(s)
    ```

    If `OK` appears in the command output, the data is queried.


