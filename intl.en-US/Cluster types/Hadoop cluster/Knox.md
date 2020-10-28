# Knox

This topic describes how to use Knox in EMR.

An EMR cluster is created, and Knox is selected from the optional services during the cluster creation. For more information, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

## Preparations

-   Configure security group access

    1.  Obtain the public IP address of your on-premises machine.

        For security purposes, we recommend that you only allow access from the current public IP address when you configure a security group policy. To obtain your current public IP address, visit [ip.taobao.com](http://ip.taobao.com/). You can view your public IP address in the lower-left corner.

    2.  Enable port 8443:
        1.  In the **Network Info** section of the **Cluster Overview** page, take note of the network type of the cluster and click the ID of the security group.
        2.  Click **Add Security Group Rule**.
        3.  Set **Port Range** to **8443/8443**.
        4.  Set **Authorization Object** to the public IP address obtained in [Step a](#step_01).
        5.  Click **OK**.
    **Note:**

    -   To prevent attacks from external users, you are not allowed to set **Authorization Object** to **0.0.0.0/0**.
    -   If no public IP address is assigned to the cluster during cluster creation, you can add a public IP address to the cluster in the ECS console. After the IP address is added, go back to the EMR console. In the left-side navigation pane of the Cluster Overview page, click **Instances**. In the upper-right corner of the Instances page, click **Update Instance Info** to immediately synchronize instance information.
    -   After the public IP address is assigned, you must [submit a ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex) to bind the domain name with the public IP address.
-   Set a Knox account

    When you access Knox, you must enter your username and password. The authentication is based on LDAP. You can use the LDAP service of Apache Directory Server in the cluster or your own LDAP service.

    -   Use the LDAP service of Apache Directory Server in the cluster

        Method 1 \(recommended\):

        Add a Knox account on the Users page. For more information, see [Manage users](/intl.en-US/Cluster Management/Cluster planning/Manage users.md).

        Method 2:

        1.  Log on to the cluster in SSH mode. For more information, see [Connect to the master node of an EMR cluster in SSH mode](/intl.en-US/Cluster Management/Configure clusters/Connect to a cluster/Connect to the master node of an EMR cluster in SSH mode.md).
        2.  Prepare your username, such as Tom.

            Run the following commands to open the users.ldif file:

            ```
            su knox
            cd /usr/lib/knox-current/templates  
            vi users.ldif
            ```

            In the file, replace all `emr-guest` with `Tom` and `EMR GUEST` with `Tom`, and set setPassword to the password of your username.

        3.  Run the following commands to import user data to LDAP:

            ```
            su knox
            cd /usr/lib/knox-current/templates
            sh ldap-sample-users.sh
            ```

    -   Use your own LDAP service
        1.  Go to the **Configure** tab of the Knox service. Click the **cluster-topo** tab in the Service Configuration section.
        2.  Configure the parameters in the **xml-direct-to-file-content** field.

            |Parameter|Description|
            |---------|-----------|
            |`main.ldapRealm.userDnTemplate`|Specifies a distinguished name \(DN\) template.|
            |`main.ldapRealm.contextFactory.url`|Specifies the domain name and port number of your LDAP server.|

            ![cluster-topo](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2511883061/p134502.png)

        3.  In the upper-right corner of the Service Configuration section, click **Save**.
        4.  In the **Confirm Changes** dialog box, configure the parameters and click **OK**.
        5.  Select **Restart Knox** from the **Actions** drop-down list in the upper-right corner.
        6.  In the **Cluster Activities** dialog box, configure the parameters and click **OK**.

            In the **Confirm** message, click **OK**.

        7.  Enable the Knox port, such as port 10389, to access the LDAP service over the Internet.

            The steps to enable this port are similar to those to enable port 8443. Note that you must set Rule Direction to **Outbound**.


## Access the Knox web UI

You can use your Knox account to access the web UIs of other services, such as HDFS, YARN, Spark, and Ganglia.

-   Access a service in the EMR console
    1.  Log on to the [Alibaba Cloud EMR console](https://emr.console.aliyun.com/).
    2.  In the top navigation bar, select the region where your cluster resides. Select the resource group as required. By default, all resources of the account appear.
    3.  Click the **Cluster Management** tab.
    4.  On the **Cluster Management** page, find the cluster whose services you want to access, and click **Details** in the Actions column.
    5.  In the left-side navigation pane of the Cluster Overview page, click **Connect Strings**.
    6.  On the **Public Connect Strings** page, click the URL of the service that you want to access.
-   Access a service by using the public IP address of the cluster from your browser
    1.  On the **Cluster Overview** page, obtain the public IP address of your cluster.
    2.  In the address bar of the browser, enter the URL of the service that you want to access and press Enter.
        -   HDFS: https://\{Public IP address of the cluster\}:8443/gateway/cluster-topo/hdfs/
        -   Yarn: https://\{Public IP address of the cluster\}:8443/gateway/cluster-topo/yarn/
        -   Spark History: https://\{Public IP address of the cluster\}:8443/gateway/cluster-topo/sparkhistory/
        -   Ganglia: https://\{Public IP address of the cluster\}:8443/gateway/cluster-topo/ganglia/
        -   Storm: https://\{Public IP address of the cluster\}:8443/gateway/cluster-topo/storm/
        -   Oozie: https://\{Public IP address of the cluster\}:8443/gateway/cluster-topo/oozie/

## Access control

Knox offers service-level access control. You can manage access permissions on a specific service by user, user group, or IP address. For more information, see [Apache Knox authorization](https://knox.apache.org/books/knox-0-13-0/user-guide.html?spm=a2c4g.11186623.2.14.459af364CUTH7M#Authorization).

Example:

-   Scenario: Authorize only user Tom to access the YARN web UI.
-   Procedure:

    1.  Go to the **Configure** tab of the Knox service. Click the **cluster-topo** tab in the Service Configuration section.
    2.  Configure the parameters in the **xml-direct-to-file-content** field.

        Add the following access control code between the `<gateway> and </gateway>` labels.

        ```
        <provider>
              <role>authorization</role>
              <name>AclsAuthz</name>
              <enabled>true</enabled>
              <param>
                  <name>YARNUI.acl</name>
                  <value>Tom;*;*</value>
              </param>
        </provider>
        ```

    3.  In the upper-right corner of the Service Configuration section, click **Save**.
    4.  In the **Confirm Changes** dialog box, configure the parameters and click **OK**.
    5.  Select **Restart Knox** from the **Actions** drop-down list in the upper-right corner.
    6.  In the **Cluster Activities** dialog box, configure the parameters and click **OK**.

        In the **Confirm** message, click **OK**.

    **Warning:** Knox allows you to use the RESTful API of a service to perform related operations on the service. For example, you can use the RESTful API of HDFS to add files to or remove files from HDFS. To ensure security, you are not allowed to use the LDAP username and password saved in the Knox software directory to access a service.


