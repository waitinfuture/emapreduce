# HBase配置 {#concept_gsf_rmj_bfb .concept}

## HBase集成Ranger {#section_gcm_wnj_bfb .section}

前面简介中介绍了E-MapReduce中创建启动Ranger服务的集群，以及一些准备工作，本节介绍HBase集成Ranger的一些步骤流程。

## Enable HBase Plugin {#section_hnm_b4j_bfb .section}

1.  在集群配置页面，在你想操作的集群后的操作栏中单击**管理**
2.  在服务列表中单击**Ranger**进入Ranger配置页面
3.  在Ranger配置页面，单击右侧的**操作**下拉菜单，选择**Enable HBase PLUGIN**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17951/153829647411513_zh-CN.png)

4.  单击右上角**查看任务进度**，等待任务完成。![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17951/153829647411514_zh-CN.png)

## Ranger UI添加HBase service {#section_elq_cpj_bfb .section}

参见[Ranger简介](https://help.aliyun.com/document_detail/66410.html?spm=a2c4g.11186623.2.6.6b7979cbs5BcBk)的介绍进入Ranger UI。

在Ranger的UI页面添加Hase Service：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17951/153829647511521_zh-CN.png)

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17951/153829647511522_zh-CN.png)

**说明：** $\{id\}： 可登录机器执行`host`命令，hostname中的数字即为$\{id\}的值。

## 重启HBase {#section_nkb_sqj_bfb .section}

上述任务完成后，需要重启HBase才生效。重启HBase，请按照以下步骤操作：

1.  在集群配置管理页面中，从**Ranger**切换到**HBase**。
2.  单击右上角**操作**下拉菜单，选择**RESTART All Components**。
3.  单击右上角**查看操作历史**查看任务进度，等待重启任务完成。

## 设置管理员账号 {#section_twd_xqj_bfb .section}

用户需要设置管理员账号的权限\(admin权限\)，用于执行一些管理命令，如`balance/compaction/flush/split`等等。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17951/153829647511523_zh-CN.png)

上图中已经存在权限策略，您只需要单击右侧的编辑按钮，将user改成自己想要设置的账号即可，另外也可以修改其中的权限\(如只保留Admin权限\)。hbase账号必须要默认设置为管理员账号。

## 权限配置示例 {#section_rcd_2rj_bfb .section}

上面一节中已经将Ranger集成到HBase，可以进行相关的权限设置。例如给用户test授予表foo\_ns:test的`Create/Write/Read`权限。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17951/153829647511524_zh-CN.png)

单击上图中的**emr-hbase**进入配置页面，配置相关权限。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17951/153829647511525_zh-CN.png)

User/Group 会自动从集群中同步过来，大约需要一分钟，用户可以事先将它们添加到集群。

按照上述步骤设置添加一个Policy后，就实现了对test用户的授权，然后test用户就可以对foo\_ns:test表进行访问了。

**说明：** Policy被添加1分钟左右后，HBase才会生效。

