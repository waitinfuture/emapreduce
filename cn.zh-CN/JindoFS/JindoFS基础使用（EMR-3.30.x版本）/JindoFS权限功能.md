# JindoFS权限功能

本文介绍JindoFS的namespace的存储模式（Block或Cache）支持的文件系统权限功能。Block模式和Cache模式不支持切换。

根据您namespace的存储模式，JindoFS支持的系统权限如下：

-   当您namespace的存储模式是Block模式时，支持Unix和Ranger权限。
    -   Unix权限：你可以设置文件的777权限，以及Owner和Group。
    -   Ranger权限：您可以执行复杂或高级操作。例如使用路径通配符。
-   当您namespace的存储模式是Cache模式时，仅支持Ranger权限。

    您可以执行复杂或高级操作。例如使用路径通配符。


![JindoFS权限](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5157459951/p101944.png)

## 启用JindoFS Unix权限

1.  进入SmartData服务。

    1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

    3.  单击上方的**集群管理**页签。

    4.  在**集群管理**页面，单击相应集群所在行的**详情**。

    5.  在左侧导航栏单击**集群服务** \> **SmartData**。

2.  进入namespace服务配置。

    1.  单击**配置**页签。

    2.  单击**namespace**。

        ![namespace_smartdata](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1906533061/p175738.png)

3.  单击**自定义配置**，在**新增配置项**对话框中，设置**Key**为jfs.namespaces.<namespace\>.permission.method，**Value**为unix，单击**确定**。

4.  保存配置。

    1.  单击右上角的**保存**。

    2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。

    3.  单击**确定**。

5.  重启配置。

    1.  单击右上角的**操作** \> **重启 Jindo Namespace Service**。

    2.  输入执行原因，单击**确定**。

    开启文件系统权限后，使用方式跟HDFS一样。支持以下命令。

    ```
    hadoop fs -chmod 777 jfs://{namespace_name}/dir1/file1
    hadoop fs -chown john:staff jfs://{namespace_name}/dir1/file1
    ```

    如果用户对某一个文件没有权限，将返回如下错误信息。

    ![error](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5157459951/p101887.png)


## 启用JindoFS Ranger权限

您可以在Apache Ranger组件上配置用户权限，在JindoFS上开启Ranger插件后，就可以在Ranger上对JindoFS权限（和其它组件权限）进行一站式管理。

1.  添加Ranger。

    1.  在**namespace**页签，单击**自定义配置**。

    2.  在**新增配置项**对话框中，设置**Key**为jfs.namespaces.<namespace\>.permission.method，**Value**为ranger。

    3.  保存配置。

        1.  单击右上角的**保存**。
        2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。
        3.  单击**确定**。
    4.  重启配置。

        1.  单击右上角的**操作** \> **重启 Jindo Namespace Service**。
        2.  输入执行原因，单击**确定**。
2.  配置Ranger。

    1.  进入Ranger UI页面。

        详情请参见[概述](/cn.zh-CN/集群类型/Hadoop集群/Ranger/概述.md)。

    2.  Ranger UI添加HDFS service。

        ![Ranger UI](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/5157459951/p11479.png)

    3.  配置相关参数。

        |参数|描述|
        |--|--|
        |**Service Name**|固定格式：jfs-\{namespace\_name\}。例如：jfs-test。 |
        |**Username**|自定义。|
        |**Password**|自定义。|
        |**Namenode URL**|输入jfs://\{namespace\_name\}/。|
        |**Authorization Enabled**|使用默认值No。|
        |**Authentication Type**|使用默认值Simple。|
        |**dfs.datanode.kerberos.principal**|不填写。|
        |**dfs.namenode.kerberos.principal**|
        |**dfs.secondary.namenode.kerberos.principal**|
        |**Add New Configurations**|

    4.  单击**Add**。


## 启用JindoFS Ranger权限+LDAP用户组

如果您在Ranger UserSync上开启了从LDAP同步用户组信息的功能，则JindoFS也需要修改相应的配置，以获取LDAP的用户组信息，从而对当前用户组进行Ranger权限的校验。

1.  在**namespace**页签，单击**自定义配置**。

2.  在**新增配置项**对话框中，参见以下示例设置参数来配置LDAP，单击**确定**。

    **说明：** 配置项请遵循开源HDFS内容，详情请参见[core-default.xml](https://hadoop.apache.org/docs/r2.8.0/hadoop-project-dist/hadoop-common/core-default.xml)。

    |参数|示例|
    |--|--|
    |hadoop.security.group.mapping|org.apache.hadoop.security.CompositeGroupsMapping|
    |hadoop.security.group.mapping.providers|shell4services,ad4users|
    |hadoop.security.group.mapping.providers.combined|true|
    |hadoop.security.group.mapping.provider.shell4services|org.apache.hadoop.security.ShellBasedUnixGroupsMapping|
    |hadoop.security.group.mapping.provider.ad4users|org.apache.hadoop.security.LdapGroupsMapping|
    |hadoop.security.group.mapping.ldap.url|ldap://emr-header-1:10389|
    |hadoop.security.group.mapping.ldap.search.filter.user|\(&\(objectClass=person\)\(uid=\{0\}\)\)|
    |hadoop.security.group.mapping.ldap.search.filter.group|\(objectClass=groupOfNames\)|
    |hadoop.security.group.mapping.ldap.base|o=emr|

3.  保存配置。

    1.  单击右上角的**保存**。

    2.  在**确认修改**对话框中，输入执行原因，开启**自动更新配置**。

    3.  单击**确定**。

4.  重启配置。

    1.  单击右上角的**操作** \> **重启 All Components**。

    2.  输入执行原因，单击**确定**。

5.  通过SSH登录emr-header-1节点，配置Ranger UserSync并启用LDAP选项。

    详情请参见[Ranger Usersync集成LDAP](/cn.zh-CN/集群类型/Hadoop集群/Ranger/Ranger对接LDAP/Ranger Usersync集成LDAP.md)。


