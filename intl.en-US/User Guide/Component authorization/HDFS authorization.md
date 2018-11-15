# HDFS authorization {#concept_z4t_mvv_1fb .concept}

When HDFS is enabled, user access to HDFS requires legal permissions in order to operate HDFS properly, such as read data, create folders and others.

## Add a configuration {#section_i3m_xvv_1fb .section}

Configurations related to HDFS permission are as follows:

-   dfs.permissions.enabled

    Enable permission check. Even if the value is false, chmod/chgrp/chown/setfacl performs permission check.

-   dfs.datanode.data.dir.perm

    The permission of the local folder used by datanode, which is 755 by default.

-   fs.permissions.umask-mode
    -   Permission mask, default permission settings when creating a new file/folder
    -   File creation: 0666 & ^umask
    -   Folder creation: 0777 & ^umask
    -   Default umask value is 022, i.e. the permission of file creation is 644 \(666&^022 = 644\), and permission of folder creation is 755 \(777&^022 = 755\).
    -   The default setting of Kerberos security cluster in the EMR is 027, the corresponding permission of file creation is 640, and permission of folder creation is 750.
-   dfs.namenode.acls.enabled
    -   Enable ACL control. This gives you permission control on owner/group, and you can also set it for other users.
    -   Commands for setting ACL:

        ```
        hadoop fs -getfacl [-R] <path>
        hadoop fs -setfacl [-R] [-b |-k -m |-x <acl_spec> <path>] |[--set <acl_spec>   <path>]
        ```

        For example:

        ```
        su test
         #The user test creates a folder
         hadoop fs -mkdir /tmp/test
         #View the permission of the created folder
         hadoop fs -ls /tmp
         drwxr-x---   - test   hadoop          0 2017-11-26 21:18 /tmp/test
         #Set ACL and grant rwx permissions to user foo
         hadoop fs -setfacl -m user:foo:rwx /tmp/test
         #View the permission of the file (+ means that ACL is set)
         hadoop fs -ls  /tmp/
         drwxrwx---+  - test   hadoop          0 2017-11-26 21:18 /tmp/test
         #View ACL
          hadoop fs -getfacl  /tmp/test
          # file: /tmp/test
        # owner: test
        # group: hadoop
        user::rwx
        user:foo:rwx
        group::r-x
        mask::rwx
        other::---
        ```

-   dfs.permissions.superusergroup

    Super user group. Users in the group have super user permissions.


## Restart the HDFS service {#section_wjv_wwv_1fb .section}

For Kerberos security clusters, HDFS permissions have been set by default \(umask is set to 027\), without configuration and service restart.

For non-Kerberos security clusters, a configuration must be added and the service must be restarted.

## Other {#section_vh3_cxv_1fb .section}

-   Umask value can be modified as needed.
-   HDFS is a basic service, and Hive/HBase are based on HDFS. Therefore, HDFS permission control must be configured in advance when configuring other upper layer services.
-   When permissions are enabled for HDFS, the services must be set up \(such as /spark-history for spark, and /tmp/$user/ for yarn\).
-   Sticky bit:

    Sticky bit can be set for a folder to prevent users other than superuser/file owner/dir owner from deleting files/folders in the folder \(even if other users have rwx permissions on the folder\), for example:

    ```
    #That is, adding numeral 1 as the first digit
    hadoop fs -chmod 1777 /tmp
    hadoop fs -chmod 1777 /spark-history
    hadoop fs -chmod 1777 /user/hive/warehouse
    ```


