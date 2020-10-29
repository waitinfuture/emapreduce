# Integrate Ranger Admin with an LDAP server

This topic describes how to integrate Ranger Admin with an LDAP server. After the integration, you can use an account in the LDAP server to log on to Ranger web UI.

Ranger has both internal users and external users. The LDAP and UNIX users synchronized to Ranger are external users. Users created in the Ranger web UI are internal users. You can grant permissions to both internal and external users in Ranger.

After Ranger Admin is integrated with an LDAP server, a user of the LDAP server can log on to the Ranger web UI. After logon, Ranger automatically creates this user as an external user on the Users page. By default, this user can only view the information of Ranger services and policies. The admin user can upgrade standard users to administrators on the Users page.

## EMR V3.28.0 and later V3.X versions, and EMR V4.3.0 and later

1.  Go to the **Configure** tab for the Ranger service.

    1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

    2.  In the top navigation bar, select the region where your cluster resides. Select the resource group as required. By default, all resources of the account appear.

    3.  Click the **Cluster Management** tab.

    4.  Find your cluster and click **Details** in the **Actions** column.

    5.  In the left-side navigation pane, click **Cluster Service** and then **RANGER**.

    6.  Click the **Configure** tab.

2.  Configure parameters on the **ranger-admin-site** tab.

    1.  In the **Service Configuration** section, click the **ranger-admin-site** tab.

    2.  Configure the parameters listed in the following table to synchronize LDAP users to Ranger.

        |Parameter|Required value|
        |---------|--------------|
        |**ranger.ldap.bind.dn**|uid=admin,o=emr|
        |**ranger.ldap.bind.password**|Value of the **manager\_password** parameter on the Configure tab for the OpenLDAP service|
        |**ranger.ldap.base.dn**|ou=people,o=emr|
        |**ranger.authentication.method**|LDAP|
        |**ranger.ldap.url**|ldap://emr-header-1:10389|
        |**ranger.ldap.user.dnpattern**|uid=\{0\},ou=people,o=emr|

3.  Restart Ranger Admin to make the configurations take effect.

    1.  In the left-side navigation pane, click **Cluster Service** and then **RANGER**.

    2.  In the **Components** section, find the **RangerAdmin** parameter and click **Restart** in the Actions column.

    3.  In the **Cluster Activities** dialog box, configure the parameters.

    4.  Click **OK**.

    5.  In the **Confirm** message, click **OK**.


## EMR V3.X versions earlier than V3.28.0 and EMR V4.X versions earlier than V4.3.0

1.  Log on to the emr-header-1 node of the cluster. For more information, see [Connect to the master node of an EMR cluster in SSH mode](/intl.en-US/Cluster Management/Configure clusters/Connect to a cluster/Connect to the master node of an EMR cluster in SSH mode.md).

2.  Open the install.properties file.

    ```
    cd /usr/lib/ranger-usersync-current
    vim install.properties
    ```

3.  Configure the following information in the file:

    ```
    authentication_method = LDAP
    xa_ldap_url = ldap://emr-header-1:10389
    xa_ldap_userDNpattern = uid={0},ou=people,o=emr
    xa_ldap_base_dn = ou=people,o=emr
    xa_ldap_bind_dn = uid=admin,o=emr
    xa_ldap_bind_password = [password]
    ```

    The preceding example demonstrates the integration of EMR OpenLDAP. If you integrate Ranger Admin with a user-created LDAP server, you must configure the parameters based on the description in the following table. For more information about the parameters, see the [official Ranger Admin installation guide](https://cwiki.apache.org/confluence/display/RANGER/Apache+Ranger+0.5.0+Installation#ApacheRanger0.5.0Installation-InstallStepsforRangerPolicyAdminonRHEL/CentOS).

    |Parameter|Description|
    |---------|-----------|
    |`xa_ldap_url`|The URL of the LDAP service. Example: `ldap://ldap.example.com:389`.|
    |`xa_ldap_userDNpattern`|The pattern that matches a logon user with an LDAP distinguished name \(DN\). For example, if the value of this parameter is `uid={0},ou=users,dc=example,dc=com` and the logon user is hadoop, the LDAP DN is `uid=hadoop,ou=users,dc=example,dc=com`.|
    |`xa_ldap_base_dn`|The user search domain in the LDAP server. Example: `ou=users,dc=example,dc=com`.|
    |`xa_ldap_bind_dn`|The distinguished name \(DN\) used to connect the LDAP server to query users and user groups. Example: `cn=ldapadmin,ou=users,dc=example,dc=com`.|
    |`xa_ldap_bind_password`|The password of the DN that is used to connect to the LDAP server.|

4.  Run the `setup.sh` command in the /usr/lib/ranger-usersync-current directory of the emr-header-1 node.

    ```
    cd /usr/lib/ranger-usersync-current
    sh setup.sh
    ```

5.  Restart Ranger Admin to make the configurations take effect.

    1.  In the left-side navigation pane, click **Cluster Service** and then **RANGER**.

    2.  In the **Components** section, find the **RangerAdmin** parameter and click **Restart** in the Actions column.

    3.  In the **Cluster Activities** dialog box, configure the parameters.

    4.  Click **OK**.

    5.  In the **Confirm** message, click **OK**.


