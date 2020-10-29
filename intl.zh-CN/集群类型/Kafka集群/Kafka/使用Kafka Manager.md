# 使用Kafka Manager

E-MapReduce支持通过Kafka Manager服务对Kafka集群进行管理。

已创建Kafka类型的集群，创建详情请参见[创建集群](/intl.zh-CN/集群管理/集群配置/创建集群.md)。

**说明：** 创建Kafka集群时，默认安装Kafka Manager软件服务，并开启Kafka Manager的认证功能。

1.  使用SSH隧道方式访问Web页面，详情请参见[通过SSH隧道方式访问开源组件Web UI](/intl.zh-CN/集群管理/集群配置/连接集群/通过SSH隧道方式访问开源组件Web UI.md)。

    **说明：**

    -   建议您首次使用Kafka Manager时修改默认密码。
    -   为了防止8085端口暴露，建议使用SSH隧道方式来访问Web界面。如果使用http://localhost:8085方式访问Web界面，请做好IP白名单保护，避免数据泄漏。
2.  在登录页面，输入用户名和密码。

    用户名和密码可以通过以下步骤获取：

    1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)
    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。
    3.  单击上方的**集群管理**页签。
    4.  在**集群管理**页面的集群列表中，单击对应集群后面的**详情**。
    5.  在左侧导航栏，选择**集群服务** \> **Kafka-Manager**。
    6.  单击**配置**，在**服务配置**区域，查看以下参数的值：

        -   **kafka.manager.authentication.username**：登录Kafka Manager页面的用户名。
        -   **kafka.manager.authentication.password**：登录Kafka Manager页面的密码。
        -   kafka.manager.zookeeper.hosts：Kafka集群的Zookeeper地址。
        ![kafka configure](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6548559951/p66976.png)

3.  添加已创建的Kafka集群。

    1.  输入集群名称。

    2.  配置Kafka集群的Zookeeper地址。

        填写在[步骤2](#step_ch3_0jd_29k)中获取kafka.manager.zookeeper.hosts的值。

    3.  选择对应的Kafka版本。

    4.  建议打开JMX功能。

        ![添加Kafka集群](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6548559951/p10850.png)

        创建好之后即可使用常见的Kafka功能。

        ![常见Kafka功能。](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/6548559951/p10851.png)


