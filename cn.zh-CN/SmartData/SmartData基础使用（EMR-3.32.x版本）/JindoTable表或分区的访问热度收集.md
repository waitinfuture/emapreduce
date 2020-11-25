# JindoTable表或分区的访问热度收集

您可以通过JindoTable表或分区的访问热度收集功能来区分冷热数据，从而节约整体的存储成本，提高缓存利用效率。

## 数据收集

JindoTable支持收集访问Hive表的记录，目前支持的引擎有Spark和Hive。收集的数据保存在集群SmartData服务的Namespace中。

数据收集是默认打开的。如果需要关闭，请参见[关闭数据收集](#section_h9z_rb5_yd6)。

## 数据查询

JindoTable提供了命令方式查询热度信息。

-   语法

    ```
    jindo table -accessStat <-d [days]> <-n [topNums]>
    ```

    `days`和`topNums`为正整数。当天数为1时，表示查询从本地时间当天0:00开始到现在的所有访问记录。

-   功能

    查询在指定时间范围内，访问最多表或分区的指定条数。

-   示例，查询近七天访问最多的表或分区的20条访问记录。

    ```
    jindo table -accessStat -d 7 -n 20
    ```


JindoTable使用详情，请参见[JindoTable使用说明](/cn.zh-CN/SmartData/SmartData基础使用（EMR-3.30.x版本）/JindoTable使用说明.md)。

## 关闭数据收集

1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

3.  单击上方的**集群管理**页签。

4.  在**集群管理**页面，单击相应集群所在行的**详情**。

5.  修改参数值。

    **说明：** 删除如下参数值中的部分内容。

    -   Hive服务：
        1.  在左侧导航栏单击**集群服务** \> **Hive**。
        2.  单击**配置**页签。
        3.  单击**hive-site**页签。
        4.  搜索参数hive.exec.post.hooks，删除参数值中的**com.aliyun.emr.table.hive.HivePostHook**。

            ![hive-site](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2045226061/p184659.png)

    -   Spark服务：
        1.  在左侧导航栏单击**集群服务** \> **Spark**。
        2.  单击**配置**页签。
        3.  单击**spark-defaults**页签。
        4.  搜索参数spark.sql.queryExecutionListeners，删除参数值中的**com.aliyun.emr.table.spark.SparkSQLQueryListener**。

            ![spark_default](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2045226061/p184658.png)

6.  保存配置。

    1.  单击右上角的**保存**。

    2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。

    3.  单击**确定**。

7.  重启服务。

    -   Hive服务：
        1.  单击右上角的**操作** \> **重启 HiveServer2**。
        2.  在**执行集群操作**对话框，设置相关参数。
        3.  单击**确定**。
        4.  在**确认**对话框中，单击**确定**。
    -   Spark服务：
        1.  单击右上角的**操作** \> **重启 ThriftServer**。
        2.  在**执行集群操作**对话框，设置相关参数。
        3.  单击**确定**。
        4.  在**确认**对话框中，单击**确定**。

