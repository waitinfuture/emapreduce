# Kerberos简介 {#concept_anp_pgl_z2b .concept}

E-MapReduce从2.7.x/3.5.x版本开始支持创建安全类型的集群，即集群中的开源组件以Kerberos的安全模式启动,在这种安全环境下只有经过认证的客户端\(Client\)才能访问集群的服务\(Service，如HDFS\)。

## 前置 {#section_yrh_qhl_z2b .section}

目前E-MapReduce版本中支持的Kerberos的组件列表如下所示：

|组件名称|组件版本|
|:---|:---|
|YARN|2.7.2|
|SPARK|2.1.1/1.6.3|
|HIVE|2.0.1|
|TEZ|0.8.4|
|ZOOKEEPER|3.4.6|
|HUE|3.12.0|
|ZEPPELIN|0.7.1|
|OOZIE|4.2.0|
|SQOOP|1.4.6|
|HBASE|1.1.1|
|PHOENIX|4.7.0|

**说明：** Kafka/Presto/Storm目前版本不支持Kerberos。

创建安全集群

在集群创建页面的软件配置下打开**安全**按钮即可，如下所示:

![创建安全集群](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/20194/155255222230950_zh-CN.png)

## Kerberos身份认证原理 {#section_gf3_xkl_z2b .section}

Kerberos是一种基于对称密钥技术的身份认证协议，它作为一个独立的第三方的身份认证服务，可以为其它服务提供身份认证功能，且支持SSO\(即客户端身份认证后，可以访问多个服务如HBase/HDFS等\)。

Kerberos协议过程主要有两个阶段，第一个阶段是KDC对Client身份认证，第二个阶段是Service对Client身份认证。

![Kerberos身份认证原理](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17934/155255222211118_zh-CN.png)

-   KDC

    Kerberos的服务端程序

-   Client

    需要访问服务的用户\(principal\)，KDC和Service会对用户的身份进行认证

-   Service

    集成了Kerberos的服务，如HDFS/YARN/HBase等


-   KDC对Client身份认证

    当客户端用户\(principal\)访问一个集成了Kerberos的服务之前，需要先通过KDC的身份认证。

    若身份认证通过则客户端会拿到一个TGT\(Ticket Granting Ticket\)，后续就可以拿该TGT去访问集成了Kerberos的服务。

-   Service对Client身份认证

    当2.1中用户拿到TGT后，就可以继续访问Service服务。它会使用TGT以及需要访问的服务名称\(如HDFS\)去KDC获取SGT\(Service Granting Ticket\)，然后使用SGT去访问Service，Service会利用相关信息对Client进行身份认证，认证通过后就可以正常访问Service服务。


## EMR实践 {#section_rls_lml_z2b .section}

EMR的Kerberos安全集群中的服务在创建集群的时候会以Kerberos安全模式启动。

-   Kerberos服务端程序为HasServer
    -   登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com/)，选择**集群管理** \> **管理** \> **Has**，执行查看/修改配置/重启等操作。
    -   非HA集群部署在emr-header-1,HA集群部署在emr-header-1/emr-header-2两个节点
-   支持四种身份认证方式

    HasServer可同时支持以下4种身份认证方式，客户端可以通过配置相关参数来指定HasServer使用哪种方式进行身份认证

    -   兼容MIT Kerberos的身份认证方式

        客户端配置:

        ```
        如果在集群的某个节点上执行客户端命令，则需要将
        /etc/ecm/hadoop-conf/core-site.xml中hadoop.security.authentication.use.has设置为false.
        如果有通过控制台的执行计划跑作业，则不能修改master节点上面/etc/ecm/hadoop-conf/core-site.xml中的值，否则执行计划的作业认证就不通过而失败，可以使用下面的方式
        export HADOOP_CONF_DIR=/etc/has/hadoop-conf临时export环境变量，该路径下的hadoop.security.authentication.use.has已经设置为false
        ```

        访问方式:Service的客户端包完全可使用开源的，如HDFS客户端等。[详见](intl.zh-CN/用户指南/Kerberos认证/兼容MIT Kerberos认证.md#)

    -   RAM身份认证

        客户端配置:

        ```
        如果在集群的某个节点上执行客户端命令，则需要将
        /etc/ecm/hadoop-conf/core-site.xml中hadoop.security.authentication.use.has设置为true，/etc/has/has-client.conf中auth_type设置为RAM.
        如果有通过控制台的执行计划跑作业，则不能修改master节点上面/etc/ecm/hadoop-conf/core-site.xml以及/etc/has/has-client.conf中的值，否则执行计划的作业认证就不通过而失败，可以使用下面的方式
        export HADOOP_CONF_DIR=/etc/has/hadoop-conf; export HAS_CONF_DIR=/path/to/has-client.conf临时export环境变量，其中HAS_CONF_DIR文件夹下的has-client.conf的auth_type设置为RAM
        ```

        访问方式： 客户端需要使用集群中的软件包\(如Hadoop/HBase等\)，[详见](intl.zh-CN/用户指南/Kerberos认证/RAM 认证.md#)

    -   LDAP身份认证

        客户端配置：

        ```
        如果在集群的某个节点上执行客户端命令，则需要将
        /etc/ecm/hadoop-conf/core-site.xml中hadoop.security.authentication.use.has设置为true，/etc/has/has-client.conf中auth_type设置为LDAP.
        如果有通过控制台的执行计划跑作业，则不能修改master节点上面/etc/ecm/hadoop-conf/core-site.xml以及/etc/has/has-client.conf中的值，否则执行计划的作业认证就不通过而失败，可以使用下面的方式
        export HADOOP_CONF_DIR=/etc/has/hadoop-conf; export HAS_CONF_DIR=/path/to/has-client.conf临时export环境变量，其中HAS_CONF_DIR文件夹下的has-client.conf的auth_type设置为LDAP
        ```

        访问方式：客户端需要使用集群中的软件包\(如Hadoop/HBase等\)，[详见](intl.zh-CN/用户指南/Kerberos认证/LDAP认证.md#)。

    -   执行计划认证

        如果用户有使用EMR控制台的执行计划提交作业，则emr-header-1节点的配置必须不能被修改\(默认配置\)。

        客户端配置:

        ```
        emr-header-1上面的/etc/ecm/hadoop-conf/core-site.xml中hadoop.security.authentication.use.has设置为true，/etc/has/has-client.conf中auth_type设置为EMR.
        ```

        访问方式：跟非Kerberos安全集群使用方式一致，[详见](intl.zh-CN/用户指南/Kerberos认证/执行计划认证.md#)

        。

-   其他

    登陆master节点访问集群

    集群管理员也可以登陆master节点访问集群服务，登陆master节点切换到has账号\(默认使用兼容MIT Kerberos的方式\)即可访问集群服务，方便做一些排查问题或者运维等。

    ```
    >sudo su has
    >hadoop fs -ls /
    ```

    **说明：** 也可以登录其他账号操作集群，前提是该账号可以通过Kerberos认证。另外，如果在master节点上需要使用 兼容MITKerberos的方式，需要在该账号下先export一个环境变量。

    `export HADOOP_CONF_DIR=/etc/has/hadoop-conf/`


