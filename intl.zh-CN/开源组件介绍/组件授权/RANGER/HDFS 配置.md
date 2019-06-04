# HDFS 配置 {#concept_oj5_b3d_bfb .concept}

前面简介中介绍了 E-MapReduce 中创建启动 Ranger 服务的集群，以及一些准备工作，本节介绍 HDFS 集成 Ranger 的一些步骤流程。

## HDFS 集成 Ranger {#section_qf5_d3d_bfb .section}

-   Enable HDFS Plugin
    1.  在集群管理页面，在需要操作的集群后单击**管理**
    2.  在服务列表中单击 **Ranger** 进入 Ranger 配置页面
    3.  在 Ranger 配置页面，单击右侧的**操作**，下拉菜单选择**Enable HDFS PLUGIN**。

        ![Enable HDFS PLUGIN](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/155963244111456_zh-CN.png)

    4.  在弹出框输入执行 Commit 记录，然后单击**确定**。

        单击右上角**查看操作历史**查看任务进度，等待任务完成。

        ![查看操作历史](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/155963244111459_zh-CN.png)

-   重启 NameNode

    上述任务完成后，需要重启 NameNode 才生效。

    1.  在 Ranger 配置页面中，单击左上角 Ranger 后的倒三角，从 Ranger 切换到 HDFS。
    2.  单击右上角**操作**，下拉菜单中选择 **RESTART NameNode**。
    3.  在弹出框输入执行 Commit 记录，然后单击**确定**。
    4.  单击右上角的**查看操作历史**查看任务进度，等待重启任务完成。

        ![查看操作历史](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/155963244211463_zh-CN.png)

-   Ranger UI 添加 HDFS service

    参见[Ranger 简介](intl.zh-CN/开源组件介绍/组件授权/RANGER/Ranger 简介.md#)进入Ranger UI。

    在 Ranger UI 页面添加 HDFS Service。

    ![Ranger UI](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/155963244211479_zh-CN.png)

    -   标准集群

        如果您创建的是标准集群（可到**详情**页面查看安全模式），请参考下图进行配置：

        ![标准集群](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/155963244211480_zh-CN.png)

    -   高安全集群

        如果您创建的是高安全集群（可到**详情**页面查看安全模式），请参考下图进行配置：

        ![高安全集群](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/155963244211481_zh-CN.png)

        ![高安全集群](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/155963244221757_zh-CN.png)


## 权限配置示例 {#section_osm_th3_bfb .section}

上面一节中已经将 Ranger 集成到 HDFS，现在可以进行相关的权限设置。例如给用户 test 授予 /user/foo路径的写/执行权限：

![权限配置示例](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/155963244211482_zh-CN.png)

单击上图中的 **emr-hdfs** 进入配置页面，配置相关权限。

![配置相关权限](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/155963244211483_zh-CN.png)

按照上述步骤设置添加一个 Policy 后，就实现了对 test 的授权，然后用户 test 就可以对 /user/foo 的 HDFS 路径进行访问了。

**说明：** 添加 Policy 1 分钟左右，HDFS 才会生效。

