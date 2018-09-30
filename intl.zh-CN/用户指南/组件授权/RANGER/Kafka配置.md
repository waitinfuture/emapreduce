# Kafka配置 {#concept_ury_ktj_bfb .concept}

从EMR-3.12.0版本开始，E-MapReduce Kafka支持用Ranger进行权限配置。

## Kafka集成Ranger {#section_svh_g5j_bfb .section}

前面简介中介绍了E-MapReduce中创建启动Ranger服务的集群，以及一些准备工作，本节介绍Kafka集成Ranger的一些步骤流程。

-   Enable Kakfa Plugin
    1.  在集群配置页面，在你想操作的集群后的操作栏中单击**管理**
    2.  在服务列表中单击**Ranger**进入Ranger配置页面
    3.  在**集群配置管理**页面的Ranger服务下，单击右侧的**操作**下拉菜单，选择**Enable Kafka PLUGIN**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17952/153829643211548_zh-CN.png)

    4.  在弹出框输入备注信息，然后单击**确定**。

        单击右上角的**查看操作历史**查看任务进度，等待任务完成100%。

-   重启Kafka broker

    上述任务完成后，需要重启broker才能生效。

    1.  在集群配置管理页面中，从**Ranger**切换到**Kafka**。
    2.  单击右上角**操作**下拉菜单，选择**RESTART Broker**。
    3.  单击右上角**查看操作历史**查看任务进度，等待重启任务完成。
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17952/153829643211556_zh-CN.png)

-   Ranger UI页面添加Kafka Service

    参见[Ranger简介](https://help.aliyun.com/document_detail/66410.html?spm=a2c4g.11186623.2.7.6bcf79f4rJKjjQ)的介绍进入Ranger UI页面。

    在Ranger的UI页面添加Kafka Service:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17952/153829643211560_zh-CN.png)

    配置Kafka Service:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17952/153829643211561_zh-CN.png)


## 权限配置示例 {#section_mgr_yxj_bfb .section}

上面一节中已经将Ranger集成到Kafka，现在可以进行相关的权限设置。

**说明：** 标准集群中，在添加了Kafka Service后，ranger会默认生成规则all - topic，不作任何权限限制（即允许所有用户进行所有操作），此时ranger无法通过用户进行权限识别。

以test用户为例，添加Publish权限：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17952/153829643311563_zh-CN.png)

按照上述步骤设置添加一个Policy后，就实现了对test的授权，然后用户test就可以对test的topic进行写入操作。

**说明：** Policy添加后需要1分钟左右才会生效。

