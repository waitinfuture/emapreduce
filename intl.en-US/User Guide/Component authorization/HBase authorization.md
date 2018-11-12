# HBase authorization {#concept_e5l_2mb_bfb .concept}

Without authorization, any account can perform any operations on the HBase cluster that includes disable table, drop table, major compact, and others.

**Note:** 

For clusters without Kerberos authentication, users can forge identities to access to the cluster service even when HBase authorization is enabled. Therefore, we recommend that you create a cluster with high security mode \(i.e. supporting Kerberos\) as detailed in [Kerberos Security Document](intl.en-US/User Guide/Kerberos authentication/Introduction to Kerberos.md#).

## Add configuration {#section_q4z_qmb_bfb .section}

In Configuration Management, choose **HBase** \> **Configuration** \> **hbase-site** \> **Custom Configuration** in the HBase cluster.

Add the following parameters:

```
<property>
     <name>hbase.security.authorization</name>
     <value>true</value>
</property>
<property>
     <name>hbase.coprocessor.master.classes</name>
     <value>org.apache.hadoop.hbase.security.access.AccessController</value>
</property>
<property>
     <name>hbase.coprocessor.region.classes</name>
 <value>org.apache.hadoop.hbase.security.token.TokenProvider,org.apache.hadoop.hbase.security.access.AccessController</value>
</property>
<property>
  <name>hbase.coprocessor.regionserver.classes</name>
  <value>org.apache.hadoop.hbase.security.access.AccessController,org.apache.hadoop.hbase.security.token.TokenProvider</value>
</property>
```

## Restart the HBase cluster {#section_qvn_knb_bfb .section}

In the HBase cluster Configuration Management page, click **HBase** \> **Operations** \> **RESTART All Components**.

## Authorization \(ACL\) {#section_gmy_pnb_bfb .section}

-   Basic concepts

    Authorization is for grant \[operation permissions\] of \[resources in a certain scope\] to \[a certain entity\].

    In HBase, the preceding three concepts are:

    -   Resources in a certain scope
        -   Superuser

            A Superuser can perform any operations, and the account running HBase service is the Superuser by default. You can also add Superusers through configuring the value of hbase.superuser in hbase-site.xml.

        -   Global

            Global Scope has Admin permissions of all tables in the cluster.

        -   Namespace

            It has permission control in Namespace Scope.

        -   Table

            It has permission control in Table Scope.

        -   ColumnFamily

            It has permission control in ColumnFamily Scope.

        -   Cell

            It has permission control in Cell Scope.

    -   Operation permission
        -   Read \(R\)

            Read data from resources in a certain Scope.

        -   Write \(W\)

            Write data to resources in a certain Scope.

        -   Execute \(X\)

            Execute co-processor in a certain Scope.

        -   Create \(C\)

            Create/delete a table in a certain Scope.

        -   Admin \(A\)

            Perform cluster related operations in a certain Scope, such as balance/assign.

    -   A certain entity
        -   User

            Authorize a user

        -   Group

            Authorize a user group

-   Authorization command
    -   grant

        ```
        grant <user> <permissions> [<@namespace> [<table> [<column family> [<column qualifier>]]]
        ```

        **Note:** 

        -   The authorization methods for users or groups are the same. A prefix @ needs to be added for group.

            ```
            grant 'test','R','tbl1'   #grant the read permission of the table tb11 to the user test.
              grant '@test','R','tbl1'   #grant the read permission of the table tb11 to the user group test.
            ```

        -   A prefix @ needs to be added for namespace.

            ```
            grant 'test 'C','@ns_1'  #grant the create permission of the namespace @ns_1 to the user test.
            ```

    -   revoke
    -   user\_permissions \(view permissions\)

