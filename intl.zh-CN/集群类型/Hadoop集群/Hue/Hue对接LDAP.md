# Hue对接LDAP

当您使用LDAP管理用户账号，访问Hue时需进行LDAP相关的配置。本文以Hue对接E-MapReduce自带的OpenLDAP为例，介绍如何配置Hue后端对接LDAP，并通过LDAP进行身份验证。自建的LDAP请您根据实际情况修改参数。

1.  进入服务配置。

    1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

    3.  单击上方的**集群管理**页签。

    4.  在**集群管理**页面，单击相应集群所在行的**详情**。

    5.  在左侧导航栏中，单击**集群服务** \> **Hue**。

    6.  单击**配置**页签。

    7.  在**服务配置**区域，单击**hue**。

        ![hue](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9447459951/p101014.png)

2.  修改**backend**的值为**desktop.auth.backend.LdapBackend**。

3.  增加自定义配置。

    1.  单击右上角的**自定义配置**，添加如下配置项。

        **说明：** 增加下表的自定义配置时，只有**desktop.ldap.bind\_password**参数需要在E-MapReduce控制台获取，其余参数请按表格中给出的参数值填写。

        |配置项|说明|参数值|
        |---|--|---|
        |**desktop.ldap.ldap\_url**|LDAP服务器的URL。|ldap://emr-header-1:10389|
        |**desktop.ldap.bind\_dn**|绑定的管理员用户dn，该dn用于连接到LDAP或AD以搜索用户和用户组信息。如果LDAP服务器支持匿名绑定，则此项可不设置。|uid=admin,o=emr|
        |**desktop.ldap.bind\_password**|绑定的管理员用户dn的密码。 **说明：** 需要在E-MapReduce控制台上，通过OpenLDAP的服务配置获取**manager\_password**的值，即为密码。

|无|
        |**desktop.ldap.ldap\_username\_pattern**|LDAP用户名dn匹配模式，描述了username如何对应到LDAP中的dn。必须包含**<username\>**字符串才能在身份验证期间用于替换。|uid=<username\>,ou=people,o=emr|
        |**desktop.ldap.base\_dn**|用于搜索LDAP用户名及用户组的base dn。|ou=people,o=emr|
        |**desktop.ldap.search\_bind\_authentication**|是否使用**desktop.ldap.bind\_dn**和**desktop.ldap.bind\_password**配置中提供的凭据，搜索绑定身份验证连接到LDAP服务器。|false|
        |**desktop.ldap.use\_start\_tls**|是否尝试与用ldap://指定的LDAP服务器建立TLS连接。|false|
        |**desktop.ldap.create\_users\_on\_login**|用户尝试以LDAP凭据登录后，是否在Hue中创建用户。|true|

    2.  单击**确定**。

4.  保存配置。

    1.  单击右上角的**保存**。

    2.  开启**自动更新配置**并设置相关信息。

    3.  单击**确定**。

5.  部署配置。

    1.  单击右上角的**部署客户端配置**。

    2.  设置相关信息。

    3.  单击**确定**。

6.  单击右上角的**操作** \> **重启Hue**。


**说明：** 对接LDAP之后，原有的管理员账号admin已经不能登录，新的管理员用户为对接LDAP之后第一个登录的用户。

访问Hue，请参见[使用说明](/intl.zh-CN/集群类型/Hadoop集群/Hue/Hue 使用说明.md)。

