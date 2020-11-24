# Overview

Apache Ranger provides a centralized permission management framework that can implement fine-grained access control on Kafka data. Apache Ranger also provides a web UI for administrators to conveniently perform operations.

## Add the Ranger service to a cluster

-   When you create a Kafka cluster, select Ranger from the optional services in the E-MapReduce \(EMR\) console.

    ![create_kafka](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7788765061/p165749.png)

-   If you have created a Kafka cluster, click Add Service in the upper-right corner of the Status tab on the **Clusters and Services** page. In the Add Service dialog box, select RANGER and click OK.

    ![add_service](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7788765061/p165775.png)


## Access the Ranger UI

1.  Check configurations.

    Before you access the Ranger UI, ensure that a security group is configured, which indicates that you are allowed to access the Hadoop cluster on the current network. For more information, see [Access open-source components](/intl.en-US/Cluster Management/Configure clusters/Access open-source components.md).

2.  Log on to the Ranger UI.
    1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/).
    2.  Click the **Cluster Management** tab.
    3.  Find the target cluster and click **Details** in the Actions column.
    4.  In the left-side navigation pane, click **Access Links and Ports**.
    5.  On the Access Links and Ports page that appears, click the link for **RANGER UI**.
    6.  On the Ranger UI logon page, log on with the default username \(admin\) and password.

        ![Ranger UI](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/8730811951/p11490.png)

3.  Change the password.
    1.  When you log on to the Ranger UI for the first time, click **Settings** in the top navigation bar.

        ![Change password](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4974326061/p11492.png)

    2.  Change the password of the **admin** user.

        ![Change password](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/4974326061/p11493.png)

    3.  In the upper-right corner, choose **admin** \> **Log Out**.

        Log on to the Ranger UI with the new password.


## Manage users

You can use Ranger to manage the permissions of users or user groups, which include users and user groups from an LDAP server \(recommended\) or the local UNIX system.

-   Interconnect Ranger Admin with an LDAP server

    For more information, see [Integrate Ranger Admin with an LDAP server](/intl.en-US/Cluster types/Hadoop cluster/Ranger/Connect to LDAP for Ranger/Integrate Ranger Admin with an LDAP server.md).

-   Integrate Ranger UserSync with an LDAP server

    For more information, see [Integrate Ranger UserSync with an LDAP server](/intl.en-US/Cluster types/Hadoop cluster/Ranger/Connect to LDAP for Ranger/Integrate Ranger UserSync with an LDAP server.md).


