# OpenLDAP

EMR-3.22.0及之后版本，默认启动了OpenLDAP服务。E-MapReduce支持Knox与OpenLDAP集成，您可以通过OpenLDAP管理Knox用户信息。

已创建E-MapReduce集群，并且选择了OpenLDAP服务。详情请参见[创建集群](/intl.zh-CN/集群管理/集群配置/创建集群.md)。

## 查看节点信息

1.  已通过主账号登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com)。

2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

3.  单击上方的**集群管理**页签。

4.  在**集群管理**页面，单击集群右侧的**详情**。

5.  在左侧导航栏，单击**集群服务** \> **OpenLDAP**。

6.  单击**部署拓扑**。

    您可以查看OpenLDAP的节点信息，OpenLDAP部署在Master节点。如果是高可用集群，OpenLDAP部署在两个Master节点。


## 修改OpenLDAP信息

-   方式一：在E-MapReduce控制台修改集群中的OpenLDAP信息。

    在**用户管理**页面，通过设置Knox账户，添加或删除集群中的LDAP信息，详细信息请参见[管理用户](/intl.zh-CN/集群管理/集群规划/管理用户.md)。

-   方式二：通过`ldap`命令修改集群中的OpenLDAP信息。

    例如，添加uid为arch，密码为12345678的LDAP信息。

    1.  在E-MapReduce控制台，OpenLDAP服务的**配置**页签，获取root dn和密码。

        ![OpenLDAP](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/4207917951/p93012.png)

    2.  登录集群的Master节点，编辑arch.ldif文件。

        ```
        dn: uid=arch,ou=people,o=emr
        cn: arch
        sn: arch
        objectClass: inetOrgPerson
        userPassword: 12345678
        uid: arch
        ```

    3.  添加LDAP信息。

        ```
        ldapadd -H ldap://emr-header-1:10389 -f arch.ldif -D uid=admin,o=emr -w ${rootDnPW}
        ```

        -   `${rootDnPW}`为root dn的密码。
        -   `10389`为OpenLDAP服务的监听端口。
    4.  查看到该LDAP信息。

        ```
        ldapsearch -w ${rootDnPW}  -D "uid=admin,o=emr" -H ldap://emr-header-1:10389 -b uid=arch,ou=people,o=emr
        ```

        删除添加的LDAP信息。

        ```
        ldapdelete -x -D "uid=admin,o=emr" -w ${rootDnPW} -r uid=arch,ou=people,o=emr -H ldap://emr-header-1:10389
        ```


