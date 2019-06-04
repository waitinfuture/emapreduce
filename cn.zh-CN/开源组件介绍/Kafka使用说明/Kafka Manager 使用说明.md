# Kafka Manager 使用说明 {#concept_xqc_3hd_z2b .concept}

从 EMR-3.4.0 版本开始 E-MapReduce 将支持 Kafka Manager 服务进行 Kafka 集群管理。

## 操作步骤 {#section_yjv_b3d_z2b .section}

**说明：** 创建 Kafka 集群时将默认安装 Kafka Manager 软件服务，并开启 Kafka Manager 的认证功能。我们强烈建议您首次使用 Kafka Manager 时修改默认密码，且使用 SSH 隧道方式来访问。不建议您公网暴露 8085 端口，否则需要做好 IP 白名单保护，避免数据泄漏。

-   建议使用 SSH 隧道方式访问 Web 界面，请参见 [SSH 登录集群](../../../../intl.zh-CN/集群规划与配置/集群配置/SSH 登录集群.md#)。
-   访问 *http://localhost:8085* 。
-   输入用户名密码，请参考 Kafka Manager 的配置信息。

    ![Kafka Manager的配置信息](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17903/155963099010849_zh-CN.png)

-   添加一个创建好的 Kafka 集群，需要注意配置正确 Kafka 集群的 Zookeeper 地址，可以参考 Kakfa 的配置信息。选择对应的 Kafka 版本，另外建议打开 JMX 功能。

    ![添加Kafka集群](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17903/155963099010850_zh-CN.png)

-   创建好之后即可使用一些常见的 Kakfa 功能。

    ![常见Kakfa功能。](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17903/155963099010851_zh-CN.png)


