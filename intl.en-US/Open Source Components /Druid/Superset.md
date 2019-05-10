# Superset {#concept_gnn_p5d_z2b .concept}

The Druid cluster integrates the Superset tool, which is integrated with Druid and supports a variety of relational databases. Because Druid supports SQL, you can access Druid through Superset in two ways: Druid's native query language or SQL.

Superset is installed in emr-header-1 by default and does not support high availability at present. Before you use this tool, make sure that your host can access emr-header-1. You can connect to the host by establishing the [SSH tunnel](reseller.en-US/Quick Start/Connect to a cluster using SSH.md#).

1.  Log on to the Superset

    Enter http://emr-header-1:18088 in your browser to go to the Superset logon page. The default username is admin and the default password is admin. When you log on for the first time, we strongly recommend changing your password.

    ![Log on to the Superset](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/155746881410869_en-US.png)

2.  Add a Druid cluster

    The English interface is displayed by default. You can select the appropriate language by clicking the flag icon in the upper-right corner. In the menu bar along the top, select **Data Source** \> **Druid Cluster** to add a Druid cluster.

    ![Add a Druid cluster](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/155746881410870_en-US.png)

    Configure the addresses of the coordinator and broker. The default port number in E-MapReduce is the corresponding open source port number with "1" added in front. For example, if the open-source broker port number is 8082, the port number in E-MapReduce is 18082.

    ![Add Druid Cluster](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/155746881410871_en-US.png)

3.  Refresh or add a new data source

    After adding the Druid cluster, you can click **Data Source** \> **Scan** to add new data sources. The data sources on the Druid cluster loaded automatically.

    You can also customize a new data source by clicking **Sources** \> **Druid Datasources** on the interface. \(This operation is equivalent to writing a JSON file for data source ingestion.\)

    ![Druid Datasources](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/155746881410872_en-US.png)

    Enter the necessary information for custom data sources, and save it.

    ![Add Druid Datasource](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/155746881410873_en-US.png)

    Click the second of the three small icons on the left side to edit the data source. Enter the appropriate information, such as dimensions and metrics.

    ![Edit Druid Datasource](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/155746881410874_en-US.png)

4.  Query Druid

    After the data source has been added successfully, click it to go to the details page.

    ![Query Druid](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/155746881410875_en-US.png)

5.  \(Optional\) Use Druid as a database

    Superset provides SQLAlchemy to support a wide variety of databases with various dialects, as shown in the following figure.

    ![Supported databases](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/155746881510876_en-US.png)

    Superset also supports accessing Druid in this way. The corresponding SQLAlchemy URI of Druid is druid://emr-header-1:18082/druid/v2/sql. When you add Druid as a database, check the "Expose in SQL Lab" check box.

    ![Add Database](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/155746881510877_en-US.png)

    You can now use SQL to query in the SQL toolkit.


