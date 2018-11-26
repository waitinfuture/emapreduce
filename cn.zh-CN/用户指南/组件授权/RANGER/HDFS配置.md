# HDFS配置 {#concept_oj5_b3d_bfb .concept}

前面简介中介绍了E-MapReduce中创建启动Ranger服务的集群，以及一些准备工作，本节介绍HDFS集成Ranger的一些步骤流程。

## HDFS集成Ranger {#section_qf5_d3d_bfb .section}

-   Enable HDFS Plugin
    1.  在集群管理页面，在需要操作的集群后单击**管理**
    2.  在服务列表中单击**Ranger**进入Ranger配置页面
    3.  在Ranger配置页面，单击右侧的**操作**下拉菜单，选择**Enable HDFS PLUGIN**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154294488411456_zh-CN.png)

    4.  在弹出框输入执行Commit记录，然后单击**确定**。

        单击右上角**查看操作历史**查看任务进度，等待任务完成。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154294488411459_zh-CN.png)

-   重启NameNode

    上述任务完成后，需要重启NameNode才生效。

    1.  在Ranger配置页面中，单击左上角Ranger后的倒三角，从Ranger切换到HDFS。
    2.  单击右上角**操作**，从下拉菜单中选择**RESTART NameNode**。
    3.  在弹出框输入执行Commit记录，然后单击**确定**。
    4.  单击右上角的**查看操作历史**查看任务进度，等待重启任务完成。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154294488411463_zh-CN.png)

-   Ranger UI添加HDFS service

    参见[Ranger简介](intl.zh-CN/用户指南/组件授权/RANGER/Ranger简介.md#)的介绍进入Ranger UI。

    在Ranger UI页面添加HDFS Service:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154294488511479_zh-CN.png)

    -   标准集群

        如果您创建的是标准集群\(可到**详情**页面查看安全模式\)，请参考下图进行配置：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154294488511480_zh-CN.png)

    -   高安全集群

        如果您创建的是高安全集群\(可到**详情**页面查看安全模式\)，请参考下图进行配置：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154294488511481_zh-CN.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154294488521757_zh-CN.png)


## 权限配置示例 {#section_osm_th3_bfb .section}

上面一节中已经将Ranger集成到HDFS，现在可以进行相关的权限设置。例如给用户test授予/user/foo路径的写/执行权限：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154294488511482_zh-CN.png)

单击上图中的**emr-hdfs**进入配置页面，配置相关权限。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/154294488511483_zh-CN.png)

按照上述步骤设置添加一个Policy后，就实现了对test的授权，然后用户test就可以对/user/foo的HDFS路径进行访问了。

**说明：** Policy被添加1分钟左右后，HDFS才会生效。

