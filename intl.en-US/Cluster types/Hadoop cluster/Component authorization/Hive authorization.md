# Hive authorization

Hive has two authorization modes: one based on storageand the other based on SQL standards. For more information, see Hive's [Authorization guide](https://cwiki.apache.org/confluence/display/Hive/LanguageManual+Authorization).

**Note:** Both means of authorization can be configured at the same time without conflict.

Storage based authorization \(for Hive metastore\)

If a user in a cluster has direct access to data in Hive through an HDFS or Hive client, a permission control must be performed on Hive data in HDFS. By doing so, operation permissions related to Hive SQL can be controlled.

For more information, see Hive's[Storage Based Authorization](https://cwiki.apache.org/confluence/display/Hive/Storage+Based+Authorization+in+the+Metastore+Server) guide.

## Add configuration

In the cluster Configuration Management page, click **Hive** \> **Configuration** \> **hive-site.xml** \> **Add Custom Configuration**.

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

## Restart Hivemetastore

Restart the Hivemetastore in the cluster's Configuration Management page.

## HDFS permission control

For Kerberos security clusters in E-MapReduce, HDFS permissions for the Hive warehouseare set.

For non-Kerberos security clusters, you must complete the following steps to set the basic HDFS permission:

-   EnableHDFS permissions
-   Configure permissions for the Hive warehouse

    ```
    hadoop fs -chmod 1771 /user/hive/warehouse
    It can be set as follows, in which 1 denotes stick bit (i.e. cannot delete files/folders created by others)
    hadoop fs -chmod 1777 /user/hive/warehouse
    ```


With the basic permission set, users and user groups can create, read, and write tables as usualby authorizing the folder warehouse.

```
sudo su has
      #Grant rwx permission of folder warehouse to user test
      hadoop fs -setfacl -m user:test:rwx /user/hive/warehouse
      #Grant rwx permission of folder warehouse to user hivegrp
      hadoo fs -setfacl -m group:hivegrp:rwx /user/hive/warehouse
```

With HDFS authorized, users and user groups can create, read, and write tables as usual. Data in Hive tables that is created by different users in HDFS can only be accessed by the users themselves.

## Verification

-   The test user creates a table testtbl.

    ```
    hive> create table testtbl(a string);
    FAILED: Execution Error, return code 1 from org.apache.hadoop.hive.ql.exec.DDLTask. MetaException(message:Got exception: org.apache.hadoop.security.AccessControlException Permission denied: user=test, access=WRITE, inode="/user/hive/warehouse/testtbl":hadoop:hadoop:drwxrwx--t)
    at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:320)
    at org.apache.hadoop.hdfs.server.namenode.FSPermissionChecker.check(FSPermissionChecker.java:292)
    ```

    An error occurs due to the lack of permissions. Permissions should be granted to the test user.

    ```
    #Switch from root account to has account
    su has
    #Add ACL and grant rwx permissions of the directory warehouse to the account test.
     hadoop fs -setfacl -m user:test:rwx /user/hive/warehouse
    ```

    The test account recreates the database successfully.

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

-   User foo accesses the table testtbl.

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

    Foo cannot perform operations on the table created by the test user. HDFS authorization is needed to grant permissions to foo.

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

    **Note:** You can create a Hive user group, authorize it, and then add new users it.


## SQL Standard Based Authorization

-   Scenario

    If a cluster user cannot access data in Hive through an HDFS or Hive client, and the only way is to run Hive related commands through `HiveServer (beeline, jdbc, and so on)`, SQL standardbased authorization can be used.

    If users can use the Hive shell or similar methods and as long as hive-site.xml in the user's client has not been configured, Hive can still be accessed as usual, even if the following settings are implemented.

    For more information, see Hive's[SQL Standard Based Authorization](https://cwiki.apache.org/confluence/display/Hive/SQL+Standard+Based+Hive+Authorization) guide.

-   Add configuration
    -   The configuration is provided to HiveServer.
    -   In the cluster Configuration Management page, click **Hive** \> **Configuration** \> **hive-site.xml** \> **Add Custom Configuration**.

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
-   Verification
    -   User foo accessesthe test user'stable, testtbl, through beeline.

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

    -   Foo can select as usual.

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

    -   Revoke permissions.

        ```
        Switch to account test, and revoke the select permission from user foo
          hive> revoke select from user foo;
            OK
            Time taken: 1.094 seconds
        ```

    -   Foo cannot select testtbl data.

        ```
        User foo cannot select data from table testtbl.
        Error: Error while compiling statement: FAILED: HiveAccessControlException Permission denied: Principal [name=foo, type=USER] does not have following privileges for operation QUERY [[SELECT] on Object [type=TABLE_OR_VIEW, name=default.testtbl]] (state=42000,code=40000)
        ```


