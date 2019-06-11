# Hive 配置 {#concept_ej3_jn3_bfb .concept}

[Ranger 简介](cn.zh-CN/开源组件介绍/组件授权/RANGER/Ranger 简介.md#)中介绍了 E-MapReduce 中创建和启动 Ranger 服务的集群，以及一些准备工作，本节介绍 Hive 集成 Ranger 的步骤流程。

## Hive 集成 Ranger {#section_l5r_443_bfb .section}

-   Hive 访问模型

    用户可以通过三种方式访问 Hive 数据，包括 HiveServer 2、Hive Client 和 HDFS。

    -   HiveServer 2 方式
        -   场景： 用户只能通过 HiveServer2 访问 Hive 数据，不能通过 Hive Client 和 HDFS 访问。
        -   方式：使用 beeline 客户端或者 JDBC 代码通过 HiveServer 2 去执行相关的 Hive 脚本。
        -   权限设置：

            Hive 官方自带的 [SQL Standards Based Authorization](cn.zh-CN/开源组件介绍/组件授权/Hive 授权.md#) 就是针对 HiveServer2 使用场景进行权限控制的。

            Ranger 中对 Hive 的表/列级别的权限控制也是针对 HiveServer2 的使用场景，如果用户还可以通过 Hive Client 或者 HDFS 访问 Hive 数据，仅仅对表/列层面做权限控制还不够，还需要下面两种方式的一些设置进一步控制权限。

    -   Hive Client 方式
        -   场景： 用户可以通过 Hive Client 访问 Hive 数据。
        -   方式： 使用 Hive Client 访问。
        -   权限设置：

            Hive Client 会请求 HiveMetaStore 进行一些 DDL 操作（如 Alter Table Add Columns等），也会通过提交 MapReduce 作业读取 HDFS 中的数据进行处理。

            Hive 官方自带的 [Storage Based Authorization](cn.zh-CN/开源组件介绍/组件授权/Hive 授权.md#) 就是针对 Hive Client 使用场景进行的权限控制，它会根据 SQL 中表的 HDFS 路径的读写权限来决定该用户是否可以进行相关的 DDL/DML 操作，如`ALTER TABLE test ADD COLUMNS(b STRING)`

            Ranger 中可以对 Hive 表的 HDFS 路径进行权限控制，加上 HiveMetaStore 配置 Storage Based Authorization，从而可以实现对 Hive Client 访问场景的权限控制。

            **说明：** Hive Client 场景的 DDL 操作权限实际也是通过底层 HDFS 权限控制，所以如果用户有 HDFS 权限，则也会有对应表的 DDL 操作权限（如 Drop Table/Alter Table 等）。

    -   HDFS 方式
        -   场景： 用户可以直接访问 HDFS。
        -   方式： HDFS 客户端/代码等。
        -   权限设置:

            用户可以直接访问 HDFS，则需要对 Hive 表的底层 HDFS 数据增加 HDFS 的权限控制。

            通过 Ranger 对 Hive 表底层的 HDFS 路径进行[权限控制](cn.zh-CN/开源组件介绍/组件授权/RANGER/Hive 配置.md#)。

-   Enable Hive Plugin

    1.  在集群配置页面，在你想操作的集群后的操作栏中单击**管理**。
    2.  在服务列表中单击 **Ranger** 进入 Ranger 配置界面。
    3.  在 Ranger 配置页面，单击右侧的**操作**下拉菜单，选择 **Enable Hive PLUGIN**，然后单击**确定**。

        ![Enable Hive PLUGIN](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/156021780311501_zh-CN.png)

    4.  在弹出框输入备注信息，然后单击**确定**。

        单击右上角**查看操作历史**查看任务进度，等待任务完成。

        ![查看操作历史](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/156021780411502_zh-CN.png)

    **说明：** Enable Hive Plugin 并重启 Hive 后，则 HiveServer2 场景 /HiveClient 场景（Storage Based Authorization）的都进行了相关的配置，HDFS 的权限可参见 [HDFS 配置](cn.zh-CN/开源组件介绍/组件授权/RANGER/HDFS 配置.md#)进行开启。

-   重启 Hive

    上述任务完成后，需要重启 Hive 才生效。

    1.  在 Ranger 配置页面中，单击左上角 Ranger 后的倒三角，从 Ranger 切换到 Hive。
    2.  单击右上角**操作**，从下拉菜单中选择 **RESTART All Components**，然后单击**确定**。
    3.  单击右上角的**查看操作历史**查看任务进度，等待重启任务完成。
-   Ranger UI添加Hive service

    参见[Ranger 简介](cn.zh-CN/开源组件介绍/组件授权/RANGER/Ranger 简介.md#)进入 Ranger UI。

    在 Ranger UI 页面添加 Hive Service。

    ![Ranger UI](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/156021780411506_zh-CN.png)

    ![添加Hive Service](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/156021780411507_zh-CN.png)

    -   填写说明

        上述截图中相关的固定填写内容如下：

        |名称|值|
        |--|--|
        |Service Name|emr-hive|
        |jdbc.driverClassName|org.apache.hive.jdbc.HiveDriver|

    -   相关的变量值:

        |名称|值|
        |--|--|
        |jdbc.url|         -   标准集群： jdbc:hive2://emr-header-1:10000/
        -   高安全集群：jdbc:hive2://$\{master1\_fullhost\}:10000/;principal=hive/$\{master1\_fullhost\}@EMR.$id.COM
 |
        |policy.download.auth.users|hadoop（非高安全集群）/hive（高安全集群）|

        $\{master1\_fullhost\} 为 master 1 的长域名，可登录 master 1 执行`hostname`命令获取，$\{master1\_fullhost\} 中的数字即为 $id 的值。


## 权限配置示例 {#section_yww_vv3_bfb .section}

上面一节中已经将 Ranger 集成到 Hive，现在可以进行相关的权限设置。例如给用户 foo 授予表 testdb.test 的 a 列 Select 权限。

![权限配置示例](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/156021780511509_zh-CN.png)

单击上图中的 **emr-hive** 进入配置页面，配置相关权限。

![配置相关权限](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17950/156021780511510_zh-CN.png)

按照上述步骤设置添加一个 Policy 后，就实现了对 foo 的授权，然后用户 foo 就可以对 testdb.test 的表进行访问了。

**说明：** 添加 Policy 1 分钟左右后，Hive 才会生效。

