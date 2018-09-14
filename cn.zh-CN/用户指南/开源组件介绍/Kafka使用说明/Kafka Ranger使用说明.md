# Kafka Ranger使用说明 {#concept_blg_2gx_y2b .concept}

从 EMR-3.12.0 版本开始，E-MapReduce Kafka 支持用Ranger进行权限配置。

## Kafka集成Ranger {#section_kk2_fxb_z2b .section}

前面简介中介绍了E-MapReduce中创建启动Ranger服务的集群，以及一些准备工作，本节介绍Kafka集成Ranger的一些步骤流程。

-   **Enable Kakfa Plugin**
    1.  在**集群配置管理**页面的 **Ranger** 服务下，单击右侧的**操作**下拉菜单，选择 **Enable Kafka PLUGIN**。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/153690882310838_zh-CN.png)

    2.  单击右上角的**查看操作历史**查看任务进度，等待任务完成100%。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/153690882310839_zh-CN.png)

-   **重启Kafka broker**

    上述任务完成后，需要重启broker才能生效。

    1.  在集群配置管理页面中，从 **Ranger** 切换到 **Kafka**。
    2.  点击右上角 **Actions** 中的 **RESTART Broker**。
    3.  点击右上角**查看操作历史**查看任务进度，等待重启任务完成。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/153690882310840_zh-CN.png)

-   **Ranger UI页面添加Kafka Service**

    参见[Ranger简介](cn.zh-CN/用户指南/组件授权/RANGER/Ranger简介.md#)的介绍进入Ranger UI页面。

    在 **Ranger** 的UI页面添加Kafka Service：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/153690882310841_zh-CN.png)

    配置Kafka Service：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/153690882310842_zh-CN.png)


## 权限配置示例 {#section_nkp_hyb_z2b .section}

上面一节中已经将Ranger集成到Kafka，现在可以进行相关的权限设置。

**说明：** 标准集群中，在添加了 Kafka Service 后，ranger 会默认生成规则 **all - topic**，不作任何权限限制（即允许所有用户进行所有操作），此时ranger无法通过用户进行权限识别。

以test用户为例，添加Publish权限：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/153690882410843_zh-CN.png)

按照上述步骤设置添加一个Policy后，就实现了对 test 的授权，然后用户 test 就可以对 test 的topic进行写入操作。

**说明：** Policy添加后需要1分钟左右才会生效。

