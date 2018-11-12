# Superset {#concept_gnn_p5d_z2b .concept}

The Druid cluster integrates the Superset tool. Superset provides deep integration to Druid, and supports a variety of relational databases. Because Druid supports SQL, you can access Druid in two ways through Superset, that is, Druid native query language or SQL.

Superset is installed in emr-header-1 by default, and does not support high availability at present. Before you use this tool, make sure that your host can access emr-header-1. You can connect to the host by establishing the [SSH tunnel](intl.en-US/User Guide/Connect to clusters using SSH.md#).

1.  Log on to the Superset.

    Visit http://emr-header-1:18088 in the browser to enter the Superset logon page. The default username is admin, and the default password is admin. Change your password upon logon.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/154200957110869_en-US.png)

2.  Add a Druid cluster.

    The English interface is displayed by default. You can select the appropriate language by clicking the flag icon in the upper right corner. In the top menu bar, select **Data Source** \> **Druid Cluster** to add a Druid cluster.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/154200957210870_en-US.png)

    Configure the addresses of the coordinator and broker. The default port number in E-MapReduce is the corresponding open source port number with “1” added in front. For example, the open source broker port number is 8082, and the port number in E-MapReduce is port 18082.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/154200957210871_en-US.png)

3.  Refresh or add a new data source.

    After adding the Druid cluster, you can click **Data Source** \> **Scan** for new data sources. The data sources on the Druid cluster can be automatically loaded.

    You can also customize a new data source by clicking **Sources** \> **Druid Datasources** on the interface. \(The operation is equivalent to writing a JSON file for data source ingestion.\)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/154200957210872_en-US.png)

    You need to fill in necessary information for custom data source, and then save it.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/154200957210873_en-US.png)

    Click the second of the three small icons on the left side to edit the data source. Fill in the appropriate information, such as dimensions and metrics.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/154200957210874_en-US.png)

4.  Query Druid.

    After the data source has been added successfully, click the data source to go to the details page.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/154200957210875_en-US.png)

5.  \(Optional\) Use Druid as a database.

    Superset provides SQLAlchemy to support a wide variety of databases with various dialects, as shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/154200957210876_en-US.png)

    Superset also supports access to Druid in this way. The corresponding SQLAlchemy URI of Druid is “druid://emr-header-1:18082/druid/v2/sql”. When you add Druid as a database, check the Expose the database in the SQL toolkit check box.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/154200957210877_en-US.png)

    Then you can use SQL to query in the SQL toolkit:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/154200957210878_en-US.png)


