# Knox guide {#concept_knp_s1x_y2b .concept}

Currently E-MapReduce supports [Apache Knox](https://knox.apache.org/). If you select the Knox-supported image to create a cluster, you can directly access the Web UI from the public network to use services such as YARN, HDFS, and SparkHistory after completing the following preparations.

## Preparations {#section_m1v_wkl_z2b .section}

-   Enable Knox access using a public IP address

    1.  The service port of Knox on E-MapReduce is 8443. In the cluster details, find the ECS security group in which the cluster is located.
    2.  Change the corresponding security group in the ECS console and add a rule in **Internet inbound** to enable Port 8443.
    **Note:** 

    -   For security, the authorization object must be your limited IP address range. 0.0.0.0/0 is forbidden.
    -   After Port 8443 of the security group is enabled, all nodes \(including non-E-MapReduce ECS nodes\) in the security group enable Port 8443 at the ingress of the public network.
-   Set a Knox user

    Accessing Knox requires the username and password for authentication. The authentication is based on LDAP. You can use your own LDAP service or the LDAP service of Apache Directory Server in the cluster.

    -   Use the LDAP service in the cluster

        Method one\(recommended\):

        In the [User Management](intl.en-US/User Guide/Cluster/User management.md#) page, add Knox account directly.

        Method Two

        1.  Log on to the cluster over SSH. See [SSH Logon to Clusters](intl.en-US/User Guide/Connect to clusters using SSH.md#) for detailed steps.
        2.  Prepare your user data, for example, user Tom. In the file, replace all emr-guest with Tom, and cn:EMR GUEST with cn:Tom, and set userPassword to your password.

            ```
            su knox
            cd /usr/lib/knox-current/templates  
            vi users.ldif
            ```

            **Note:** For security, before you export your user data to LDAP, change the password of users.ldif by setting userPassword to your password.

        3.  Export to LDAP.

            ```
            su knox
            cd /usr/lib/knox-current/templates
            sh ldap-sample-users.sh
            ```

    -   Use your LDAP service
        1.  In the cluster configuration management, find the Knox configuration management. In the cluster-topo configuration, set main.ldapRealm.userDnTemplate to your user DN template and main.ldapRealm.contextFactory.url to your LDAP server domain name and port. Then, save the settings and restart Knox.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17921/154200999211122_en-US.png)

        2.  Generally, your LDAP service is not running in the cluster. You must enable the Knox port for accessing the LDAP service in the public network, for example, Port 10389. For more information, see the preceding steps for enabling Port 8443. Select **Internet outbound**.

            **Note:** For security, the authorization object must be the public IP address of your Knox cluster. 0.0.0.0/0\*\* is forbidden.


## Access Knox { .section}

-   Access using the shortcut link of E-MapReduce
    1.  Log on to the [E-MapReduce console](https://emr.console.aliyun.com/).
    2.  Click the ID link of the target cluster.
    3.  In the left-side navigation pane, click **Clusters and Services**.
    4.  Click the relevant services on the EMR Service console page, such as HDFS and YARN.
    5.  In the upper-right corner, click **Quick Link**.
-   Access using the public IP address of the cluster
    1.  Check the public IP address in cluster details.
    2.  Access the URLs of relevant services in the browser.
        -   HDFS UI: [https://\{cluster\_access\_ip\}:8443/gateway/cluster-topo/hdfs/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/hdfs/?spm=a2c4g.11186623.2.8.459af364CUTH7M)
        -   Yarn UI: [https://\{cluster\_access\_ip\}:8443/gateway/cluster-topo/yarn/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/yarn/?spm=a2c4g.11186623.2.9.459af364CUTH7M)
        -   SparkHistory UI : [https://\{cluster\_access\_ip\}:8443/gateway/cluster-topo/sparkhistory/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/sparkhistory/?spm=a2c4g.11186623.2.10.459af364CUTH7M)
        -   Ganglia UI: [https://\{cluster\_access\_ip\}:8443/gateway/cluster-topo/ganglia/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/ganglia/?spm=a2c4g.11186623.2.11.459af364CUTH7M)
        -   Storm UI: [https://\{cluster\_access\_ip\}:8443/gateway/cluster-topo/storm/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/storm/?spm=a2c4g.11186623.2.12.459af364CUTH7M)
        -   Oozie UI: [https://\{cluster\_access\_ip\}:8443/gateway/cluster-topo/oozie/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/oozie/?spm=a2c4g.11186623.2.13.459af364CUTH7M)
    3.  The browser shows **website is not security** because the Knox service uses the self-signed certificate. Confirm that the accessed IP address is the same as that of your cluster and the port is 8443. Click **advance** \> **continue**.
    4.  Enter the username and password set in LDAP in the logon dialog box.

## Access control lists \(ACLs\) { .section}

Knox provides service-level permission management to limit service access to specific users, user groups, or IP addresses. See [Apache Knox Authorization](https://knox.apache.org/books/knox-0-13-0/user-guide.html?spm=a2c4g.11186623.2.14.459af364CUTH7M#Authorization).

-   Example:
    -   Scenario: YARN UI only allows the access by user Tom.
    -   Steps: In the cluster configuration management, find the Knox configuration management and the cluster-topo configuration, and add ACL code between `<gateway>...</gateway>` labels in the cluster-topo configuration.

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

-   Notes:

    Knox provides REST APIs for operating a range of services, for example, adding or deleting HDFS files. For security, make sure the authorization object is your limited IP address range when you enable Knox Port 8443 of the security group in the ECS console. 0.0.0.0/0 is forbidden. Do not use the LDAP username and password in the Knox installation directory to access Knox.


