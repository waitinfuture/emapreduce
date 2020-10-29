# Ranger Admin集成LDAP

本文介绍Ranger Admin如何集成LDAP，以便于您使用LDAP中的用户登录Ranger WebUI。

Ranger的用户可以分为Internal User和External User。LDAP和UNIX用户同步至Ranger后属于Internal User，在Ranger WebUI中创建的用户属于Internal User。在Ranger中配置权限时，可以给Internal User和External User授权。

Ranger Admin集成LDAP后，LDAP中的用户可以登录Ranger WebUI。登录后，Ranger会在用户管理中同步创建该用户为External User。初始状态下，该用户仅能查看Ranger Service和Policy。管理员用户admin可以在用户管理中，升级普通用户为管理员用户。

## EMR-3.28.0及后续版本（EMR 3.x系列）和EMR-4.3.0及后续版本配置方法

1.  进入**配置**页签。

    1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

    3.  单击上方的**集群管理**页签。

    4.  在**集群管理**页面，单击待操作集群所在行的**详情**。

    5.  在左侧导航栏，单击**集群服务** \> **RANGER**。

    6.  单击**配置**页签。

2.  在**ranger-admin-site**页面，配置各项参数。

    1.  在**服务配置**区域，单击**ranger-admin-site**页签。

    2.  配置如下参数，同步LDAP用户至Ranger。

        |参数|固定值|
        |--|---|
        |**ranger.ldap.bind.dn**|uid=admin,o=emr|
        |**ranger.ldap.bind.password**|密码为OpenLDAP的配置页面，**manager\_password**的值。|
        |**ranger.ldap.base.dn**|ou=people,o=emr|
        |**ranger.authentication.method**|LDAP|
        |**ranger.ldap.url**|ldap://emr-header-1:10389|
        |**ranger.ldap.user.dnpattern**|uid=\{0\},ou=people,o=emr|

3.  重启Ranger Admin，使配置生效。

    1.  在左侧导航栏，单击**集群服务** \> **RANGER**。

    2.  在**组件列表**区域，单击**RangerAdmin**所在行的**重启**。

    3.  在**执行集群操作**对话框中，配置各项参数。

    4.  单击**确定**。

    5.  在**确认**对话框中，单击**确定**。


## EMR-3.28.0之前版本（EMR 3.x系列）和EMR-4.3.0之前版本（EMR 4.x系列）配置方法

1.  登录集群的emr-header-1节点，详情请参见[使用SSH连接主节点](/intl.zh-CN/集群管理/集群配置/连接集群/使用SSH连接主节点.md)。

2.  编辑install.properties文件。

    ```
    cd /usr/lib/ranger-usersync-current
    vim install.properties
    ```

3.  修改如下配置项。

    ```
    authentication_method = LDAP
    xa_ldap_url = ldap://emr-header-1:10389
    xa_ldap_userDNpattern = uid={0},ou=people,o=emr
    xa_ldap_base_dn = ou=people,o=emr
    xa_ldap_bind_dn = uid=admin,o=emr
    xa_ldap_bind_password = [password]
    ```

    本示例给出的配置值均为对接的EMR OpenLDAP的值。如果您需要对接自建的LDAP，需要修改各参数值为自建LDAP对应的值。各参数详细解释，请参见[Ranger Admin官方安装教程](https://cwiki.apache.org/confluence/display/RANGER/Apache+Ranger+0.5.0+Installation#ApacheRanger0.5.0Installation-InstallStepsforRangerPolicyAdminonRHEL/CentOS)。

    |配置项|说明|
    |---|--|
    |`xa_ldap_url`|LDAP服务的地址。例如`ldap://ldap.example.com:389`。|
    |`xa_ldap_userDNpattern`|登录用户匹配LDAP dn的pattern。例如`uid={0},ou=users,dc=example,dc=com`，表示当用户hadoop在WebUI登录时，其对应检查的LDAP dn为`uid=hadoop,ou=users,dc=example,dc=com`。|
    |`xa_ldap_base_dn`|LDAP中用户搜索域。例如`ou=users,dc=example,dc=com`。|
    |`xa_ldap_bind_dn`|连接LDAP进行用户和用户组查询的dn。例如`cn=ldapadmin,ou=users,dc=example,dc=com`。|
    |`xa_ldap_bind_password`|用于连接的dn对应的密码。|

4.  在emr-header-1节点的/usr/lib/ranger-usersync-current路径下执行`setup.sh`。

    ```
    cd /usr/lib/ranger-usersync-current
    sh setup.sh
    ```

5.  重启Ranger Admin，使配置生效。

    1.  在左侧导航栏，单击**集群服务** \> **RANGER**。

    2.  在**组件列表**区域，单击**RangerAdmin**所在行的**重启**。

    3.  在**执行集群操作**对话框中，配置各项参数。

    4.  单击**确定**。

    5.  在**确认**对话框中，单击**确定**。


