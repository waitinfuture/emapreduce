# Hive 授权 {#concept_ssg_l1b_bfb .concept}

Hive 内置有基于底层 HDFS 的权限（Storage Based Authorization）和基于标准 SQL 的 grant 等命令（SQL Standards Based Authorization）两种授权机制。

**说明：** 

两种授权机制可以同时配置，不冲突。

Storage Based Authorization （针对 HiveMetaStore）

场景：

如果集群的用户直接通过 HDFS/Hive Client 访问 Hive 的数据，需要对 Hive 在 HDFS 中的数据进行相关的权限控制，通过 HDFS 权限控制，进而可以控制 Hive SQL 相关的操作权限。详见[Hive 官方文档](https://cwiki.apache.org/confluence/display/Hive/Storage+Based+Authorization+in+the+Metastore+Server)

## 添加配置 {#section_qpw_vcb_bfb .section}

在集群的配置管理页面选择**Hive** \> **配置** \> **hive-site.xml** \> **自定义配置**

```
<property>
<name>hive.metastore.pre.event.listeners</name>
    <value>org.apache.hadoop.hive.ql.security.authorization.AuthorizationPreEventListener</value>
</property>
<property>
<name>hive.security.metastore.authorization.manager</name>
    <value>org.apache.hadoop.hive.ql.security.authorization.StorageBasedAuthorizationProvider</value>
</property>
<property>
<name>hive.security.metastore.authenticator.manager</name>
    <value>org.apache.hadoop.hive.ql.security.HadoopDefaultMetastoreAuthenticator</value>
</property>
```

## 重启 HiveMetaStore {#section_wv5_pfb_bfb .section}

在集群配置管理页面重启 HiveMetaStore

## HDFS 权限控制 {#section_ibr_qfb_bfb .section}

EMR 的 Kerberos 安全集群已经设置了 Hive 的 warehouse 的 HDFS 相关权限。

对于非 Kerberos 安全集群，用户需要做如下步骤设置 hive 基本的 HDFS 权限：

-   打开 HDFS 的权限
-   配置 Hive 的 warehouse 权限

    ```
    hadoop fs -chmod 1771 /user/hive/warehouse
    也可以设置成，1 表示 stick bit(不能删除别人创建的文件/文件夹)
    hadoop fs -chmod 1777 /user/hive/warehouse
    ```


有了上述设置基础权限后，可以通过对 warehouse 文件夹授权，让相关用户/用户组能够正常创建表/读写表等：

```
sudo su has
      #授予test对warehouse文件夹rwx权限
      hadoop fs -setfacl -m user:test:rwx /user/hive/warehouse
      #授予hivegrp对warehouse文件夹rwx权限
      hadoo fs -setfacl -m group:hivegrp:rwx /user/hive/warehouse
```

经过 HDFS 的授权后，让相关用户/用户组能够正常创建表/读写表等，而且 不同的账号创建的 hive 表在 HDFS 中的数据只能自己的账号才能访问。

## 验证 {#section_abz_ggb_bfb .section}

-   test 用户建表 testtbl

    ```
    hive> create table testtbl(a string);
    FAILED: Execution Error, return code 1 from org.apache.hadoop.hive.ql.exec.DDLTask. MetaException(message:Got exception: org.apache.hadoop.security.AccessControlException Permission denied: user=test, access=WRITE, inode="/user/hive/warehouse/testtbl":hadoop:hadoop:drwxrwx--t
    at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:320)
    at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:292)
    ```

    上面显示错误没有权限，需要给 test 用户添加权限

    ```
    #从root账号切到has账号
    su has
    #给test账号添加acl，对warehouse目录的rwx权限
     hadoop fs -setfacl -m user:test:rwx /user/hive/warehouse
    ```

    test 账号再创建 database，成功

    ```
    hive> create table testtbl(a string);
    OK
    Time taken: 1.371 seconds
    #查看hdfs中testtbl的目录，从权限可以看出test用户创建的表数据只有test和hadoop组可以读取，其他用户没有任何权限
    hadoop fs -ls /user/hive/warehouse
    drwxr-x---   - test hadoop          0 2017-11-25 14:51 /user/hive/warehouse/testtbl
    #插入一条数据
    hive>insert into table testtbl select "hz"
    ```

-   foo 用户访问 testtbl

    ```
    #drop table
    hive> drop table testtbl;
    FAILED: Execution Error, return code 1 from org.apache.hadoop.hive.ql.exec.DDLTask. MetaException(message:Permission denied: user=foo, access=READ, inode="/user/hive/warehouse/testtbl":test:hadoop:drwxr-x---
        at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:320)
        at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.checkPermission(FSPermissionChecker.java:219)
        at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.checkPermission(FSPermissionChecker.java:190)
    #alter table
    hive> alter table testtbl add columns(b string);
    FAILED: SemanticException Unable to fetch table testtbl. java.security.AccessControlException: Permission denied: user=foo, access=READ, inode="/user/hive/warehouse/testtbl":test:hadoop:drwxr-x---
        at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:320)
        at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.checkPermission(FSPermissionChecker.java:219)
        at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.checkPermission(FSPermissionChecker.java:190)
        at org.apache.hadoop.hdfs.server.namenode.FSDirectory.checkPermission(FSDirectory.java:1720)
    #select
    hive> select * from testtbl;
    FAILED: SemanticException Unable to fetch table testtbl. java.security.AccessControlException: Permission denied: user=foo, access=READ, inode="/user/hive/warehouse/testtbl":test:hadoop:drwxr-x---
        at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:320)
        at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.checkPermission(FSPermissionChecker.java:219)
    ```

    可见 foo 用户不能对 test 用户创建的表做任何的操作，如果想要授权给 foo，需要通过 HDFS 的授权来实现

    ```
    su has
    #只授权读的权限，也可以根据情况授权写权限(比如alter)
    #备注: -R 将testtbl文件夹下的文件也设置可读
    hadoop fs -setfacl -R -m user:foo:r-x /user/hive/warehouse/testtbl
    #可以select成功
    hive> select * from testtbl;
    OK
    hz
    Time taken: 2.134 seconds, Fetched: 1 row(s)
    ```

    **说明：** 一般可以根据需求新建一个 hive 用户的 group，然后通过给 group 授权，后续将新用户添加到 group 中，同一个 group 的数据权限都可以访问。


## SQL Standards Based Authorization {#section_z3c_g3b_bfb .section}

-   场景

    如果集群的用户不能直接通过 hdfs/hive client 访问，只能通过 `HiveServer（beeline/jdbc） 等)`来执行 hive 相关的命令，可以使用 SQL Standards Based Authorization 的授权方式。

    如果用户能够使用 hive shell 等方式，即使做下面的一些设置操作，只要用户客户端的 hive-site.xml 没有相关配置，都可以正常访问 hive。

    详见 [Hive文档](https://cwiki.apache.org/confluence/display/Hive/SQL+Standard+Based+Hive+Authorization)

-   添加配置
    -   配置是提供给 HiveServer
    -   在集群与服务管理页面选择 **Hive** \> **配置** \> **hive-site.xml** \> **自定义配置** 

        ```
        <property>
        <name>hive.security.authorization.enabled</name>
         <value>true</value>
        </property>
         <property>
        <name>hive.users.in.admin.role</name>
         <value>hive</value>
        </property>
         <property>
        <name>hive.security.authorization.createtable.owner.grants</name>
         <value>ALL</value>
        </property>
        ```

-   重启 HiveServer2

    在集群配置管理页面重启 HiveServer2

-   权限操作命令

    具体命令操作详见[Hive 文档](https://cwiki.apache.org/confluence/display/Hive/SQL+Standard+Based+Hive+Authorization)

-   验证
    -   用户 foo 通过 beeline 访问 test 用户的 testtbl 表

        ```
        2: jdbc:hive2://emr-header-1.cluster-xxx:10> select * from testtbl;
        Error: Error while compiling statement: FAILED: HiveAccessControlException Permission denied: Principal [name=foo, type=USER] does not have following privileges for operation QUERY [[SELECT] on Object [type=TABLE_OR_VIEW, name=default.testtbl]] (state=42000,code=40000)
        ```

    -   grant 权限

        ```
        切到 test 账号执行 grant 给 foo 授权 select 操作
        hive> grant select on table testtbl to user foo;
         OK
         Time taken: 1.205 seconds
        ```

    -   foo 可以正常 select

        ```
        0: jdbc:hive2://emr-header-1.cluster-xxxxx:10> select * from testtbl;
        INFO  : OK
        +------------+--+
        | testtbl.a  |
        +------------+--+
        | hz         |
        +------------+--+
        1 row selected (0.787 seconds)
        ```

    -   回收权限

        ```
        切换到 test 账号，回收权限 foo 的 select 权限
          hive> revoke select from user foo;
            OK
            Time taken: 1.094 seconds
        ```

    -   foo 无法 select testtbl 的数据

        ```
        0: jdbc:hive2://emr-header-1.cluster-xxxxx:10> select * from testtbl;
        Error: Error while compiling statement: FAILED: HiveAccessControlException Permission denied: Principal [name=foo, type=USER] does not have following privileges for operation QUERY [[SELECT] on Object [type=TABLE_OR_VIEW, name=default.testtbl]] (state=42000,code=40000)
        ```


