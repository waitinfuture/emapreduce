# Kafka Manager 使用说明 {#concept_xqc_3hd_z2b .concept}

从EMR-3.4.0版本开始将支持Kafka Manager服务进行Kafka集群管理。

## 操作步骤 {#section_yjv_b3d_z2b .section}

**说明：** 创建Kafka集群时将默认安装Kafka Manager软件服务，并开启Kafka Manager的认证功能。我们强烈建议您首次使用Kafka Manager时修改默认密码，且使用SSH隧道方式来访问。不建议您公网暴露8085端口，否则需要做好IP白名单保护，避免数据泄漏。

-   建议使用SSH隧道方式访问Web界面，参考[SSH 登录集群](intl.zh-CN/用户指南/SSH 登录集群.md#)。
-   访问 http://localhost:8085。
-   输入用户名密码，请参考Kafka Manager的配置信息。

    ![Kafka Manager的配置信息](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17903/154770955210849_zh-CN.png)

-   添加一个创建好的Kafka集群，需要注意配置正确Kafka集群的Zookeeper地址，不清楚的可以看Kakfa的配置信息。选择对应的Kafka版本，另外建议打开JMX功能。

    ![添加Kafka集群](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17903/154770955210850_zh-CN.png)

-   创建好之后即可使用一些常见的Kakfa功能。

    ![常见Kakfa功能。](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17903/154770955210851_zh-CN.png)


