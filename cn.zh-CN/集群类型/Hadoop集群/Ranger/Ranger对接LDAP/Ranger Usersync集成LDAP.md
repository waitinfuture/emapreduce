# Ranger Usersync集成LDAP

本文介绍Ranger Usersync如何集成LDAP，以便于您在配置Ranger的Policy时，可以授权LDAP中的用户或用户组访问组件。

已创建集群，详情请参见[创建集群](/cn.zh-CN/集群管理/集群配置/创建集群.md)。

EMR的OpenLDAP没有配置用户组，如果您需要使用LDAP配置用户组，需要自行配置。如果您需要同步LDAP用户组至Ranger，请根据LDAP实际配置，自行配置LDAP各项参数。

## EMR-3.28.0及后续版本（EMR 3.x系列）和EMR-4.3.0及后续版本配置方法

1.  进入**配置**页签。

    1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

    3.  单击上方的**集群管理**页签。

    4.  在**集群管理**页面，单击待操作集群所在行的**详情**。

    5.  在左侧导航栏，单击**集群服务** \> **RANGER**。

    6.  单击**配置**页签。

2.  在**ranger-ugsync-site**页面，配置各项参数。

    1.  在**服务配置**区域，单击**ranger-ugsync-site**页签。

    2.  配置如下参数，同步LDAP用户至Ranger。

        |参数|固定值|
        |--|---|
        |**ranger.usersync.sync.source**|ldap|
        |**ranger.usersync.ldap.binddn**|uid=admin,o=emr|
        |**ranger.usersync.ldap.ldapbindpassword**|密码为OpenLDAP的配置页面，**manager\_password**的值。|
        |**ranger.usersync.ldap.searchBase**|o=emr|
        |**ranger.usersync.ldap.url**|ldap://emr-header-1:10389|
        |**ranger.usersync.ldap.user.nameattribute**|cn|
        |**ranger.usersync.ldap.user.objectclass**|person|
        |**ranger.usersync.ldap.user.searchbase**|ou=people,o=emr|
        |**ranger.usersync.source.impl.class**|org.apache.ranger.ldapusersync.process.LdapUserGroupBuilder|
        |**ranger.usersync.sleeptimeinmillisbetweensynccycle**|3600000|

        **说明：** 当您创建的是高安全集群时，请配置**ranger.usersync.ldap.user.searchfilter**为\(!\(cn=\*/\*\)\)，以便于过滤掉OpenLDAP中已创建的组件服务的Kerberos Principal记录。

    3.  如果您需要同步LDAP用户组至Ranger，请根据LDAP实际信息配置如下参数。

        |参数|示例|
        |--|--|
        |**ranger.usersync.group.memberattributename**|member|
        |**ranger.usersync.group.nameattribute**|cn|
        |**ranger.usersync.group.objectclass**|groupofnames|
        |**ranger.usersync.group.searchbase**|ou=groups,o=emr|
        |**ranger.usersync.group.searchenabled**|true|
        |**ranger.usersync.group.usermapsyncenabled**|true|
        |**ranger.usersync.sleeptimeinmillisbetweensynccycle**|3600000|

3.  重启Ranger UserSync，使配置生效。

    1.  在左侧导航栏，单击**集群服务** \> **RANGER**。

    2.  在**组件列表**区域，单击**RangerUserSync**所在行的**重启**。

    3.  在**执行集群操作**对话框中，配置各项参数。

    4.  单击**确定**。

    5.  在**确认**对话框中，单击**确定**。


## EMR-3.28.0之前版本（EMR 3.x系列）和EMR-4.3.0之前版本（EMR 4.x系列）配置方法

1.  登录集群的emr-header-1节点，详情请参见[使用SSH连接主节点](/cn.zh-CN/集群管理/集群配置/连接集群/使用SSH连接主节点.md)。

2.  编辑install.properties文件。

    ```
    cd /usr/lib/ranger-usersync-current
    vim install.properties
    ```

3.  修改如下配置项。

    ```
    SYNC_SOURCE = ldap
    SYNC_LDAP_URL = ldap://emr-header-1:10389
    SYNC_LDAP_BIND_DN = uid=admin,o=emr
    SYNC_LDAP_BIND_PASSWORD = [password]
    SYNC_LDAP_USER_SEARCH_BASE = ou=people,o=emr
    ```

    **说明：** 当您创建的是高安全集群时，请配置**SYNC\_LDAP\_USER\_SEARCH\_FILTER**为\(!\(cn=\*/\*\)\)，以便于过滤掉OpenLDAP中已创建的组件服务的Kerberos Principal记录。

    本示例给出的配置值均为对接的EMR OpenLDAP的值。如果您需要对接自建的LDAP，需要修改各参数值为自建LDAP对应的值。各参数详细解释，请参见[Ranger Usersync官方安装教程](https://cwiki.apache.org/confluence/display/RANGER/Apache+Ranger+0.5.0+Installation#ApacheRanger0.5.0Installation-InstallingtheRangerUserSyncProcess)。

    |参数|描述|
    |--|--|
    |`SYNC_LDAP_URL`|LDAP服务的地址。例如`ldap://ldap.example.com:389`。|
    |`SYNC_LDAP_BIND_DN`|连接LDAP进行用户和用户组查询的dn。例如`cn=ldapadmin,ou=users,dc=example,dc=com`。|
    |`SYNC_LDAP_BIND_PASSWORD`|用于连接的dn对应的密码。|
    |`EARCH_BASE`|LDAP中用户搜索域。例如`ou=users,dc=example,dc=com`。|

4.  如果您需要同步LDAP用户组至Ranger，请根据LDAP实际信息修改如下参数。

    ```
    SYNC_LDAP_USER_GROUP_NAME_ATTRIBUTE = gitNumber
    SYNC_GROUP_SEARCH_ENABLED = true
    SYNC_GROUP_USER_MAP_SYNC_ENABLED = true
    SYNC_GROUP_SEARCH_BASE = ou=group,o=emr
    SYNC_GROUP_OBJECT_CLASS = posixGroup
    SYNC_GROUP_NAME_ATTRIBUTE = cn
    SYNC_GROUP_MEMBER_ATTRIBUTE_NAME = memberUid
    ```

    |参数|说明|
    |--|--|
    |`SYNC_LDAP_USER_GROUP_NAME_ATTRIBUTE`|用户条目（Entry）中表示用户组属性（Attribute）的名称。例如`gitNumber(user objectClass=posixAccount)`。|
    |`SYNC_GROUP_SEARCH_ENABLED`|是否根据条目（Entry）中记录的用户组属性（Attribute）来确定用户组信息。`true`。|
    |`SYNC_GROUP_USER_MAP_SYNC_ENABLED`|用户与用户组之间的映射关系是否通过LDAP搜索进行确定。例如`true`。|
    |`SYNC_GROUP_SEARCH_BAS`E|LDAP中用户搜索域。例如`ou=groups,dc=example,dc=com`。|
    |`SYNC_GROUP_OBJECT_CLASS`|用户组的对象类（ObjectClass）。例如`posixGroup`。|
    |`SYNC_GROUP_NAME_ATTRIBUTE`|用户组条目（Entry）中用户组名的标识。例如`cn`。|
    |`SYNC_GROUP_MEMBER_ATTRIBUTE_NAME`|用户组条目（Entry）中标识用户组成员的属性（Attribute）名称。例如`memberUid`。|

5.  在emr-header-1节点的/usr/lib/ranger-usersync-current路径下执行`setup.sh`。

    ```
    cd /usr/lib/ranger-usersync-current
    sh setup.sh
    ```

6.  重启Ranger UserSync，使配置生效。

    1.  在左侧导航栏，单击**集群服务** \> **RANGER**。

    2.  在**组件列表**区域，单击**RangerUserSync**所在行的**重启**。

    3.  在**执行集群操作**对话框中，配置各项参数。

    4.  单击**确定**。

    5.  在**确认**对话框中，单击**确定**。


