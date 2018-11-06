# Hue {#concept_uh5_qtw_y2b .concept}

E-MapReduce currently supports Hue. You can access [Hue](http://gethue.com/) by Apache Knox.

## Preparation {#section_vy3_stw_y2b .section}

In the [Security group](intl.en-US/User Guide/Cluster/Security group.md#) cluster, [set security group rules](../../../../intl.en-US/User Guide/Security groups/Add security group rules.md#), and open the 8888 port.

**Note:** Set security group rules for limited IP ranges. It is prohibited to open rules to 0.0.0.0/0 when configuring.

## Access Hue {#section_ws2_j5w_y2b .section}

To accesss Hue, take the following steps:

1.  Click **Manage** on the right side of the cluster ID in the EMR console.
2.  On the left side of the Configuration page, click **Access Links and Ports**.

## Access password {#section_vbs_l5w_y2b .section}

If Hue has no administrator after the first running, the first login user is set to administrator by default. EMR will generate an administrator account and password by default for security. The administrator account is admin. You can view the password through the following way:

1.  On the right-side of the cluster ID, click **Manage**.
2.  In the Clusters and Services panel, click **Hue**.
3.  Click the **Configuration** tab to go to the parameter admin\_pwd. It is a random password.

## Forgot password {#section_ekc_p5w_y2b .section}

If you forget the password corresponding to your Hue account, you can recreate an account by the following method:

1.  Click **Manage** of the specific cluster in the cluster list page.
2.  In the left-side navigation panel, click **Cluster Overview**.
3.  In the **Core Instance Group**, obtain public network IPs of some master nodes.
4.  Log on to the master node through SSH.
5.  Create a new account by executing the following command.

    ```
    /opt/apps/hue/build/env/bin/hue createsuperuser
    ```

6.  Enter a new user name, e-mail, password, and enter the password again, press Enter.

    If **Superuser created successfully** is prompted, it means the new account is created successfully, and you can log on to Hue with the new account later.


## Add/modify configuration { .section}

1.  On the right-side of the cluster list panel, click **Manage**.
2.  In the service list, click **Hue**, and then click the **Configuration** tab.
3.  At the right-side corner of the page, click **Custom Configuration**, configure the Key and Value fields. The Key needs to follow the following specifications:

    ```
    $section_path.$real_key
    ```

    **Note:** 

    -   $real\_key is the actual key to be added, such as hive\_server\_host.
    -   You can view $section\_pathbefore $real\_key, in hue.ini file. For example,

        You can see hive\_server\_host belongs to the \[beeswax\] section in [hue.ini](https://github.com/cloudera/hue/blob/release-4.1.0/desktop/conf.dist/hue.ini) file. That is, $section\_path is beeswax.

    -   In conclusion, the key to be added is beeswax.hive\_server\_host.
    -   If you need to modify the multilevel section \[desktop\] -\> \[\[ldap\]\] -\> \[\[\[ldap\_servers\]\]\] -\> \[\[\[\[users\]\]\]\] -\>user\_name\_attr value in the hue.ini file, the key to be configured is desktop.ldap.ldap\_servers.users.user\_name\_attr.

