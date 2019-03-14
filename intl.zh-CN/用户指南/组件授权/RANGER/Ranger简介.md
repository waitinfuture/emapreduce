# Ranger简介 {#concept_gpl_jrc_z2b .concept}

Apache Ranger 提供集中式的权限管理框架，可以对 Hadoop 生态中的 HDFS/Hive/YARN/Kafka/Storm/Solr 等组件进行细粒度的权限访问控制，并且提供了 Web UI 方便管理员进行操作。

## 创建集群 {#section_e4j_sj3_bfb .section}

在 E-MapReduce 控制台创建 EMR-2.9.2/EMR-3.9.0 及以上的集群，勾选 Ranger 组件即可, 如下图所示:

![创建集群](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17948/155255250611486_zh-CN.png)

如果已经创建 EMR-2.9.2/EMR-3.9.0 及以上的集群，可以在集群的集群与服务管理页面添加 Ranger 服务：

![添加Ranger服务](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17948/155255250611487_zh-CN.png)

![添加Ranger服务](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17948/155255250711488_zh-CN.png)

**说明：** 

-   开启 Ranger 后，设置安全控制策略之前，不会对应用程序产生影响和限制。
-   Ranger 中设置的用户策略为集群 Hadoop 账号。

## Ranger UI {#section_mcp_pk3_bfb .section}

在安装了 Ranger 的集群后，单击**管理**，然后在左侧导航栏中选择**访问链接与端口**，可以通过快捷链接访问 Ranger UI，如下图所示:

![访问链接与端口](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17948/155255250711489_zh-CN.png)

单击链接后会进入Ranger UI登录界面，默认的用户名/密码是admin/admin，如下图：

![Ranger UI](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17948/155255250711490_zh-CN.png)

![Service Manager](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17948/155255250711491_zh-CN.png)

## 修改密码 {#section_igf_ll3_bfb .section}

管理员首次登录后，需要修改 admin 的密码，如下图：

![修改密码](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17948/155255250711492_zh-CN.png)

![修改密码](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17948/155255250711493_zh-CN.png)

改完admin密码后，单击右上角**admin**下拉菜单的**Log Out**，然后使用新的密码登录即可。

## 组件集成Ranger {#section_lnk_tl3_bfb .section}

经过上述步骤，可以将集群中的相关组件集成到 Ranger，进行相关权限的控制，详情请参见：

-   [HDFS集成Ranger](intl.zh-CN/用户指南/组件授权/RANGER/HDFS配置.md#)
-   [Hive集成Ranger](intl.zh-CN/用户指南/组件授权/RANGER/Hive配置.md#)
-   [HBase集成Ranger](intl.zh-CN/用户指南/组件授权/RANGER/HBase配置.md#)

