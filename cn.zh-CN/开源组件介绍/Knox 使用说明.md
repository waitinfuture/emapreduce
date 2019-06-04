# Knox 使用说明 {#concept_knp_s1x_y2b .concept}

目前 E-MapReduce 中支持了 [Apache Knox](https://knox.apache.org/)，选择支持 Knox 的镜像创建集群，完成以下准备工作后，即可在公网直接访问 Yarn，HDFS，SparkHistory 等服务的 Web UI。

## 准备工作 {#section_m1v_wkl_z2b .section}

-   开启 Knox 公网 IP 访问

    1.  在 E-MapReduce 上 Knox 的服务端口是 8443，在集群详情中找到集群所在的 ECS 安全组。
    2.  在 ECS 控制台修改对应的安全组，在**入方向**添加一条规则，打开 8443 端口。
    **说明：** 

    -   为了安全原因，这里设置的授权对象必须是您的一个有限的 IP 段范围，禁止使用 0.0.0.0/0。
    -   打开安全组的 8443 端口之后，该安全组内的所有机器均会打开公网入方向的 8443 端口，包括非 E-MapReduce 的 ECS 机器。
-   设置 Knox 用户

    访问 Knox 时需要对身份进行验证，会要求您输入用户名和密码。Knox 的用户身份验证基于 LDAP，您可以使用自有 LDAP 服务，也可以使用集群中的 Apache Directory Server 的 LDAP 服务。

    -   使用集群中的 LDAP 服务

        方式一（推荐）

        在[用户管理](intl.zh-CN/集群规划与配置/集群规划/用户管理.md#)中直接添加 Knox 访问账号。

        方式二

        1.  SSH 登录到集群上，详细步骤请参见[SSH登录集群](intl.zh-CN/集群规划与配置/集群配置/SSH 登录集群.md#)。
        2.  准备您的用户数据，如：Tom。将文件中所有的 emr-guest 替换为 Tom，将 cn:EMR GUEST 替换为 cn:Tom，设置 userPassword 的值为您自己的密码。

            ```
            su knox
            cd /usr/lib/knox-current/templates  
            vi users.ldif
            ```

            **说明：** 出于安全原因，导入前务必修改 users.ldif 的用户密码，即：设置 userPassword 的值为您自己的用户密码。

        3.  导入到 LDAP。

            ```
            su knox
            cd /usr/lib/knox-current/templates
            sh ldap-sample-users.sh
            ```

    -   使用自有 LDAP 服务的情况
        1.  在集群配置管理中找到 KNOX 的配置管理，在 cluster-topo 配置中设置两个属性：main.ldapRealm.userDnTemplate 与 main.ldapRealm.contextFactory.url。main.ldapRealm.userDnTemplate 设置为自己的用户 DN 模板，main.ldapRealm.contextFactory.url 设置为自己的 LDAP 服务器域名和端口。设置完成后保存并重启 Knox。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17921/155963142111122_zh-CN.png)

        2.  一般自己的 LDAP 服务不在集群上运行，所以需要开启 Knox 访问公网 LDAP 服务的端口，如：10389。参考 8443 端口的开启步骤，选择**出方向**。

            **说明：** 为了安全原因，这里设置的授权对象必须是您的Knox所在集群的公网 IP，禁止使用 0.0.0.0/0。


## 开始访问 Knox { .section}

-   使用 E-MapReduce 快捷链接访问
    1.  登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com/)。
    2.  单击对应的集群 ID
    3.  在左侧导航栏中单击**集群与服务管理**。
    4.  单击相应的服务，如：HDFS，Yarn等。
    5.  单击右上角的**快捷链接**。
-   使用集群公网 IP 地址访问
    1.  通过集群详情查看公网 IP。
    2.  在浏览器中访问相应服务的 URL。
        -   HDFS UI：[https://\{集群公网ip\}:8443/gateway/cluster-topo/hdfs/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/hdfs/?spm=a2c4g.11186623.2.8.459af364CUTH7M)
        -   Yarn UI：[https://\{集群公网ip\}:8443/gateway/cluster-topo/yarn/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/yarn/?spm=a2c4g.11186623.2.9.459af364CUTH7M)
        -   SparkHistory UI：[https://\{集群公网ip\}:8443/gateway/cluster-topo/sparkhistory/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/sparkhistory/?spm=a2c4g.11186623.2.10.459af364CUTH7M)
        -   Ganglia UI：[https://\{集群公网ip\}:8443/gateway/cluster-topo/ganglia/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/ganglia/?spm=a2c4g.11186623.2.11.459af364CUTH7M)
        -   Storm UI：[https://\{集群公网ip\}:8443/gateway/cluster-topo/storm/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/storm/?spm=a2c4g.11186623.2.12.459af364CUTH7M)
        -   Oozie UI：[https://\{集群公网ip\}:8443/gateway/cluster-topo/oozie/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/oozie/?spm=a2c4g.11186623.2.13.459af364CUTH7M)
    3.  浏览器显示**您的链接不是私密链接**，是因为 Knox 服务使用了自签名证书，请再次确认访问的是自己集群的 IP，且端口为 8443。单击**高级** \> **继续前往**。
    4.  在登录框中输入您在 LDAP 中设置的用户名和密码

## 用户权限管理（ACLs） { .section}

Knox 提供服务级别的权限管理，可以限制特定的用户，特定的用户组和特定的 IP 地址访问特定的服务，可以参见[Apache Knox 授权](https://knox.apache.org/books/knox-0-13-0/user-guide.html?spm=a2c4g.11186623.2.14.459af364CUTH7M#Authorization)。

-   示例：

    -   场景：Yarn UI 只允许用户 Tom 访问。
    -   步骤：在集群配置管理中找到 KNOX 的配置管理，找到 cluster-topo 配置，在 cluter-topo 配置的`<gateway>...</gateway>`标签之间添加 ACLs 代码：

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

    **警告：** Knox 会开放相应服务的 REST API，用户可以通过各服务的 REST API 操作服务，如：HDFS 的文件添加，删除等。出于安全原因，请务必确保在 ECS 控制台开启安全组 Knox 端口 8443 时，授权对象必须是您的一个有效 IP 地址段，禁止使用 0.0.0.0/0；请勿使用 Knox 安装目录下的 LDAP 用户名和密码作为 Knox 的访问用户。


