# Kafka Manager使用说明 {#concept_xqc_3hd_z2b .concept}

从EMR-3.4.0版本开始，E-MapReduce支持通过Kafka Manager服务对Kafka集群进行管理。

## 操作步骤 {#section_yjv_b3d_z2b .section}

**说明：** 创建 Kafka集群时将默认安装Kafka Manager软件服务，并开启Kafka Manager的认证功能。

1.  使用SSH隧道方式访问 Web 界面，请参见 [SSH 登录集群](../../../../cn.zh-CN/集群规划与配置/集群配置/SSH 登录集群.md#)。

    **说明：** 

    -   建议您首次使用Kafka Manager时修改默认密码。
    -   为了防止8085端口暴露，建议使用SSH隧道方式来访问Web界面；如果使用http://localhost:8085方式访问Web界面时，请做好IP白名单保护，避免数据泄漏。
2.  输入用户名密码，请参考Kafka Manager的配置信息。

    ![Kafka Manager的配置信息](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17903/156860570410849_zh-CN.png)

3.  添加一个创建好的Kafka集群，需要注意配置正确Kafka集群的Zookeeper地址，可以参考Kakfa的配置信息。选择对应的Kafka版本，另外建议打开JMX功能。

    ![添加Kafka集群](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17903/156860570610850_zh-CN.png)

4.  创建好之后即可使用一些常见的Kakfa功能。

    ![常见Kakfa功能。](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17903/156860570610851_zh-CN.png)


