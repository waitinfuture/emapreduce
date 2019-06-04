# HBase 授权 {#concept_e5l_2mb_bfb .concept}

HBase 在不开启授权的情况下，任何账号对 HBase 集群可以进行任何操作，例如 disable table/drop table/major compact 等。

**说明：** 

对于没有 Kerberos 认证的集群，即使开启了 HBase 授权，用户也可以伪造身份访问集群服务。所以建议创建高安全模式（即支持 Kerberos）的集群，详见 [Kerberos 简介](intl.zh-CN/开源组件介绍/Kerberos认证/Kerberos 简介.md#)。

## 添加配置 {#section_q4z_qmb_bfb .section}

在 HBase 集群的集群与服务管理页面选择 **HBase** \> **配置** \> **hbase-site** \> **自定义配置**

添加如下几个参数:

```
<property>
     <name>hbase.security.authorization</name>
     <value>true</value>
</property>
<property>
     <name>hbase.coprocessor.master.classes</name>
     <value>org.apache.hadoop.hbase.security.access.AccessController</value>
</property>
<property>
     <name>hbase.coprocessor.region.classes</name>
 <value>org.apache.hadoop.hbase.security.token.TokenProvider,org.apache.hadoop.hbase.security.access.AccessController</value>
</property>
<property>
  <name>hbase.coprocessor.regionserver.classes</name>
  <value>org.apache.hadoop.hbase.security.access.AccessController,org.apache.hadoop.hbase.security.token.TokenProvider</value>
</property>
```

## 重启 HBase 集群 {#section_qvn_knb_bfb .section}

在 HBase 集群的集群与服务管理页面选择 **HBase** \> **操作** \> **RESTART All Components**

## 授权（ACL） {#section_gmy_pnb_bfb .section}

-   基本概念

    授权就是将对 \[某个范围的资源\] 的 \[操作权限\] 授予\[某个实体\]

    在 HBase 中，上述对应的三个概念分别为：

    -   某个范围（Scope）的资源
        -   Superuser

            超级账号可以进行任何操作，运行 HBase 服务的账号默认是 Superuser。也可以通过在 hbase-site.xml 中配置 hbase.superuser 的值可以添加超级账号

        -   Global

            Global Scope 拥有集群所有 table 的 Admin 权限

        -   Namespace

            在 Namespace Scope 进行相关权限控制

        -   Table

            在 Table Scope 进行相关权限控制

        -   ColumnFamily

            在 ColumnFamily Scope 进行相关权限控制

        -   Cell

            在 Cell Scope 进行相关权限控制

    -   操作权限
        -   Read（R）

            读取某个 Scope 资源的数据

        -   Write （W）

            写数据到某个 Scope 的资源

        -   Execute （X）

            在某个 Scope 执行协处理器

        -   Create （C）

            在某个 Scope 创建/删除表等操作

        -   Admin（A）

            在某个 Scope 进行集群相关操作，如 balance/assign 等

    -   某个实体
        -   User

            对某个用户授权

        -   Group

            对某个用户组授权

-   授权命令
    -   grant 授权

        ```
        grant <user> <permissions> [<@namespace> [<table> [<column family> [<column qualifier>]]]
        ```

        -   user/group 的授权方式一样，group 需要加一个前缀 @

            ```
            grant 'test','R','tbl1'   #给用户test授予表tbl1的读权限
              grant '@testgrp','R','tbl1' #给用户组testgrp授予表tbl1的读权限
            ```

        -   namespace 需要加一个前缀 @

            ```
            grant 'test 'C','@ns_1'  #给用户test授予namespace ns_1的CREATE权限
            ```

    -   revoke 回收
    -   user\_permissions 查看权限

