# Superset {#concept_gnn_p5d_z2b .concept}

E-MapReduce Druid 集群集成了 Superset 工具。Superset 对 E-MapReduce Druid 做了深度集成，同时也支持多种关系型数据库。由于 E-MapReduce Druid 也支持 SQL，所以可以通过 Superset 以两种方式访问 E-MapReduce Druid，即Apache Druid 原生查询语言或者 SQL。

Superset 默认安装在 emr-header-1 节点，目前还不支持 HA。在使用该工具前，确保您的主机能够正常访问 emr-header-1。您可以通过打 [SSH 隧道](intl.zh-CN/集群规划与配置/集群配置/SSH 登录集群.md#) 的方式连接到主机。

1.  登录 Superset

    在浏览器地址栏中输入 http://emr-header-1:18088 后回车，打开 Superset 登录界面，默认用户名/密码为 admin/admin，请您登录后及时修改密码。

    ![登录Superset](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/156566037810869_zh-CN.png)

2.  添加 E-MapReduce Druid 集群

    登录后默认为英文界面，可点击右上角的国旗图标选择合适的语言。接下来在上方菜单栏中依次选择**数据源** \> **Druid 集群**来添加一个 E-MapReduce Druid 集群。

    ![添加 Druid 集群](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/156566037810870_zh-CN.png)

    配置好协调机（Coordinator）和代理机（Broker）的地址，注意 E-MapReduce 中默认端口均为相应的开源端口前加数字 1，例如开源 Broker 端口为 8082，E-MapReduce 中为 18082。

    ![添加Druid集群](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/156566037810871_zh-CN.png)

3.  刷新或者添加新数据源

    添加好 E-MapReduce Druid 集群之后，您可以单击**数据源** \> **扫描新的数据源**，这时 E-MapReduce Druid 集群上的数据源（datasource）就可以自动被加载进来。

    您也可以在界面上单击**数据源** \> **Druid 数据源**自定义新的数据源（其操作等同于写一个 data source ingestion 的 json 文件），步骤如下：

    ![数据源](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/156566037910872_zh-CN.png)

    1.  自定义数据源时需要填写必要的信息，然后保存。

        ![添加Druid数据源](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/156566037910873_zh-CN.png)

    2.  保存之后点击左侧三个小图标中的第二个，编辑该数据源，填写相应的维度列与指标列等信息。

        ![编辑Druid数据源](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/156566037910874_zh-CN.png)

4.  查询 E-MapReduce Druid

    数据源添加成功后，单击数据源名称，进入查询页面进行查询。

    ![查询 Druid](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/156566038010875_zh-CN.png)

5.  （可选）将 E-MapReduce Druid 作为E-MapReduce Druid数据库使用

    Superset 提供了 SQLAlchemy 以多种方言支持各种各样的数据库，其支持的数据库类型如下表所示。

    ![支持的数据库类型](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/156566038010876_zh-CN.png)

    Superset 亦支持该方式访问 E-MapReduce Druid，E-MapReduce Druid 对应的 SQLAlchemy URI 为 druid://emr-header-1:18082/druid/v2/sql，如下图所示，将 E-MapReduce Druid 作为一个数据库添加：

    ![添加数据库](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17910/156566038010877_zh-CN.png)

    接下来就可以在 SQL 工具箱里用 SQL 进行查询了。


