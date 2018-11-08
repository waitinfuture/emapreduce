# Hive authorization {#concept_ssg_l1b_bfb .concept}

Two authorization features are built in Hive:

-   Storage Based Authorization
-   SQL Standards Based Authorization

For more information, see [Hive official documentation](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Authorization).

**Note:** The two authorization features can be configured at the same time without conflict.

Storage Based Authorization \(for HiveMetaStore\)

Scenario:

If a user in the cluster has a direct access to data in Hive through HDFS/Hive Client, a permission control must be performed on Hive data in HDFS. Through HDFS permission control, operation permissions related to Hive SQL can be controlled.

For more information, see [Hive document](https://cwiki.apache.org/confluence/display/Hive/Storage+Based+Authorization+in+the+Metastore+Server).

## Add configuration {#section_qpw_vcb_bfb .section}

In the cluster Configuration Management page, **Hive** \> **Configuration** \> **hive-site.xml** \> **Add Custom Configuration**.

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

## Restart HiveMetaStore {#section_wv5_pfb_bfb .section}

Restart HiveMetaStore in the cluster Configuration Management page.

## HDFS permission control {#section_ibr_qfb_bfb .section}

HDFS related permissions of warehouse in Hive has been set for Kerberos security cluster in the EMR.

For non-Kerberos security cluster, users must set basic HDFS permission through the following steps:

-   EnableHDFS permissions
-   Configure permissions of warehouse in Hive

    ```
    hadoop fs -chmod 1771 /user/hive/warehouse
    It can be set as follows, in which 1 denotes stick bit (i.e. cannot delete files/folders created by others)
    hadoop fs -chmod 1777 /user/hive/warehouse
    ```


With the basic permission set \(mentioned earlier\), related users/user groups can normally create/read/write tables through authorizing the folder warehouse.

```
sudo su has
      #Grant rwx permission of folder warehouse to user test
      hadoop fs -setfacl -m user:test:rwx /user/hive/warehouse
      #Grant rwx permission of folder warehouse to user hivegrp
      hadoo fs -setfacl -m group:hivegrp:rwx /user/hive/warehouse
```

With the HDFS authorization, related users/user groups can normally create/read/write tables, and data in Hive tables created by different users in HDFS can only be accessed by the users themselves.

## Verification {#section_abz_ggb_bfb .section}

-   User test creates a table testtbl.

    ```
    hive> create table testtbl(a string);
    FAILED: Execution Error, return code 1 from org.apache.hadoop.hive.ql.exec.DDLTask. MetaException(message:Got exception: org.apache.hadoop.security.AccessControlException Permission denied: user=test, access=WRITE, inode="/user/hive/warehouse/testtbl":hadoop:hadoop:drwxrwx--t
    at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:320)
    at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:292)
    ```

    An error occurs due to no permissions. Permissions should be granted to the user test.

    ```
    #Switch from root account to has account
    su has
    #Add ACL and grant rwx permissions of the directory warehouse to the account test.
     hadoop fs -setfacl -m user:test:rwx /user/hive/warehouse
    ```

    The account test recreates the database successfully.

    ```
    hive> create table testtbl(a string);
    OK
    Time taken: 1.371 seconds
    #View the directory of testtbl in HDFS. From the permissions it can be seen that only the groups test and hadoop can read data from the table created by the user test, while other users have no permissions
    hadoop fs -ls /user/hive/warehouse
    drwxr-x---   - test hadoop          0 2017-11-25 14:51 /user/hive/warehouse/testtbl
    #Insert a record
    hive>insert into table testtbl select "hz"
    ```

-   User foo accesses to table testtbl.

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

    It can be seen that the user foo cannot perform any operations on the table created by the user test. HDFS authorization is needed to grant permissions to foo.

    ```
    su has
    #Only read permission is granted, and write permission can also be granted as needed (for example, alter)
    #Note: -R: Set files in the folder testtbl to readable
    hadoop fs -setfacl -R -m user:foo:r-x /user/hive/warehouse/testtbl
    #The table can be selected successfully
    hive> select * from testtbl;
    OK
    hz
    Time taken: 2.134 seconds, Fetched: 1 row(s)
    ```

    **Note:** In general, a Hive user group can be created and authorized, then add new users the group.


## SQL Standards Based Authorization {#section_z3c_g3b_bfb .section}

-   Scenario

    If a cluster user can’t access through a HDFS/Hive client, and the only way is to run Hive related commands through `HiveServer (beeline, jdbc, and so on)`. SQL Standards Based Authorization can be used.

    If you are able to use methods such as Hive shell, as long as no related configuration has been made to the hive-site.xml in the user’s client, Hive can still be normally accessed even if the following settings are implemented.

    For more information, see [Hive documentation](https://cwiki.apache.org/confluence/display/Hive/SQL+Standard+Based+Hive+Authorization).

-   Add configuration
    -   The configuration is provided to HiveServer.
    -   In the cluster Configuration Management page, click **Hive** \> **Configuration** \> **hive-site.xml** \> **Add Custom Configuration**

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

-   Restart HiveServer2

    Restart HHiveServer2 in the cluster Configuration Management page.

-   Permission operation commands

    For detailed command operations, click [here](https://cwiki.apache.org/confluence/display/Hive/SQL+Standard+Based+Hive+Authorization).

-   Verification
    -   User foo access to user test’s table testtbl through beeline.

        ```
        2: jdbc:hive2://emr-header-1.cluster-xxx:10> select * from testtbl;
        Error: Error while compiling statement: FAILED: HiveAccessControlException Permission denied: Principal [name=foo, type=USER] does not have following privileges for operation QUERY [[SELECT] on Object [type=TABLE_OR_VIEW, name=default.testtbl]] (state=42000,code=40000)
        ```

    -   Grant permissions.

        ```
        Switch to account test to grant select permission to user foo
        hive> grant select on table testtbl to user foo;
         OK
         Time taken: 1.205 seconds
        ```

    -   User foo can normally select.

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

    -   Revoke permission.

        ```
        Switch to account test, and revoke the select permission from user foo
          hive> revoke select from user foo;
            OK
            Time taken: 1.094 seconds
        ```

    -   Foo could not select data for testtbl

        ```
        User foo cannot select data from table testtbl.
        Error: Error while compiling statement: FAILED: HiveAccessControlException Permission denied: Principal [name=foo, type=USER] does not have following privileges for operation QUERY [[SELECT] on Object [type=TABLE_OR_VIEW, name=default.testtbl]] (state=42000,code=40000)
        ```


