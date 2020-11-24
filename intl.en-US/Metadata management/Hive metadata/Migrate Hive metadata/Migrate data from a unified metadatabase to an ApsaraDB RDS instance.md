# Migrate data from a unified metadatabase to an ApsaraDB RDS instance

To ensure stable running of large-scale Hive metadata services, you can migrate data from a unified metadatabase to your created ApsaraDB RDS instance.

An ApsaraDB RDS instance is purchased. For more information, see [Create an ApsaraDB RDS for MySQL instance](/intl.en-US/RDS MySQL Database/Quick start/Create an ApsaraDB RDS for MySQL instance.md).

**Note:**

-   We recommend that you select 5.7 from the **MySQL** drop-down list for **Database Engine** and set **Edition** to **High-availability** when you create an ApsaraDB RDS instance.
-   The ApsaraDB RDS for MySQL instance must be in the same security group as instances that are in an E-MapReduce \(EMR\) cluster. This way, ApsaraDB RDS can communicate with EMR over the internal network.

## Procedure

1.  Create a database in the ApsaraDB RDS instance and name it **hivemeta**. Create a user and grant the user the read and write permissions on **hivemeta**. For more information, see [Configure an independent ApsaraDB RDS instance](/intl.en-US/Metadata management/Hive metadata/Configure an independent ApsaraDB RDS instance.md).

2.  Export data from a unified metadatabase \(the table schema not included\).

    1.  To ensure data consistency and avoid the changes of metadata during the export, stop the Hive metastore service on the Hive service page. For more information, see [Stop the Hive metastore service](#section_7ea_anx_wx9).

    2.  On the Hive service page, click the **Configure** tab.

    3.  On the Configure tab, search for values of **javax.jdo.option.ConnectionUserName**, **javax.jdo.option.ConnectionPassword**, and **javax.jdo.option.ConnectionURL**.

        **Note:**

        -   For clusters of earlier versions, search for the values in the $HIVE\_CONF\_DIR/hive-site.xml file.
        -   **javax.jdo.option.ConnectionUserName** specifies the database username. **javax.jdo.option.ConnectionPassword** specifies the database password. **javax.jdo.option.ConnectionURL** specifies the database access URL and database name.
    4.  Log on to the master node of the EMR cluster and run the following command:

        ```
        mysqldump -t DATABASENAME -h HOST -P PORT -u USERNAME -p PASSWORD > /tmp/metastore.sql
        ```

        **Note:** Enter the value of javax.jdo.option.ConnectionPassword as the password.

3.  In the /usr/local/emr/emr-agent/run/meta\_db\_info.json file that is stored in the master node of the cluster, set **use\_local\_meta\_db** to **false**, and configure the access URL, username, and password of the ApsaraDB RDS instance for the metadatabase.

    **Note:**

    -   If the cluster does not have this file, skip this step.
    -   If an HA cluster is deployed, configure the information for all the master nodes.
4.  On the Hive service configuration page, configure the access URL, username, and password of the ApsaraDB RDS instance for the metadatabase.

    For clusters of earlier versions, modify the required information in the $HIVE\_CONF\_DIR/hive-site.xml file.

5.  On the master node, configure the access URL, username, and password of the ApsaraDB RDS instance for the metadatabase in the hive-site.xml file, and initialize the schema.

    -   For Hive V2.3.5:

        Log on to the master node by using SSH and perform the following steps to initialize the schema:

        1.  Enter the specified directory.

            ```
            cd /usr/lib/hive-current/scripts/metastore/upgrade/mysql/
            ```

        2.  Log on to the ApsaraDB RDS for MySQL database.

            ```
            mysql -h {Private or public IP address of the ApsaraDB RDS for MySQL database} -u{Username to log on to the ApsaraDB RDS for MySQL database} -p{Password to log on to the ApsaraDB RDS for MySQL database}
            ```

        3.  Run the following command on the command line of the ApsaraDB RDS for MySQL database:

            ```
            use {Name of the ApsaraDB RDS for MySQL database};
            source /usr/lib/hive-current/scripts/metastore/upgrade/mysql/hive-schema-2.3.0.mysql.sql;
            ```

        **Note:** Set `{Name of the ApsaraDB RDS for MySQL database}` to the name of the database created in [Step 1](#step_tsu_445_5fr), such as hivemeta.

    -   For Hive of another version:

        Log on to the master node by using SSH and run the following command to initialize the schema:

        ```
        cd /usr/lib/hive-current/bin
        ./schematool -initSchema -dbType mysql
        ```

6.  Import the exported metadata to the ApsaraDB RDS instance. Access the ApsaraDB RDS for MySQL database from the command line.

    ```
    mysql -h {URL of the ApsaraDB RDS for MySQL database} -u {Username to log on to the ApsaraDB RDS for MySQL database} -p
    ```

    On the command line, data can be imported by running `source /tmp/metastore.sql`.

7.  [Restart all Hive components](#section_ncd_1g2_jsm).

    Run the `hive cli` command on the master node to check data consistency.


## Access the Hive service configuration page

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  Click the **Cluster Management** tab.

3.  On the **Cluster Management** page, find your cluster and click **Details** in the Actions column.

4.  In the left-side navigation pane of the Cluster Overview page, choose **Cluster Service** \> **Hive**.


## Stop the Hive metastore service

1.  [Access the Hive service configuration page](#section_vo6_t1q_3st).

2.  In the **Components** section, click **Stop** in the Actions column that corresponds to **Hive MetaStore**.

    1.  In the **Cluster Activities** dialog box, specify **Target Nodes**, **Rolling Execute**, **Actions on Failures**, and **Description**.

    2.  Click **OK**.


## Restart all Hive components

1.  [Access the Hive service configuration page](#section_vo6_t1q_3st).

2.  Click the **Configure** tab.

3.  Choose **Actions** \> **Restart All Components** in the upper-right corner.

    1.  In the **Cluster Activities** dialog box, specify **Target Nodes**, **Rolling Execute**, **Actions on Failures**, and **Description**.

    2.  Click **OK**.


