# 使用 ApacheDS 进行认证 {#concept_bmr_b45_xgb .concept}

Presto 可以对接 LDAP，实现用户密码认证。只需要 Coordinator 节点对接 LDAP 即可

## 主要步骤 {#section_byq_gp5_xgb .section}

1.  配置 ApacheDS，启用 LDAPS
2.  在 ApacheDS 中创建用户信息
3.  配置 Presto Coordinator，重启生效
4.  验证配置

## 启用 LDAPS {#section_fkx_qp5_xgb .section}

1.  创建 ApacheDS 服务端使用的 keystore, 此处密码全部使用 '123456'：

    ```
    ## 创建keystore
    > cd /var/lib/apacheds-2.0.0-M24/default/conf/
    > keytool -genkeypair -alias apacheds -keyalg RSA -validity 7 -keystore ads.keystore
    
    Enter keystore password:
    Re-enter new password:
    What is your first and last name?
      [Unknown]:  apacheds
    What is the name of your organizational unit?
      [Unknown]:  apacheds
    What is the name of your organization?
      [Unknown]:  apacheds
    What is the name of your City or Locality?
      [Unknown]:  apacheds
    What is the name of your State or Province?
      [Unknown]:  apacheds
    What is the two-letter country code for this unit?
      [Unknown]:  CN
    Is CN=apacheds, OU=apacheds, O=apacheds, L=apacheds, ST=apacheds, C=CN correct?
      [no]:  yes
    
    Enter key password for <apacheds>
    	(RETURN if same as keystore password):
    Re-enter new password:
    
    Warning:
    The JKS keystore uses a proprietary format. It is recommended to migrate to PKCS12 which is an industry standard format using "keytool -importkeystore -srckeystore ads.keystore -destkeystore ads.keystore -deststoretype pkcs12".
    
    ## 修改文件用户，否则ApacheDS没有权限读取
    > chown apacheds:apacheds ./ads.keystore
    
    ## 导出证书。
    ## 需要输入密码，密码为上一步设置的值，这里为：123456
    > keytool -export -alias apacheds -keystore ads.keystore -rfc -file apacheds.cer
    Enter keystore password:
    Certificate stored in file <apacheds.cer>
    
    Warning:
    The JKS keystore uses a proprietary format. It is recommended to migrate to PKCS12 which is an industry standard format using "keytool -importkeystore -srckeystore ads.keystore -destkeystore ads.keystore -deststoretype pkcs12".
    
    ## 将证书导入系统证书库，实现自认证
    > keytool -import -file apacheds.cer -alias apacheds -keystore /usr/lib/jvm/java-1.8.0/jre/lib/security/cacerts
    ```

2.  修改配置，启用 LDAPS

    打开 ApacheDS Studio，链接到集群上到 ApacheDS 服务：

    -   DN 设置为：uid=admin,ou=system
    -   密码在此文件中查看：/var/lib/ecm-agent/cache/ecm/service/APACHEDS/2.0.0.1.1/package/files/modifypwd.ldif

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/133788/155711312839736_zh-CN.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/133788/155711312839737_zh-CN.png)

        链接后，打开配置页，启用 LDAPs，将第一步创建的 keystore 设置到相关配置中，保存（ctrl + s）。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/133788/155711312839739_zh-CN.png)

3.  重启 ApacheDS 服务

    登入集群，执行如下命令重启 ApacheDS：

    ```
    > service apacheds-2.0.0-M24-default restart
    ```

    到此，LDAPS 启动， 服务端口是 10636。

    **说明：** ApacheDS Studio 有 Bug，在连接属性页测试 LDAPS 服务连接时会报握手失败，主要是内部默认的超时时间太短导致的，不会影响实际使用。


## 创建用户信息 {#section_rtk_hs5_xgb .section}

本用例在 DN: dc=hadoop,dc=apache,dc=org 下创建相关用户。

1.  创建 dc=hadoop,dc=apache,dc=org 分区，打开配置页，作如下配置，保存（ctrl+s）。重启 ApacheDS 服务生效。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/133788/155711312839740_zh-CN.png)

2.  创建用户

    登入集群，创建如下文件：/tmp/users.ldif

    ```
    # Entry for a sample people container
    # Please replace with site specific values
    dn: ou=people,dc=hadoop,dc=apache,dc=org
    objectclass:top
    objectclass:organizationalUnit
    ou: people
    
    # Entry for a sample end user
    # Please replace with site specific values
    dn: uid=guest,ou=people,dc=hadoop,dc=apache,dc=org
    objectclass:top
    objectclass:person
    objectclass:organizationalPerson
    objectclass:inetOrgPerson
    cn: Guest
    sn: User
    uid: guest
    userPassword:guest-password
    
    # entry for sample user admin
    dn: uid=admin,ou=people,dc=hadoop,dc=apache,dc=org
    objectclass:top
    objectclass:person
    objectclass:organizationalPerson
    objectclass:inetOrgPerson
    cn: Admin
    sn: Admin
    uid: admin
    userPassword:admin-password
    
    # entry for sample user sam
    dn: uid=sam,ou=people,dc=hadoop,dc=apache,dc=org
    objectclass:top
    objectclass:person
    objectclass:organizationalPerson
    objectclass:inetOrgPerson
    cn: sam
    sn: sam
    uid: sam
    userPassword:sam-password
    
    # entry for sample user tom
    dn: uid=tom,ou=people,dc=hadoop,dc=apache,dc=org
    objectclass:top
    objectclass:person
    objectclass:organizationalPerson
    objectclass:inetOrgPerson
    cn: tom
    sn: tom
    uid: tom
    userPassword:tom-password
    
    # create FIRST Level groups branch
    dn: ou=groups,dc=hadoop,dc=apache,dc=org
    objectclass:top
    objectclass:organizationalUnit
    ou: groups
    description: generic groups branch
    
    # create the analyst group under groups
    dn: cn=analyst,ou=groups,dc=hadoop,dc=apache,dc=org
    objectclass:top
    objectclass: groupofnames
    cn: analyst
    description:analyst  group
    member: uid=sam,ou=people,dc=hadoop,dc=apache,dc=org
    member: uid=tom,ou=people,dc=hadoop,dc=apache,dc=org
    
    
    # create the scientist group under groups
    dn: cn=scientist,ou=groups,dc=hadoop,dc=apache,dc=org
    objectclass:top
    objectclass: groupofnames
    cn: scientist
    description: scientist group
    member: uid=sam,ou=people,dc=hadoop,dc=apache,dc=or
    ```

    执行如下命令，导入用户：

    ```
    > ldapmodify -x -h localhost -p 10389 -D "uid=admin,ou=system" -w {密码} -a -f /tmp/users.ldif
    ```

    执行完成后，可以在 ApacheDS Studio 上看到相关到用户，如下所示：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/133788/155711312839753_zh-CN.png)


## 配置 Presto {#section_dll_zrz_xgb .section}

1.  开启 Coordinator Https
    1.  创建 Presto coordinator 使用的 keystore

        ```
        ## 使用EMR自带的脚本生成keystore
        ## keystore地址: /etc/ecm/presto-conf/keystore
        ## keystore密码: 81ba14ce6084
        > expect /var/lib/ecm-agent/cache/ecm/service/PRESTO/0.208.0.1.2/package/files/tools/gen-keystore.exp
        ```

    2.  配置 Presto coordinator配置

        编辑/etc/ecm/presto-conf/config.properties， 加入如下内容：

        ```
        http-server.https.enabled=true
        http-server.https.port=7778
        
        http-server.https.keystore.path=/etc/ecm/presto-conf/keystore
        http-server.https.keystore.key=81ba14ce6084
        ```

2.  配置认证模式，接入 ApacheDS

    1.  编辑/etc/ecm/presto-conf/config.properties， 加入如下内容：

        ```
        http-server.authentication.type=PASSWORD
        ```

    2.  编辑jvm.config， 加入如下内容：

        ```
        -Djavax.net.ssl.trustStore=/usr/lib/jvm/java-1.8.0/jre/lib/security/cacerts
        -Djavax.net.ssl.trustStorePassword=changeit
        ```

    3.  创建password-authenticator.properties，加入如下内容：

        ```
        password-authenticator.name=ldap
        ldap.url=ldaps://emr-header-1.cluster-84423:10636
        ldap.user-bind-pattern=uid=${USER},ou=people,dc=hadoop,dc=apache,dc=org
        ```

    4.  创建jndi.properties， 加入如下内容：

        ```
        java.naming.security.principal=uid=admin,ou=system
        java.naming.security.credentials={密码}
        java.naming.security.authentication=simple
        ```

    5.  将jndi.properties打包到 jar 包中，复制到 presto 库文件目录中：

        ```
        jar -cvf jndi-properties.jar jndi.properties
        > cp ./jndi-properties.jar /etc/ecm/presto-current/lib/
        ```

    **说明：** 

    -   下面 3 个参数用于登入 LDAP 服务。然而，在 Presto 上没地方配置这几个参数。分析源码可以先将这几个参数加到 jvm 参数里，并不会生效。（会被过滤掉）： java.naming.security.principal=uid=admin,ou=system java.naming.security.credentials=ZVixyOY+5k java.naming.security.authentication=simple
    -   进一步分析代码，发现 JNDI 库会用 classload 加载 jndi.properties 这个资源文件，因此可以将这几个参数放到 jndi.properties 这个文件中；
    -   Presto 的 launcher 只会把 jar 文件加到 classpath 里，所以还需把这个 jndi.properties 打成 jar 包，拷贝到 lib 目录中。
3.  重启 Presto，至此完成所有配置

## 验证配置 {#section_ipx_4vz_xgb .section}

使用 Presto cli 验证配置是否生效。

```
## 使用用户sam，输入正确的密码
> presto  --server https://emr-header-1:7778  --keystore-path /etc/ecm/presto-conf/keystore --keystore-password 81ba14ce6084 --catalog hive --schema default --user sam --password
Password: <输入了正确的密码>
presto:default> show schemas;
              Schema
----------------------------------
 tpcds_bin_partitioned_orc_5
 tpcds_oss_bin_partitioned_orc_10
 tpcds_oss_text_10
 tpcds_text_5
 tst
(5 rows)

Query 20181115_030713_00002_kp5ih, FINISHED, 3 nodes
Splits: 36 total, 36 done (100.00%)
0:00 [20 rows, 331B] [41 rows/s, 694B/s]

## 使用用户sam，输入错误的密码
> presto  --server https://emr-header-1:7778  --keystore-path /etc/ecm/presto-conf/keystore --keystore-password 81ba14ce6084 --catalog hive --schema default --user sam --password
Password: <输入了错误的密码>
presto:default> show schemas;
Error running command: Authentication failed: Access Denied: Invalid credentials
```

