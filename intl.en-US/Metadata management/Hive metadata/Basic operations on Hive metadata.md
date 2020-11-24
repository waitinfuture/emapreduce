# Basic operations on Hive metadata

This topic describes basic operations on Hive metadata, such as creating a database, deleting a database, creating a table, and deleting a table.

A cluster is created and Type is set to **Unified Metabases**. For more information about how to create a cluster, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

## Create a database

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

3.  Click the **Metadata** tab.

4.  On the **Tables** page, click **Create Database** in the upper-right corner.

5.  In the **Create Database** dialog box, configure parameters.

    The EMR table management feature can be used only to create databases and tables based on OSS. Set **Data Source** to **OSS**. Database and table file locations must be in a directory under an OSS bucket. Set the location to a specific directory, instead of an OSS bucket.

6.  Click **OK**.

    You can click **Tasks** to view the results.

    -   If **Status** is **Successful**, the topic is added.
    -   If **Status** is **Failed**, you can click **View Details** in the **Action** column to identify the cause.

## Create a table

**Note:** You can create an external table or a partitioned table.

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

3.  Click the **Metadata** tab.

4.  On the **Tables** page, click a created metadatabase.

5.  Click **Create Table** in the upper-right corner.

6.  In the **Create Table** dialog box, configure the parameters described in the following table.

    |Parameter|Description|
    |---------|-----------|
    |**Table**|The name of the table.|
    |**Delimiter**|Select a delimiter or Custom from the **Delimiter** drop-down list.|
    |**External Table**|This parameter is not selected by default. To create an external table, perform the following steps:

    1.  Select the **External Table** check box. Then, click ![File path](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4465326061/p96723.png) to specify a file path.
    2.  Click **Add Column** and configure relevant parameters. |
    |**Enable Partition Mode**|Default value: **No**. To create a partitioned table, perform the following steps:

    1.  Set Enable Partition Mode to **Yes**.
    2.  Click **Add Partition Column** and configure relevant parameters. |

7.  Click **OK**.

    You can click **Tasks** to view the results.

    -   If **Status** is **Successful**, the topic is added.
    -   If **Status** is **Failed**, you can click **View Details** in the **Action** column to identify the cause.

## Delete a table

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

3.  Click the **Metadata** tab.

4.  On the **Tables** page, click a created metadatabase.

5.  Find the table you want to delete and click **Delete** in the **Actions** column.

6.  In the **Delete Table** message, click **OK**.

    You can click **Tasks** to view the results.

    -   If **Status** is **Successful**, the topic is added.
    -   If **Status** is **Failed**, you can click **View Details** in the **Action** column to identify the cause.

## Delete a database

**Note:** Before you delete a database, you must delete all tables stored in the database.

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

3.  Click the **Metadata** tab.

4.  On the **Tables** page, click a created metadatabase.

5.  Find the database you want to delete and click **Delete** in the **Actions** column.

6.  In the **Delete Database** message, click **OK**.

    You can click **Tasks** to view the results.

    -   If **Status** is **Successful**, the topic is added.
    -   If **Status** is **Failed**, you can click **View Details** in the **Action** column to identify the cause.

