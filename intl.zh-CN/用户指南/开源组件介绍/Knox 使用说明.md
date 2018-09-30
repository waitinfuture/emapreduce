# Knox 使用说明 {#concept_knp_s1x_y2b .concept}

目前E-MapReduce中支持了[Apache Knox](https://knox.apache.org/)，选择支持Knox的镜像创建集群，完成以下准备工作后，即可在公网直接访问Yarn，HDFS，SparkHistory等服务的Web UI。

## 准备工作 {#section_m1v_wkl_z2b .section}

-   **开启Knox公网IP访问**

    1.  在E-MapReduce上Knox的服务端口是8443，在集群详情中找到集群所在的ECS安全组。
    2.  在ECS控制台修改对应的安全组，在**公网入方向**添加一条规则，打开8443端口。
    **说明：** 

    -   为了安全原因，这里设置的授权对象必须是您的一个有限的ip段范围，禁止使用 0.0.0.0/0。
    -   打开安全组的8443端口之后，该安全组内的所有机器均会打开公网入方向的 8443 端口，包括非E-MapReduce的ecs机器。
-   **设置Knox用户**

    访问Knox时需要对身份进行验证，会要求您输入用户名和密码。Knox的用户身份验证基于LDAP，您可以使用自有LDAP服务，也可以使用集群中的Apache Directory Server的LDAP服务。

    -   **使用集群中的LDAP服务**

        方式一（推荐）

        在[用户管理](https://help.aliyun.com/document_detail/88134.html)中直接添加Knox访问账号。

        方式二

        1.  ssh登录到集群上，详细步骤请参考[SSH登录集群](https://help.aliyun.com/document_detail/28187.html)。
        2.  准备您的用户数据，如：Tom。将文件中所有的emr-guest替换为 Tom，将 cn:EMR GUEST 替换为 cn:Tom，设置 userPassword 的值为您自己的密码。

            ```
            su knox
            cd /usr/lib/knox-current/templates  
            vi users.ldif
            ```

            **说明：** 出于安全原因，导入前务必修改users.ldif的用户密码，即：设置userPassword的值为您自己的用户密码。

        3.  导入到LDAP。

            ```
            su knox
            cd /usr/lib/knox-current/templates
            sh ldap-sample-users.sh
            ```

    -   **使用自有LDAP服务的情况**
        1.  在集群配置管理中找到KNOX的配置管理，在cluster-topo配置中设置两个属性：main.ldapRealm.userDnTemplate与main.ldapRealm.contextFactory.url。main.ldapRealm.userDnTemplate设置为自己的用户DN模板，main.ldapRealm.contextFactory.url设置为自己的LDAP服务器域名和端口。设置完成后保存并重启Knox。

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17921/153829618511122_zh-CN.png)

        2.  一般自己的LDAP服务不在集群上运行，所以需要开启Knox访问公网LDAP服务的端口，如：10389。参考8443端口的开启步骤，选择**公网出方向**。

            **说明：** 为了安全原因，这里设置的授权对象必须是您的Knox所在集群的公网ip，禁止使用0.0.0.0/0。


## 开始访问Knox { .section}

-   **使用E-MapReduce快捷链接访问**
    1.  登录到E-MapReduce管理控制台。
    2.  在“集群与服务管理”页面点击相应的服务，如：HDFS，Yarn等。
    3.  单击右上角的**快捷链接**。
-   **使用集群公网ip地址访问**
    1.  通过集群详情查看公网ip。
    2.  在浏览器中访问相应服务的URL。
        -   HDFS UI：[https://\{集群公网ip\}:8443/gateway/cluster-topo/hdfs/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/hdfs/?spm=a2c4g.11186623.2.8.459af364CUTH7M)
        -   Yarn UI: [https://\{集群公网ip\}:8443/gateway/cluster-topo/yarn/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/yarn/?spm=a2c4g.11186623.2.9.459af364CUTH7M)
        -   SparkHistory UI : [https://\{集群公网ip\}:8443/gateway/cluster-topo/sparkhistory/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/sparkhistory/?spm=a2c4g.11186623.2.10.459af364CUTH7M)
        -   Ganglia UI: [https://\{集群公网ip\}:8443/gateway/cluster-topo/ganglia/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/ganglia/?spm=a2c4g.11186623.2.11.459af364CUTH7M)
        -   Storm UI: [https://\{集群公网ip\}:8443/gateway/cluster-topo/storm/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/storm/?spm=a2c4g.11186623.2.12.459af364CUTH7M)
        -   Oozie UI: [https://\{集群公网ip\}:8443/gateway/cluster-topo/oozie/](https://xn--%7Bip%7D-ch6m5309ab0an44r:8443/gateway/cluster-topo/oozie/?spm=a2c4g.11186623.2.13.459af364CUTH7M)
    3.  浏览器显示**您的链接不是私密链接**，是因为Knox服务使用了自签名证书，请再次确认访问的是自己集群的ip，且端口为8443。单击**高级** \> **继续前往**。
    4.  在登录框中输入您在LDAP中设置的用户名和密码

## 用户权限管理（ACLs） { .section}

Knox提供服务级别的权限管理，可以限制特定的用户，特定的用户组和特定的ip地址访问特定的服务，可以参考[Apache Knox 授权](https://knox.apache.org/books/knox-0-13-0/user-guide.html?spm=a2c4g.11186623.2.14.459af364CUTH7M#Authorization)。

-   示例：
    -   场景：Yarn UI只允许用户Tom访问。
    -   步骤：在集群配置管理中找到KNOX的配置管理，找到cluster-topo配置，在cluter-topo配置的`<gateway>...</gateway>`标签之间添加ACLs代码：

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

-   注意事项：

    Knox会开放相应服务的REST API，用户可以通过各服务的REST API操作服务，如：HDFS的文件添加，删除等。出于安全原因，请务必确保在ecs控制台开启安全组Knox端口8443时，授权对象必须是您的一个有限ip地址段，禁止使用0.0.0.0/0；请勿使用Knox安装目录下的LDAP用户名和密码作为Knox的访问用户。


