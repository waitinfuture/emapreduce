# Hue {#concept_uh5_qtw_y2b .concept}

E-MapReduce currently supports [Hue](http://gethue.com/), which you can access through Apache Knox. The following section provides an overview of how to use Hue.

## Preparation {#section_vy3_stw_y2b .section}

In the [Security groups](reseller.en-US/User Guide/Clusters/Security groups.md#) cluster, [set the security group rules](../../../../../reseller.en-US/Security/Security groups/Add security group rules.md#), and open port 8888.

**Note:** Set security group rules for limited IP ranges. IP 0.0.0.0/0 is not allowed to add into the security group.

## Access Hue {#section_ws2_j5w_y2b .section}

To access Hue, complete the following steps:

1.  In the EMR console, click **Manage** to the right of the cluster ID.
2.  On the left side of the **Configuration** page, click **Access Links and Ports**.

## View the password {#section_vbs_l5w_y2b .section}

If Hue does not have an administrator after the first running, the first user to log on is set automatically to administrator. For security, E-MapReduce generates an administrator account and password by default. The administrator account is **admin**. To view the password, complete the following steps:

1.  Click **Manage** to the right of the cluster ID.
2.  In the Clusters and Services panel, click **Hue**.
3.  Click the **Configuration** tab to go to the admin\_pwd parameter. It is a random password.

## Create a new account if you forget your password {#section_ekc_p5w_y2b .section}

If you forget your password for your Hue account, you can create a new account by completing the following steps:

1.  In the cluster list page, click **Manage** next to the target cluster.
2.  In the navigation panel on the left, click **Cluster Overview**.
3.  In the **Core Instance Group**, obtain the public network IPs of some master nodes.
4.  Log on to the master node through SSH.
5.  Execute the following command:

    ```
    /opt/apps/hue/build/env/bin/hue createsuperuser
    ```

6.  Enter a new user name, e-mail, and password, and press Enter.

    If **Superuser created successfully** is displayed, you have successfully created a new account. You can now log on to Hue with the new account.


## Add or modify a configuration { .section}

1.  In the cluster list page, click **Manage** next to the target cluster.
2.  In the service list, click **Hue**, and then click the **Configuration** tab.
3.  In the upper-right corner of the page, click **Custom Configuration**, and configure the Key and Value fields. The key must adhere to the following specifications:

    ```
    $section_path.$real_key
    ```

    **Note:** 

    -   $real\_key is the actual key to be added, such as hive\_server\_host.
    -   In the hue.ini file, you can view the $section\_path before the $real\_key.

        For example, if the hive\_server\_host belongs to the \[beeswax\] section, this means that the $section\_path is beeswax. If this is the case, the key to be added is beeswax.hive\_server\_host.

    -       -   If you need to modify the multilevel section \[desktop\] -\> \[\[ldap\]\] -\> \[\[\[ldap\_servers\]\]\] -\> \[\[\[\[users\]\]\]\] -\>user\_name\_attr value in the hue.ini file, the key to be configured is desktop.ldap.ldap\_servers.users.user\_name\_attr.

