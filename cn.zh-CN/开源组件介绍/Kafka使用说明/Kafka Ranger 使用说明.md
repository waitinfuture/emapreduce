# Kafka Ranger 使用说明 {#concept_blg_2gx_y2b .concept}

前面简介中介绍了 E-MapReduce 中创建启动 Ranger 服务的集群，以及一些准备工作，本节介绍 Kafka 集成 Ranger 的一些步骤流程。

## Kafka 集成 Ranger {#section_kk2_fxb_z2b .section}

从 E-MapReduce-3.12.0 版本开始，E-MapReduce Kafka 支持用 Ranger 进行权限配置，步骤如下：

-   Enable Kakfa Plugin
    1.  在**集群与服务管理**页面的 **Ranger** 服务下，单击右侧的**操作**下拉菜单，选择 **Enable Kafka PLUGIN**。

        ![集群与服务管理](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/155963094810838_zh-CN.png)

    2.  单击右上角的**查看操作历史**查看任务进度，等待任务完成100%。

        ![操作历史](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/155963094810839_zh-CN.png)

-   重启 Kafka broker

    上述任务完成后，需要重启 broker 才能生效。

    1.  在**集群与服务管理**页面的服务列表中单击 **Kafka** 进入 Kafka 服务配置页面。
    2.  点击右上角 **操作** 中的 **RESTART Kafka Broker**。
    3.  点击右上角**查看操作历史**查看任务进度，等待重启任务完成。

        ![查看操作历史](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/155963094810840_zh-CN.png)

-   Ranger UI 页面添加 Kafka Service

    参见 [Ranger 简介](intl.zh-CN/开源组件介绍/组件授权/RANGER/Ranger 简介.md#)进入 Ranger UI 页面。

    在 **Ranger** 的 UI 页面添加 Kafka Service：

    ![Ranger WebUI](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/155963094810841_zh-CN.png)

    配置 Kafka Service：

    ![配置Kafka Service](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/155963094810842_zh-CN.png)


## 权限配置示例 {#section_nkp_hyb_z2b .section}

上面一节中已经将 Ranger 集成到 Kafka，现在可以进行相关的权限设置。

**说明：** 标准集群中，在添加了 Kafka Service 后，ranger 会默认生成规则 all - topic，不作任何权限限制（即允许所有用户进行所有操作），此时ranger无法通过用户进行权限识别。

以 test 用户为例，添加 Publish 权限：

![添加Publish权限](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/155963094910843_zh-CN.png)

按照上述步骤设置添加一个 Policy 后，就实现了对 test 的授权，然后用户 test 就可以对 test 的topic进行写入操作。

**说明：** Policy 添加后需要 1 分钟左右才会生效。

