# Knox

本文介绍如何在E-MapReduce上使用Knox。

已创建E-MapReduce集群，并且选择了Knox服务。详情请参见[创建集群](/cn.zh-CN/集群管理/集群配置/创建集群.md)。

## 准备工作

-   设置安全组访问：

    1.  获取您当前设备的公网访问IP地址。

        为了安全的访问集群组件，在设置安全组策略时，推荐您只针对当前的公网访问IP地址开放。获取您当前公网访问IP地址的方法是，访问[ip.taobao.com](http://ip.taobao.com/)，然后在左下角即可查看您当前的公网访问IP地址。

    2.  添加8443端口：
        1.  在[阿里云E-MapReduce控制台](https://emr.console.aliyun.com)，集群**详情**页面的**网络信息**区域，查看**网络类型**，单击**安全组ID**链接。
        2.  单击**添加安全组规则**。
        3.  **端口范围**填写**8443/8443**。
        4.  **授权对象**填写[步骤a](#step_01)中获取的公网访问IP地址。
        5.  单击**确定**。
    **说明：**

    -   为防止被外部的用户攻击导致安全问题，**授权对象**禁止填写为**0.0.0.0/0**。
    -   如果您创建集群时，没有挂载公网IP，可以在ECS控制台为该ECS实例添加公网IP。添加成功后，返回EMR控制台，在集群管理下的**主机列表**页面，单击**同步主机信息**可以立即同步主机信息。
    -   Knox节点新挂载公网IP后，需要[提交工单](https://selfservice.console.aliyun.com/ticket/createIndex?spm=5176.2020520129.103.2.9Z8xg7)处理，进行域名和公网IP的绑定操作。
-   设置Knox用户

    访问Knox时需要验证身份，即需要输入您的用户名和密码。Knox的用户身份验证基于LDAP，您可以使用自有LDAP服务，也可以使用集群中Apache Directory Server的LDAP服务。

    -   使用集群中的LDAP服务

        方式一（推荐）

        在集群的**用户管理**页面，直接添加Knox访问账号，详情请参见[用户管理](/cn.zh-CN/集群管理/集群规划/管理用户.md)。

        方式二 ：

        1.  SSH登录到集群上，详细步骤请参见[使用SSH连接主节点](/cn.zh-CN/集群管理/集群配置/连接集群/使用SSH连接主节点.md)。
        2.  准备您的用户数据，例如Tom。

            编辑users.ldif文件。

            ```
            su knox
            cd /usr/lib/knox-current/templates  
            vi users.ldif
            ```

            设置文件中所有的`emr-guest`替换为`Tom`，将`EMR GUEST`替换为`Tom`，设置setPassword的值为您自己的密码。

        3.  导入用户数据至LDAP。

            ```
            su knox
            cd /usr/lib/knox-current/templates
            sh ldap-sample-users.sh
            ```

    -   使用自有LDAP服务的情况：
        1.  在Knox的**配置**页面，单击**cluster-topo**页签。
        2.  配置**xml-direct-to-file-content**中参数。

            |配置项|描述|
            |---|--|
            |`main.ldapRealm.userDnTemplate`|设置为您的用户DN模板。|
            |`main.ldapRealm.contextFactory.url`|设置为您的LDAP服务器域名和端口。|

            ![cluster-topo](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9411883061/p134502.png)

        3.  单击右上角的**保存**。
        4.  在**确认修改**对话框中，配置各项参数，单击**确定**。
        5.  单击右上角的**操作** \> **重启 Knox**。
        6.  在**执行集群操作**对话框中，配置各项参数，单击**确定**。

            在**确认**对话框中，单击**确定**。

        7.  开启Knox访问公网LDAP服务的端口，例如10389。

            您可以参见8443端口的开启步骤，选择**出方向**，开启10389端口。


## 访问Knox的Web UI

您可以使用Knox账号访问HDFS、YARN、Spark和Ganglia等Web UI页面。

-   使用E-MapReduce链接访问：
    1.  通过主账号登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com)。
    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。
    3.  单击上方的**集群管理**页签。
    4.  在**集群管理**页面，单击集群右侧的**详情**。
    5.  在左侧导航栏，单击**访问链接与端口**。
    6.  在**访问链接与端口**页面，单击服务所在行的链接。
-   使用集群公网IP地址访问：
    1.  在**集群基础信息**页面，查看公网IP地址。
    2.  在浏览器中访问相应服务的URL。
        -   HDFS UI：https://\{集群公网IP地址\}:8443/gateway/cluster-topo/hdfs/。
        -   Yarn UI：https://\{集群公网IP地址\}:8443/gateway/cluster-topo/yarn/。
        -   SparkHistory UI：https://\{集群公网IP地址\}:8443/gateway/cluster-topo/sparkhistory/。
        -   Ganglia UI：https://\{集群公网IP地址\}:8443/gateway/cluster-topo/ganglia/。
        -   Storm UI：https://\{集群公网IP地址\}:8443/gateway/cluster-topo/storm/。
        -   Oozie UI：https://\{集群公网IP地址\}:8443/gateway/cluster-topo/oozie/。

## 用户权限管理（ACLs）

Knox提供服务级别的权限管理，可以限制特定的用户、用户组和IP地址访问特定的服务，详情请参见[Apache Knox授权](https://knox.apache.org/books/knox-0-13-0/user-guide.html?spm=a2c4g.11186623.2.14.459af364CUTH7M#Authorization)。

示例如下：

-   场景：YARN UI只允许用户Tom访问。
-   操作步骤：

    1.  在Knox的**配置**页面，单击**cluster-topo**页签。
    2.  配置**xml-direct-to-file-content**中参数。

        在`<gateway>...</gateway>`标签之间添加ACLs代码。

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

    3.  单击右上角的**保存**。
    4.  在**确认修改**对话框中，配置各项参数，单击**确定**。
    5.  单击右上角的**操作** \> **重启 Knox**。
    6.  在**执行集群操作**对话框中，配置各项参数，单击**确定**。

        在**确认**对画框中，单击**确定**。

    **警告：** Knox会开放对应服务的REST API，您可以通过各服务的REST API操作服务。例如：HDFS文件的添加、删除等操作。出于安全原因，请勿使用Knox安装目录下的LDAP用户名和密码作为Knox的访问用户。


