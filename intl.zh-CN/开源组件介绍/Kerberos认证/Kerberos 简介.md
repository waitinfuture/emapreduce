# Kerberos 简介 {#concept_anp_pgl_z2b .concept}

E-MapReduce 从 2.7.x/3.5.x 版本开始支持创建安全类型的集群，即集群中的开源组件以 Kerberos 的安全模式启动，在这种安全环境下只有经过认证的客户端（Client）才能访问集群的服务（Service，如 HDFS）。

## 前置 {#section_yrh_qhl_z2b .section}

目前 E-MapReduce 版本中支持的 Kerberos 的组件列表如下所示：

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

**说明：** Kafka/Presto/Storm 目前版本不支持 Kerberos。

创建安全集群

在集群创建页面的软件配置下打开**安全**按钮即可。

。

![创建安全集群](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/20194/155963239830950_zh-CN.png)

## Kerberos 身份认证原理 {#section_gf3_xkl_z2b .section}

Kerberos 是一种基于对称密钥技术的身份认证协议，它作为一个独立的第三方的身份认证服务，可以为其它服务提供身份认证功能，且支持 SSO （即客户端身份认证后，可以访问多个服务如 HBase/HDFS 等）。

Kerberos 协议过程主要有两个阶段，第一个阶段是 KDC 对 Client 身份认证，第二个阶段是 Service 对 Client 身份认证。

![Kerberos身份认证原理](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17934/155963239811118_zh-CN.png)

-   KDC

    Kerberos 的服务端程序

-   Client

    需要访问服务的用户（principal），KDC 和 Service 会对用户的身份进行认证

-   Service

    集成了 Kerberos 的服务，如 HDFS/YARN/HBase 等


-   KDC 对 Client 身份认证

    当客户端用户（principal）访问一个集成了 Kerberos 的服务之前，需要先通过 KDC 的身份认证。

    若身份认证通过则客户端会拿到一个 TGT（Ticket Granting Ticket），后续就可以拿该 TGT 去访问集成了 Kerberos 的服务。

-   Service 对 Client 身份认证

    当 2.1 中用户拿到 TGT 后，就可以继续访问 Service 服务。它会使用 TGT 以及需要访问的服务名称\(如 HDFS\)去 KDC 获取 SGT\(Service Granting Ticket\)，然后使用 SGT 去访问 Service，Service 会利用相关信息对 Client 进行身份认证，认证通过后就可以正常访问 Service 服务。


## Kerberos 实践 {#section_rls_lml_z2b .section}

EMR 的 Kerberos 安全集群中的服务在创建集群的时候会以 Kerberos 安全模式启动。

-   Kerberos 服务端程序为 HasServer
    -   登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com/)，选择**集群管理** \> **管理** \> **Has**，执行查看/修改配置/重启等操作。
    -   非 HA 集群部署在 emr-header-1，HA 集群部署在 emr-header-1/emr-header-2 两个节点
-   支持四种身份认证方式

    HasServer 可同时支持以下 4 种身份认证方式，客户端可以通过配置相关参数来指定 HasServer 使用哪种方式进行身份认证

    -   兼容 MIT Kerberos 的身份认证方式

        客户端配置：

        ``` {#codeblock_7wm_qzu_nro}
        如果在集群的某个节点上执行客户端命令，则需要将 
        /etc/ecm/hadoop-conf/core-site.xml 中 hadoop.security.authentication.use.has 设置为 false。
        如果有通过控制台的执行计划跑作业，则不能修改 master 节点上面 /etc/ecm/hadoop-conf/core-site.xml 中的值，否则执行计划的作业认证就不通过而失败，可以使用下面的方式
        export HADOOP_CONF_DIR=/etc/has/hadoop-conf临时 export 环境变量，该路径下的 hadoop.security.authentication.use.has 已经设置为 false。
        ```

        访问方式：Service的客户端包完全可使用开源的，如 HDFS 客户端等。[兼容 MIT Kerberos 认证](intl.zh-CN/开源组件介绍/Kerberos认证/兼容 MIT Kerberos 认证.md#)

    -   RAM 身份认证

        客户端配置：

        ``` {#codeblock_qde_tab_sxf}
        如果在集群的某个节点上执行客户端命令，则需要将 
        /etc/ecm/hadoop-conf/core-site.xml 中 hadoop.security.authentication.use.has 设置为true，/etc/has/has-client.conf中auth_type设置为RAM.
        如果有通过控制台的执行计划跑作业，则不能修改 master 节点上面 /etc/ecm/hadoop-conf/core-site.xml 以及 /etc/has/has-client.conf 中的值，否则执行计划的作业认证就不通过而失败，可以使用下面的方式 
        export HADOOP_CONF_DIR=/etc/has/hadoop-conf; export HAS_CONF_DIR=/path/to/has-client.conf 临时 export 环境变量，其中 HAS_CONF_DIR 文件夹下的 has-client.conf 的auth_type设置为 RAM。
        ```

        访问方式： 客户端需要使用集群中的软件包（如 Hadoop/HBase 等\)，[RAM 认证](intl.zh-CN/开源组件介绍/Kerberos认证/RAM 认证.md#)

    -   LDAP 身份认证

        客户端配置：

        ``` {#codeblock_mux_d03_4e3}
        如果在集群的某个节点上执行客户端命令，则需要将 
        /etc/ecm/hadoop-conf/core-site.xml 中 hadoop.security.authentication.use.has 设置为true，/etc/has/has-client.conf 中 auth_type 设置为 LDAP。
        如果有通过控制台的执行计划跑作业，则不能修改 master 节点上面 /etc/ecm/hadoop-conf/core-site.xml 以及 /etc/has/has-client.conf 中的值，否则执行计划的作业认证就不通过而失败，可以使用下面的方式 
        export HADOOP_CONF_DIR=/etc/has/hadoop-conf; export HAS_CONF_DIR=/path/to/has-client.conf 临时 export 环境变量，其中 HAS_CONF_DIR 文件夹下的 has-client.conf 的 auth_type 设置为 LDAP。
        ```

        访问方式：客户端需要使用集群中的软件包（如 Hadoop/HBase 等），[LDAP 认证](intl.zh-CN/开源组件介绍/Kerberos认证/LDAP 认证.md#)。

    -   执行计划认证

        如果用户有使用 EMR 控制台的执行计划提交作业，则 emr-header-1 节点的配置必须不能被修改（默认配置）。

        客户端配置：

        ``` {#codeblock_rsb_ka1_lhj}
        emr-header-1 上面的 /etc/ecm/hadoop-conf/core-site.xml 中 hadoop.security.authentication.use.has 设置为 true，/etc/has/has-client.conf 中 auth_type设置为 EMR。
        ```

        访问方式：跟非 Kerberos 安全集群使用方式一致，[执行计划认证](intl.zh-CN/开源组件介绍/Kerberos认证/执行计划认证.md#)

        。

-   其他

    登陆 master 节点访问集群

    集群管理员也可以登陆 master 节点访问集群服务，登陆 master 节点切换到 has 账号（默认使用兼容 MIT Kerberos 的方式）即可访问集群服务，方便做一些排查问题或者运维等。

    ``` {#codeblock_ky3_07q_bxv}
    >sudo su has
    >hadoop fs -ls /
    ```

    **说明：** 也可以登录其他账号操作集群，前提是该账号可以通过 Kerberos 认证。另外，如果在 master 节点上需要使用兼容 MITKerberos 的方式，需要在该账号下先 export 一个环境变量。

     `export HADOOP_CONF_DIR=/etc/has/hadoop-conf/`


