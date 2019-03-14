# Knox {#concept_knp_s1x_y2b .concept}

E-MapReduce supports [Apache Knox](https://knox.apache.org/). If you select a Knox-supported image to create a cluster, you can access the Web UI from the public network to use services such as YARN, HDFS, and SparkHistory.

## Preparations {#section_m1v_wkl_z2b .section}

-   Enable Knox access using a public IP address

    1.  The service port of Knox on E-MapReduce is 8443. In the cluster details, find the ECS security group in which the cluster is located.
    2.  Change the corresponding security group in the ECS console and add a rule in **Internet inbound** to enable port 8443.
    **Note:** 

    -   For security reasons, the authorization object must be your limited IP address range. 0.0.0.0/0 is forbidden.
    -   After port 8443 of the security group is enabled, all nodes \(including non-E-MapReduce ECS nodes\) in the security group enable port 8443 at the ingress of the public network.
-   Set a Knox user

    Accessing Knox requires a user name and password for authentication. The authentication is based on LDAP. You can use your own LDAP service or the LDAP service of Apache Directory Server in the cluster.

    -   Use the LDAP service in the cluster

        Method one \(recommended\):

        Add a Knox account in the [User Management](reseller.en-US/User Guide/Configure clusters/User management.md#) page.

        Method two:

        1.  Log on to the cluster through SSH. For more information, see [Connect to clusters using SSH](reseller.en-US/Quick Start/Connect to clusters using SSH.md#).
        2.  Prepare your user data. Here, Tom is used as the user name. In the file, replace all emr-guest with Tom and cn:EMR GUEST with cn:Tom, and set userPassword to your password.

            ```
            su knox
            cd /usr/lib/knox-current/templates  
            vi users.ldif
            ```

            **Note:** For security reasons, before you export your user data to LDAP, change the password of users.ldif by changing userPassword to your password.

        3.  Export to LDAP.

            ```
            su knox
            cd /usr/lib/knox-current/templates
            sh ldap-sample-users.sh
            ```

    -   Use your own LDAP service
        1.  Enter the cluster configuration management page. In the cluster-topo configuration, set main.ldapRealm.userDnTemplate to your user DN template and main.ldapRealm.contextFactory.url to your LDAP server domain name and port. Then, save the settings and restart Knox.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17921/155255219911122_en-US.png)

        2.  Your LDAP service does not typically run in the cluster. You must enable the Knox port to access the LDAP service in the public network, such as port 10389. For more information, see the preceding steps for enabling port 8443. Then, select **Internet outbound**.

            **Note:** For security reasons, the authorization object must be the public IP address of your Knox cluster. 0.0.0.0/0\*\* is forbidden.


## Access Knox { .section}

-   Access using the E-MapReduce shortcut link
    1.  Log on to the [E-MapReduce console](https://partners-intl.console.aliyun.com/#/emr).
    2.  Click the ID link of the target cluster.
    3.  In the navigation pane on the left, click **Clusters and Services**.
    4.  Click the relevant services on the E-MapReduce services page, such as HDFS and YARN.
    5.  In the upper-right corner, click **Quick Link**.
-   Access using the public IP address of the cluster
    1.  Check the public IP address in the cluster details.
    2.  Access the URLs of the relevant services in the browser.
        -   HDFS UI: [https://\{cluster\_access\_ip\}:8443/gateway/cluster-topo/hdfs/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/hdfs/?spm=a2c4g.11186623.2.8.459af364CUTH7M)
        -   YARN UI: [https://\{cluster\_access\_ip\}:8443/gateway/cluster-topo/yarn/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/yarn/?spm=a2c4g.11186623.2.9.459af364CUTH7M)
        -   SparkHistory UI: [https://\{cluster\_access\_ip\}:8443/gateway/cluster-topo/sparkhistory/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/sparkhistory/?spm=a2c4g.11186623.2.10.459af364CUTH7M)
        -   Ganglia UI: [https://\{cluster\_access\_ip\}:8443/gateway/cluster-topo/ganglia/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/ganglia/?spm=a2c4g.11186623.2.11.459af364CUTH7M)
        -   Storm UI: [https://\{cluster\_access\_ip\}:8443/gateway/cluster-topo/storm/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/storm/?spm=a2c4g.11186623.2.12.459af364CUTH7M)
        -   Oozie UI: [https://\{cluster\_access\_ip\}:8443/gateway/cluster-topo/oozie/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/oozie/?spm=a2c4g.11186623.2.13.459af364CUTH7M)
    3.  **website is not security** is displayed in your browser because the Knox service uses a self-signed certificate. Confirm that the accessed IP address is the same as that of your cluster and the port is 8443. Click **advance** \> **continue**.
    4.  Enter the user name and password set in LDAP in the logon dialog box.

## Access control lists { .section}

Knox provides service-level permission management to limit service access to specific users, user groups, or IP addresses. See [Apache Knox Authorization](https://knox.apache.org/books/knox-0-13-0/user-guide.html?spm=a2c4g.11186623.2.14.459af364CUTH7M#Authorization).

-   Example
    -   Scenario: The YARN UI only allows access by user Tom.
    -   Steps: Enter the cluster configuration management page. In the cluster-topo configuration, add access control list \(ACL\) code between the `<gateway>...</gateway>` labels.

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

-   Notes

    Knox provides RESTful APIs for operating a range of services, including adding or deleting HDFS files. For security reasons, make sure that when you enable port 8443 of the security group in the ECS console, the authorization object is your limited IP address range. 0.0.0.0/0 is forbidden. Do not use the LDAP user name and password in the Knox installation directory to access Knox.


