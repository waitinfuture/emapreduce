# OpenLDAP

In E-MapReduce \(EMR\) V3.22.0 and later, OpenLDAP is enabled by default. EMR allows you to integrate Knox with OpenLDAP and use OpenLDAP to manage information about Knox users.

An EMR cluster is created, and OpenLDAP is selected from the optional services during the cluster creation. For more information, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

## View node information

1.  Log on to the [Alibaba Cloud EMR console](https://emr.console.aliyun.com/).

2.  In the top navigation bar, select the region where your cluster resides. Select the resource group as required. By default, all resources of the account appear.

3.  Click the **Cluster Management** tab.

4.  Find the cluster whose node information you want to view and click **Details** in the **Actions** column.

5.  In the left-side navigation pane, click **Cluster Service** and then **OpenLDAP**.

6.  Click the **Component Deployment** tab.

    OpenLDAP is deployed on the master node. You can view the node information on the Component Deployment tab. For a high-availability cluster, OpenLDAP is deployed on two master nodes to ensure high availability.


## Modify OpenLDAP information

-   Method 1: Modify OpenLDAP information in a cluster in the EMR console.

    On the **User Management** page for a cluster, set a Knox account to add or remove OpenLDAP information to or from the cluster. For more information, see [Manage users](/intl.en-US/Cluster Management/Cluster planning/Manage users.md).

-   Method 2: Run the `ldap` command to modify OpenLDAP information in a cluster.

    For example, add OpenLDAP information with uid set to arch and userPassword set to 12345678.

    1.  In the EMR console, obtain the root distinguished name \(DN\) and password from the Service Configuration section on the **Configure** tab for OpenLDAP.

        ![OpenLDAP](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3307917951/p93012.png)

    2.  Log on to the master node of the cluster and edit the arch.ldif file.

        ```
        dn: uid=arch,ou=people,o=emr
        cn: arch
        sn: arch
        objectClass: inetOrgPerson
        userPassword: 12345678
        uid: arch
        ```

    3.  Add the following OpenLDAP information to the file:

        ```
        ldapadd -H ldap://emr-header-1:10389 -f arch.ldif -D uid=admin,o=emr -w ${rootDnPW}
        ```

        -   `${rootDnPW}`: the password of the root DN
        -   `10389`: the listening port of the OpenLDAP service
    4.  Run the following command to view the OpenLDAP information:

        ```
        ldapsearch -w ${rootDnPW}  -D "uid=admin,o=emr" -H ldap://emr-header-1:10389 -b uid=arch,ou=people,o=emr
        ```

        If you want to remove the added OpenLDAP information, run the following command:

        ```
        ldapdelete -x -D "uid=admin,o=emr" -w ${rootDnPW} -r uid=arch,ou=people,o=emr -H ldap://emr-header-1:10389
        ```


