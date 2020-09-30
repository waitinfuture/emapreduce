# Connect Hue to an LDAP server

If you want to use an account in an LDAP server to access Hue, you must connect Hue to the LDAP server. This topic describes how to connect Hue to EMR OpenLDAP and perform authentication. If you use a user-created LDAP server, modify the configurations based on your business requirements.

1.  Go to the Service Configuration section of Hue.

    1.  Log on to the [EMR console](https://emr.console.aliyun.com/).

    2.  In the top navigation bar, select the region where your cluster resides. Select the resource group as required. By default, all resources of the account appear.

    3.  Click the **Cluster Management** tab.

    4.  On the **Cluster Management** page that appears, find the target cluster and click **Details** in the Actions column.

    5.  In the left-side navigation pane, click **Cluster Service** and then **Hue**.

    6.  Click the **Configure** tab.

    7.  In the **Service Configuration** section, click **hue**.

        ![hue](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/8857341061/p101014.png)

2.  Change the value of **backend** to **desktop.auth.backend.LdapBackend**.

3.  Add custom configurations.

    1.  Click **Custom Configuration** in the upper-right corner. In the Add Configuration Item dialog box, add the parameters described in the following table.

        **Note:** When you add the parameters, set the **desktop.ldap.bind\_password** parameter to the value obtained from the EMR console and set the other parameters to the values provided in the Value column.

        |Parameter|Description|Value|
        |---------|-----------|-----|
        |**desktop.ldap.ldap\_url**|The URL of the LDAP server.|ldap://emr-header-1:10389|
        |**desktop.ldap.bind\_dn**|The distinguished name \(DN\) of the administrator. The DN is used to connect to the LDAP or AD server and query users and user groups. If the LDAP server supports anonymous access, this parameter is not required.|uid=admin,o=emr|
        |**desktop.ldap.bind\_password**|The password of the DN of the administrator. **Note:** You can obtain the password from Service Configuration for the OpenLDAP service in the EMR console. The value of the **manager\_password** parameter is the password.

|None|
        |**desktop.ldap.ldap\_username\_pattern**|The pattern in which a username is matched with an LDAP DN. This parameter must contain **<username\>** to support authentication.|uid=<username\>,ou=people,o=emr|
        |**desktop.ldap.base\_dn**|The base DN that is used to search for users and user groups in the LDAP server.|ou=people,o=emr|
        |**desktop.ldap.search\_bind\_authentication**|Specifies whether to use credentials provided in **desktop.ldap.bind\_dn** and **desktop.ldap.bind\_password** to perform search, binding, and authentication.|false|
        |**desktop.ldap.use\_start\_tls**|Specifies whether to establish a Transport Layer Security \(TLS\) connection with the LDAP server that is specified by an ldap:// URL.|false|
        |**desktop.ldap.create\_users\_on\_login**|Specifies whether to create users in Hue after a user accesses Hue by using LDAP credentials.|true|

    2.  Click **OK**.

4.  Save the configurations.

    1.  In the upper-right corner of the Service Configuration section, click **Save**.

    2.  In the dialog box that appears, turn on **Auto-update Configuration** and specify related information.

    3.  Click **OK**.

5.  Deploy client configurations.

    1.  In the upper-right corner of the Service Configuration section, click **Deploy Client Configuration**.

    2.  Set the required parameters.

    3.  Click **OK**.

6.  In the upper-right corner of the Hue page, select **Restart Hue** from the **Actions** drop-down list.


**Note:** After you connect Hue to the LDAP server, the original admin account cannot be used to access Hue. The new administrator is the first logon user after the LDAP server is connected.

For more information about how to access Hue, see [Hue](/intl.en-US/Cluster types/Hadoop cluster/Hue/Hue.md).

