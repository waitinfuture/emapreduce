# HBase authorization {#concept_e5l_2mb_bfb .concept}

Any account can perform any operation on an HBase cluster without being authorized, including disabling and dropping tables and performing major compaction.

**Note:** 

For clusters that do not have Kerberos authentication, users can forge identities to access the cluster service, even when HBase authorization is enabled. Therefore, we recommend that you create a high-security cluster \(for example, supporting Kerberos\) as detailed in the [Introduction to Kerberos](reseller.en-US/User Guide/Kerberos authentication/Introduction to Kerberos.md#).

## Add configuration {#section_q4z_qmb_bfb .section}

In the Configuration Management page, choose **HBase** \> **Configuration** \> **hbase-site** \> **Custom Configuration** in the HBase cluster.

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

In the HBase cluster's Configuration Management page, click **HBase** \> **Operations** \> **RESTART All Components**.

## Authorization \(ACL\) {#section_gmy_pnb_bfb .section}

-   Basic concepts

    In HBase, authorization consists of three elements: the granting of **operational permissions** for a certain **scope of resources** to a certain **entity**.

    -   Resources in a certain scope
        -   Superuser

            A superuser can perform any operations. The account that runs the HBase service is the superuser by default. You can also add superusers by configuring the value of hbase.superuser in hbase-site.xml.

        -   Global

            Global Scope has administrator permissions for all tables in the cluster.

        -   Namespace

            This has permission control in Namespace Scope.

        -   Table

            This has permission control in Table Scope.

        -   ColumnFamily

            This has permission control in ColumnFamily Scope.

        -   Cell

            This has permission control in Cell Scope.

    -   Operational permissions
        -   Read \(R\)

            Read data from resources in a certain scope.

        -   Write \(W\)

            Write data to resources in a certain scope.

        -   Execute \(X\)

            Execute co-processor in a certain scope.

        -   Create \(C\)

            Create or delete a table in a certain scope.

        -   Admin \(A\)

            Perform cluster-related operations in a certain scope, such as balance or assign.

    -   Entity
        -   User

            Authorize a user.

        -   Group

            Authorize a user group.

-   Authorization command
    -   grant

        ```
        grant <user> <permissions> [<@namespace> [<table> [<column family> [<column qualifier>]]]
        ```

        **Note:** 

        -   The authorization methods for users and groups are the same. The prefix @ needs to be added for groups.

            ```
            grant 'test','R','tbl1'   #grant the read permission of the table tb11 to the user test.
              grant '@test','R','tbl1'   #grant the read permission of the table tb11 to the user group test.
            ```

        -   The prefix @ needs to be added for namespace.

            ```
            grant 'test 'C','@ns_1'  #grant the create permission of the namespace @ns_1 to the user test.
            ```

    -   revoke
    -   user\_permissions \(view permissions\)

