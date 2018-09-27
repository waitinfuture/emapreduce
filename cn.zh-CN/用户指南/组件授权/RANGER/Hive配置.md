# Hive配置 {#concept_ej3_jn3_bfb .concept}

## Hive集成Ranger {#section_l5r_443_bfb .section}

[Ranger简介](https://help.aliyun.com/document_detail/66410.html)中介绍了E-MapReduce中创建启动Ranger服务的集群，以及一些准备工作，本节介绍Hive集成Ranger的步骤流程。

-   Hive访问模型

    用户可以通过三种方式访问Hive数据，包括HiveServer2、Hive Client和HDFS。

    -   HiveServer2方式
        -   场景: 用户只能通过HiveServer2访问Hive数据，不能通过Hive Client和HDFS访问。
        -   方式: 使用beeline客户端或者JDBC代码通过HiveServer2去执行相关的Hive脚本。
        -   权限设置:

            Hive官方自带的\([SQL Standards Based Authorization](https://help.aliyun.com/document_detail/62704.html?spm=a2c4g.11174283.6.640.ZHgqsb)\)就是针对HiveServer2使用场景进行权限控制的。

            Ranger中对Hive的表/列级别的权限控制也是针对HiveServer2的使用场景，如果用户还可以通过Hive Client或者HDFS访问Hive数据，仅仅对表/列层面做权限控制还不够，还需要下面两种方式的一些设置进一步控制权限。

    -   Hive Client方式
        -   场景: 用户可以通过Hive Client访问Hive数据。
        -   方式: 使用Hive Client访问。
        -   **权限设置:**

            Hive Client会请求HiveMetaStore进行一些DDL操作\(如Alter Table Add Columns等\)，也会通过提交MapReduce作业读取HDFS中的数据进行处理。

            Hive官方自带的\([Storage Based Authorization](https://help.aliyun.com/document_detail/62704.html?spm=a2c4g.11174283.6.640.ZHgqsb)\)就是针对Hive Client使用场景进行的权限控制，它会根据SQL中表的HDFS路径的读写权限来决定该用户是否可以进行相关的DDL/DML操作，如`ALTER TABLE test ADD COLUMNS(b STRING)`

            Ranger中可以对Hive表的HDFS路径进行权限控制，加上HiveMetaStore配置Storage Based Authorization，从而可以实现对Hive Client访问场景的权限控制。

            **说明：** Hive Client场景的DDL操作权限实际也是通过底层HDFS权限控制，所以如果用户有HDFS权限，则也会有对应表的DDL操作权限\(如Drop Table/Alter Table等\)。

    -   HDFS方式
        -   场景: 用户可以直接访问HDFS。
        -   方式: HDFS客户端/代码等。
        -   权限设置:

            用户可以直接访问HDFS，则需要对Hive表的底层HDFS数据增加HDFS的权限控制。

            通过Ranger对Hive表底层的HDFS路径进行[权限控制](https://help.aliyun.com/document_detail/66411.html)。

-   Enable Hive Plugin

    1.  在集群配置页面，在你想操作的集群后的操作栏中单击**管理**。
    2.  在服务列表中单击**Hive**进入Hive配置界面。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/153803772211501_zh-CN.png)

    3.  在Ranger配置页面，单击右侧的**操作**下拉菜单，选择**Enable Hive PLUGIN**，然后单击**确定**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/153803772211501_zh-CN.png)

    4.  在弹出框输入备注信息，然后单击**确定**。

        单击右上角**查看操作历史**查看任务进度，等待任务完成。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/153803772211502_zh-CN.png)

    **说明：** Enable Hive Plugin并重启Hive后，则HiveServer2场景/HiveClient场景\(Storage Based Authorization\)的都进行了相关的配置，HDFS的权限可参见[HDFS配置](https://help.aliyun.com/document_detail/66411.html?spm=a2c4g.11186623.2.10.63f75952TKxSqc)进行开启。

-   重启Hive

    上述任务完成后，需要重启Hive才生效。

    1.  在Ranger配置页面中，单击左上角Ranger后的倒三角，从Ranger切换到Hive。
    2.  单击右上角**操作**，从下拉菜单中选择**RESTART All Components**，然后单击**确定**。
    3.  单击右上角的**查看操作历史**查看任务进度，等待重启任务完成。
-   Ranger UI添加Hive service

    参见[Ranger简介](https://help.aliyun.com/document_detail/66410.html?spm=a2c4g.11186623.2.11.63f75952TKxSqc)的介绍进入Ranger UI。

    在Ranger UI页面添加Hive Service:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/153803772211506_zh-CN.png)

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/153803772211507_zh-CN.png)

    -   填写说明

        上述截图中相关的固定填写内容如下:

        |名称|值|
        |--|--|
        |Service Name|emr-hive|
        |jdbc.driverClassName|org.apache.hive.jdbc.HiveDriver|

    -   相关的变量值:

        |名称|值|
        |--|--|
        |jdbc.url|标准集群: jdbc:hive2://emr-header-1:10000/ 高安全集群:jdbc:hive2://$\{master1\_fullhost\}:10000/;principal=hive/$\{master1\_fullhost\}@EMR.$id.COM|
        |policy.download.auth.users|hadoop\(非高安全集群\)/hive\(高安全集群\)|

        $\{master1\_fullhost\} 为master1的长域名，可登录master1执行`hostname`命令获取，$\{master1\_fullhost\}中的数字即为$id的值。


## 权限配置示例 {#section_yww_vv3_bfb .section}

上面一节中已经将Ranger集成到Hive，现在可以进行相关的权限设置。例如给用户foo授予表testdb.test的a列Select权限：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/153803772311509_zh-CN.png)

单击上图中的**emr-hive**进入配置页面，配置相关权限。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/153803772311510_zh-CN.png)

按照上述步骤设置添加一个Policy后，就实现了对foo的授权，然后用户foo就可以对testdb.test的表进行访问了。

**说明：** Policy被添加1分钟左右后，Hive才会生效。

