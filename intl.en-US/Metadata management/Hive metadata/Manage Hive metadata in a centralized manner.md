# Manage Hive metadata in a centralized manner

In E-MapReduce \(EMR\) versions earlier than V2.4.0, local MySQL databases are used to store the Hive metadata of clusters. In EMR V2.4.0 and later versions, high-reliability Hive metadatabases are used for centralized metadata management.

Type is set to **Unified Metabases** when you create a cluster. For more information, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

A metadatabase can be accessed only by using a public IP address. Ensure that you have configured a public IP address for your cluster. Do not change the public IP address. Otherwise, the database whitelist becomes invalid.

You cannot manage the metadata of a local metadatabase in the console. However, you can use the Hue tool on a cluster to manage the metadata.

If you require only a small storage capacity, you can use ApsaraDB RDS in the background of EMR to manage metadata in a centralized manner. If you require a large storage capacity, we recommend that you create an ApsaraDB RDS instance to manage metadata in a centralized manner. Default limits on your created ApsaraDB RDS instance:

-   Total capacity: 200 MiB
-   Maximum number of queries per hour: 720,000
-   Maximum number of updates per hour: 144,000

## Overview

![Hive metadatabases](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/2311549951/p11067.png)

Centralized metadata management has the following benefits:

-   Persistent metadata storage

    In earlier versions, metadata is stored in MySQL databases that are deployed on clusters and is deleted when the clusters are released. You can release a pay-as-you-go cluster if it is no longer needed. To retain the metadata, you need to log on to a cluster and export the metadata manually.

    After centralized metadata management is enabled, the metadata of released clusters is not cleared. Before you delete data in OSS or in the HDFS of a cluster or you release a cluster, make sure that the metadata is deleted. That means the tables and database that store the data are also deleted. This prevents a buildup of dirty metadata in the database.

-   Separation of computing and storage

    EMR can store data in Alibaba Cloud OSS, which significantly reduces the usage costs for large volumes of data. EMR clusters are mainly used as computing resources and can be released if they are no longer needed. You do not need to migrate metadata before cluster release because data is stored in OSS.

-   Data sharing

    If all data is stored in OSS, all clusters can access data without the need to migrate or restructure metadata. This way, EMR clusters that process different services can directly share data.


## Create a cluster that uses unified metadata

You can use one of the following methods to create a cluster that uses unified metadata:

-   Use the EMR console

    When you create a cluster, set Type to **Unified Metabases** in the **Basic Settings** step. For information about how to create a cluster, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

-   Call the CreateClusterV2 API operation

    See the description of the [CreateClusterV2](/intl.en-US/API Reference/Cluster/CreateClusterV2.md) operation.

    **Note:** Set the value of the useLocalMetaDb parameter to false.


## Table management

For more information, see [t1881605.md\#]().

## View metadata information

1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

3.  Click the **Metadata** tab.

4.  In the left-side navigation pane, click **Metabase Information**.

    On the Metabase Information page, you can view the usage and limits of the current ApsaraDB RDS instance. Submit a ticket if you need to modify the information. For information about how to submit a ticket, see [submit a ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).


