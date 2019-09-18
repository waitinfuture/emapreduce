# 如何将HiveServer2的认证方式设置为LDAP {#concept_2229846 .concept}

本节介绍如何将HiveServer2的认证方式设置为LDAP。

## 问题 {#section_i1i_oq0_150 .section}

如何将HiveServer2的认证方式设置为LDAP？

## 回答 {#section_i24_7qi_zh7 .section}

在E-MapRduce集群中，HiveServer2支持多种认证方式，包括NOSASL、None、LDAP、Kerberos、PAM和Custom，这些认证方式均可通过**hive.server2.authentication**参数来设置。

1.  登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com/)。
2.  新增LDAP认证配置项并重启HiveServer2。
    1.  查找到待配置集群，然后进入Hive组件的**配置** \> **hiveserver2-site**页面。

        ![hiveserver2-site配置页面](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1770004/156877657860649_zh-CN.png)

    2.  单击**自定义配置**，新增配置项。

        **LDAP**认证方式需要新增如下三个配置项。

        |配置项|值|说明|
        |---|--|--|
        |**hive.server2.authentication**|**LDAP**|认证方式。|
        |**hive.server2.authentication.ldap.url**|格式为**ldap://$\{emr-header-1-hostname\}:10389**|**$\{emr-header-1-hostname\}**需要以实际主机名称为准，您可在集群的emr-header-1上执行hostname命令获取（[以SSH方式登录E-MapReduce集群的Master主机](../../../../cn.zh-CN/集群规划与配置/集群配置/SSH 登录集群.md#)）。|
        |**hive.server2.authentication.ldap.baseDN**|**ou=people,o=emr**|-|

    3.  完成上述参数配置后，单击右上方的**保存**。
    4.  在弹出的对话框中填写变更描述，然后单击**确定**，系统提示操作成功。
    5.  在右上方单击**操作** \> **重启HiveServer2**，重启HiveServer2。
3.  在LDAP中添加账号。

    在E-MapReduce集群中，OpenLDAP组件是一个LDAP的服务，默认用于管理Knox的用户账号，HiveServer2的LDAP认证方式可以复用Knox的账号体系。添加账号的方法请参见[Knox使用说明 \> 准备工作 \> 设置Konx用户 \> 使用集群中的LDAP服务 \> 方式一（推荐）](../../../../cn.zh-CN/开源组件介绍/Knox 使用说明.md#)。本例新增账号为**emr-test**。

4.  测试新增账号是否可正常登录HiveServer2。

    通过/usr/lib/hive-current/bin/beeline登录HiveServer2，正常登录情况如下：

    ``` {#codeblock_57s_ou7_4kz}
    beeline> !connect jdbc:hive2://emr-header-1:10000/
    Enter username for jdbc:hive2://emr-header-1:10000/: emr-guest
    Enter password for jdbc:hive2://emr-header-1:10000/: emr-guest-pwd
    Transaction isolation: TRANSACTION_REPEATABLE_READ
    ```

    如果账号密码不正确，则会显示如下异常：

    ``` {#codeblock_ygj_v3z_dal}
    Error: Could not open client transport with JDBC Uri: jdbc:hive2://emr-header-1:10000/: Peer indicated failure: Error validating the login (state=08S01,code=0)
    ```


