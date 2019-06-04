# HDFS 授权 {#concept_z4t_mvv_1fb .concept}

HDFS 开启了权限控制后，用户访问 HDFS 需要有合法的权限才能正常操作 HDFS，如读取数据/创建文件夹等。

## 添加配置 {#section_i3m_xvv_1fb .section}

HDFS 权限相关的配置如下:

-   dfs.permissions.enabled

    开启权限检查，即使该值为 false， chmod/chgrp/chown/setfacl 操作还是会进行权限检查

-   dfs.datanode.data.dir.perm

    datanode 使用的本地文件夹路径的权限，默认755

-   fs.permissions.umask-mode
    -   权限掩码，在新建文件/文件夹的时候的默认权限设置
    -   新建文件：0666 & ^umask
    -   新建文件夹：0777 & ^umask
    -   默认 umask 值为 022，即新建文件权限为 644\(666&^022=644\)，新建文件夹权限为 755\(777&^022=755\)
    -   EMR 的 Kerberos 安全集群默认设置为 027，对应新建文件权限为 640，新建文件夹权限为 750
-   dfs.namenode.acls.enabled
    -   打开 ACL 控制，打开后除了可以对 owner/group 进行权限控制外，还可以对其它用户进行设置。
    -   设置 ACL 相关命令:

        ```
        hadoop fs -getfacl [-R] <path>
        hadoop fs -setfacl [-R] [-b |-k -m |-x <acl_spec> <path>] |[--set <acl_spec>   <path>]
        ```

        如:

        ```
        su test
         #test用户创建文件夹
         hadoop fs -mkdir /tmp/test
         #查看创建的文件夹的权限
         hadoop fs -ls /tmp
         drwxr-x---   - test   hadoop          0 2017-11-26 21:18 /tmp/test
         #设置acl,授权给foo用户rwx
         hadoop fs -setfacl -m user:foo:rwx /tmp/test
         #查看文件权限(+号表示设置了ACL)
         hadoop fs -ls  /tmp/
         drwxrwx---+  - test   hadoop          0 2017-11-26 21:18 /tmp/test
         #查看acl
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

    超级用户组，属于该组的用户都具有超级用户的权限


## 重启 HDFS 服务 {#section_wjv_wwv_1fb .section}

对于 Kerberos 安全集群，已经默认设置了 HDFS 的权限\( umask 设置为 027\)，无需配置和重启服务。

对于非 Kerberos 安全集群需要添加配置并重启服务

## 其它 {#section_vh3_cxv_1fb .section}

-   umask 值可以根据需求自行修改
-   HDFS 是一个基础的服务，Hive/HBase 等都是基于 HDFS，所以在配置其它上层服务时，需要提前配置好 HDFS 的权限控制。
-   在 HDFS 开启权限后，需要设置好服务的日志路径\(如 Spark 的 /spark-history、YARN 的 /tmp/$user/ 等\)
-   sticky bit

    针对文件夹可设置 sticky bit，可以防止除了 superuser/file owner/dir owner 之外的其它用户删除该文件夹中的文件/文件夹\(即使其它用户对该文件夹有 rwx 权限\)。如：

    ``` {#codeblock_x15_4fq_ixy}
    #即在第一位添加数字1
    hadoop fs -chmod 1777 /tmp
    hadoop fs -chmod 1777 /spark-history
    hadoop fs -chmod 1777 /user/hive/warehouse
    ```


