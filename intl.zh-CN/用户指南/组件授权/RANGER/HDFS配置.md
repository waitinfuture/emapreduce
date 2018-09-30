# HDFS配置 {#concept_oj5_b3d_bfb .concept}

## HDFS集成Ranger {#section_qf5_d3d_bfb .section}

前面简介中介绍了E-MapReduce中创建启动Ranger服务的集群，以及一些准备工作，本节介绍HDFS集成Ranger的一些步骤流程。

-   Enable HDFS Plugin
    1.  在集群配置页面，在你想操作的集群后的操作栏中单击**管理**
    2.  在服务列表中单击**Ranger**进入Ranger配置页面
    3.  在Ranger配置页面，单击右侧的**操作**下拉菜单，选择**Enable HDFS PLUGIN**，然后单击**确定**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/153829654211456_zh-CN.png)

    4.  在弹出框输入备注信息，然后单击**确定**。

        单击右上角**查看操作历史**查看任务进度，等待任务完成。

-   重启NameNode

    上述任务完成后，需要重启NameNode才生效。

    1.  在Ranger配置页面中，单击左上角Ranger后的倒三角，从Ranger切换到HDFS。
    2.  单击右上角**操作**，从下拉菜单中选择RESTART NameNode，然后单击确定。
    3.  单击右上角的**查看操作历史**查看任务进度，等待重启任务完成。
-   Ranger UI添加HDFS service

    参见[Ranger简介](https://help.aliyun.com/document_detail/66410.html?spm=a2c4g.11186623.2.7.25935f57vX43oY)的介绍进入Ranger UI。

    在Ranger UI页面添加HDFS Service:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/153829654211479_zh-CN.png)

    -   标准集群

        如果您创建的是标准集群\(可到**集群详情**页面查看安全模式\)，请参考下图进行配置：

    -   高安全集群

        如果您创建的是高安全集群\(可到**集群详情**页面查看安全模式\)，请参考下图进行配置：


## 权限配置示例 {#section_osm_th3_bfb .section}

上面一节中已经将Ranger集成到HDFS，现在可以进行相关的权限设置。例如给用户test授予/user/foo路径的写/执行权限：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/153829654211482_zh-CN.png)

单击上图中的**emr-hdfs**进入配置页面，配置相关权限。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17949/153829654311483_zh-CN.png)

按照上述步骤设置添加一个Policy后，就实现了对test的授权，然后用户test就可以对/user/foo的HDFS路径进行访问了。

**说明：** Policy被添加1分钟左右后，HDFS才会生效。

