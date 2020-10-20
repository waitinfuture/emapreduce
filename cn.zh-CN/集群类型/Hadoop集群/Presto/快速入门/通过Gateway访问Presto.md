# 通过Gateway访问Presto

本文介绍使用HAProxy反向代理，实现通过Gateway节点访问Presto服务。该方法也可以扩展到其他组件，例如Impala。

## 普通集群

普通集群配置Gateway时，只需要配置HAProxy反向代理，以便于对E-MapReduce（EMR）集群上Master节点的Presto Coodrinator的9090端口实现反向代理即可。

1.  配置HAProxy。

    1.  通过SSH登入Gateway节点，详情请参见[使用SSH连接主节点](/cn.zh-CN/集群管理/集群配置/连接集群/使用SSH连接主节点.md)。

    2.  执行如下命令，编辑HAProxy的配置文件/etc/haproxy/haproxy.cfg。

        ```
        vim /etc/haproxy/haproxy.cfg
        ```

    3.  添加如下内容。

        ```
        #---------------------------------------------------------------------
        # Global settings
        #---------------------------------------------------------------------
        global
        ......
        ## 配置代理，将Gateway的9090端口映射到emr-header-1.cluster-xxxx的9090端口。
        listen prestojdbc :9090
            mode tcp
            option tcplog
            balance source
            server presto-coodinator-1 emr-header-1.cluster-xxxx:9090
        ```

2.  保存退出，使用如下命令重启HAProxy服务。

    ```
    service haproxy restart
    ```

3.  配置如下安全组。

    |方向|配置规则|说明|
    |:-|:---|:-|
    |公网入|自定义TCP，开放9090端口。|该端口用于HAProxy代理Master节点Coodinator端口。|


访问Presto服务示例：

-   命令行使用Presto，示例请参见[使用命令行工具](/cn.zh-CN/集群类型/Hadoop集群/Presto/快速入门/使用命令行工具.md)。
-   JDBC访问Presto，示例请参见[使用JDBC](/cn.zh-CN/集群类型/Hadoop集群/Presto/快速入门/使用JDBC.md)。

## 高安全集群

EMR高安全集群中的Presto服务使用Kerberos服务进行认证，其中Kerberos KDC服务位于emr-header-1上，端口为88，支持TCP/UDP协议。 使用Gateway访问高安全集群中的Presto服务，需要同时对Presto Coodinator服务端口和Kerberos KDC实现代理。EMR Presto Coodinator集群，默认使用Keystore配置的CN为emr-header-1，只能在内网使用，因此需要重新生成CN=emr-header-1.cluster-xxx的Keystore。

-   HTTPS认证相关
    1.  创建服务端CN=emr-header-1.cluster-xxx的Keystore。

        ```
        [root@emr-header-1 presto-conf]# keytool -genkey -dname "CN=emr-header-1.cluster-xxx,OU=Alibaba,O=Alibaba,L=HZ, ST=zhejiang, C=CN" -alias server -keyalg RSA -keystore keystore -keypass 81ba14ce6084 -storepass 81ba14ce6084 -validity 36500
        Warning:
        JKS密钥库使用专用格式。建议使用 "keytool -importkeystore -srckeystore keystore -destkeystore keystore -deststoretype pkcs12" 迁移到行业标准格式 PKCS12。
        ```

    2.  导出证书。

        ```
        [root@emr-header-1 presto-conf]# keytool -export -alias server -file server.cer -keystore keystore -storepass 81ba14ce6084
        存储在文件<server.cer>中的证书。
        Warning:
        JKS密钥库使用专用格式。建议使用"keytool -importkeystore -srckeystore keystore -destkeystore keystore -deststoretype pkcs12"迁移到行业标准格式PKCS12。
        ```

    3.  制作客户端Keystore。

        ```
        [root@emr-header-1 presto-conf]# keytool -genkey -dname "CN=myhost,OU=Alibaba,O=Alibaba,L=HZ, ST=zhejiang, C=CN" -alias client -keyalg RSA -keystore client.keystore -keypass 123456 -storepass 123456 -validity 36500
        Warning:
        JKS密钥库使用专用格式。建议使用"keytool -importkeystore -srckeystore client.keystore -destkeystore client.keystore -deststoretype pkcs12" 迁移到行业标准格式 PKCS12。
        ```

    4.  导入证书到客户端Keystore。

        ```
        [root@emr-header-1 presto-conf]# keytool -import -alias server -keystore client.keystore -file server.cer -storepass 123456
        所有者: CN=emr-header-2.cluster-xxx, OU=Alibaba, O=Alibaba, L=HZ, ST=zhejiang, C=CN
        发布者: CN=emr-header-2.cluster-xxx, OU=Alibaba, O=Alibaba, L=HZ, ST=zhejiang, C=CN
        序列号: 4247108
        有效期为 Thu Mar 01 09:11:31 CST 2018 至 Sat Feb 05 09:11:31 CST 2118
        证书指纹:
             MD5:  75:2A:AA:40:01:5B:3F:86:8F:9A:DB:B1:85:BD:44:8A
             SHA1: C7:25:B9:AD:5F:FE:FC:05:8E:A0:24:4A:1C:AA:6A:8D:6C:39:28:16
             SHA256: DB:86:69:65:73:D5:C6:E2:98:7C:4A:3B:31:EF:70:80:F0:3C:3B:0C:14:94:37:9F:9C:22:47:EA:7E:1E:DE:8C
        签名算法名称: SHA256withRSA
        主体公共密钥算法: 2048位RSA密钥
        版本: 3
        扩展:
        #1: ObjectId: 2.5.29.14 Criticality=false
        SubjectKeyIdentifier [
        KeyIdentifier [
        0000: 45 1D A9 C7 D5 4E BB CF   BD CE B4 5E E2 16 FB 2F  E....N.....^.../
        0010: E9 5D 4A B6                                        .]J.
        ]
        ]
        是否信任此证书? [否]:  是
        证书已添加到密钥库中
        Warning:
        JKS密钥库使用专用格式。建议使用"keytool -importkeystore -srckeystore client.keystore -destkeystore client.keystore -deststoretype pkcs12" 迁移到行业标准格式 PKCS12。
        ```

    5.  拷贝生成的文件到客户端。

        ```
        $> scp root@xxx.xxx.xxx.xxx:/etc/ecm/presto-conf/client.keystore ./
        ```

-   Kerberos认证相关
    1.  添加客户端用户Principal。

        ```
        [root@emr-header-1 presto-conf]# sh /usr/lib/has-current/bin/hadmin-local.sh /etc/ecm/has-conf -k /etc/ecm/has-conf/admin.keytab
        [INFO] conf_dir=/etc/ecm/has-conf
        Debug is  true storeKey true useTicketCache false useKeyTab true doNotPrompt true ticketCache is null isInitiator true KeyTab is /etc/ecm/has-conf/admin.keytab refreshKrb5Config is true principal is kadmin/EMR.xxx.COM@EMR.xxx.COM tryFirstPass is false useFirstPass is false storePass is false clearPass is false
        Refreshing Kerberos configuration
        principal is kadmin/EMR.xxx.COM@EMR.xxx.COM
        Will use keytab
        Commit Succeeded
        Login successful for user: kadmin/EMR.xxx.COM@EMR.xxx.COM
        enter "cmd" to see legal commands.
        HadminLocalTool.local: addprinc -pw 123456 clientuser
        Success to add principal :clientuser
        HadminLocalTool.local: ktadd -k /root/clientuser.keytab clientuser
        Principal export to keytab file : /root/clientuser.keytab successful .
        HadminLocalTool.local: exit
        ```

    2.  拷贝生成的文件到客户端。

        ```
        $> scp root@xxx.xxx.xxx.xxx:/root/clientuser.keytab ./
        $> scp root@xxx.xxx.xxx.xxx:/etc/krb5.conf ./
        ```

    3.  修改拷贝到客户端的krb5.conf文件，修改如下两处。

        ```
        [libdefaults]
            kdc_realm = EMR.xxx.COM
            default_realm = EMR.xxx.COM
            # 修改参数为1，使客户端使用TCP协议与KDC通信（因为HAProxy不支持UDP协议）。
            udp_preference_limit = 1 
            kdc_tcp_port = 88
            kdc_udp_port = 88
            dns_lookup_kdc = false
        [realms]
            EMR.xxx.COM = {
                # 设置为Gateway的外网IP。
                kdc = xxx.xxx.xxx.xxx:88
            }
        ```

    4.  修改客户端主机的hosts文件，添加如下内容。

        ```
        #  gateway ip
        xxx.xxx.xxx.xxx emr-header-1.cluster-xxx
        ```

-   配置Gateway HAProxy。
    1.  通过SSH方式登录Gateway节点，修改/etc/haproxy/haproxy.cfg。添加如下内容。

        ```
        #---------------------------------------------------------------------
        # Global settings
        #---------------------------------------------------------------------
        global
        ......
        listen prestojdbc :7778
            mode tcp
            option tcplog
            balance source
            server presto-coodinator-1 emr-header-1.cluster-xxx:7778
        listen kdc :88
            mode tcp
            option tcplog
            balance source
            server emr-kdc emr-header-1:88
        ```

    2.  保存退出，使用如下命令重启HAProxy服务。

        ```
        $> service haproxy restart
        ```

    3.  配置如下安全组规则。

        |方向|配置规则|说明|
        |:-|:---|:-|
        |公网入|自定义UDP，开放88端口。|该端口用于HAProxy代理Master节点上的KDC。|
        |公网入|自定义TCP，开放88端口。|该端口用于HAProxy代理Master节点上的KDC。|
        |公网入|自定义TCP，开放7778端口。|该端口用于HAProxy代理Master节点的Coodinator端口。|

-   使用JDBC访问Presto，代码示例如下。

    ```
    try {
        Class.forName("com.facebook.presto.jdbc.PrestoDriver");
    } catch(ClassNotFoundException e) {
        LOG.error("Failed to load presto jdbc driver.", e);
        System.exit(-1);
    }
    Connection connection = null;
    Statement statement = null;
    try {
        String url = "jdbc:presto://emr-header-1.cluster-59824:7778/hive/default";
        Properties properties = new Properties();
        properties.setProperty("user", "hadoop");
        // https相关配置。
        properties.setProperty("SSL", "true");
        properties.setProperty("SSLTrustStorePath", "resources/59824/client.keystore");
        properties.setProperty("SSLTrustStorePassword", "123456");
        // Kerberos相关配置。
        properties.setProperty("KerberosRemoteServiceName", "presto");
        properties.setProperty("KerberosPrincipal", "clientuser@EMR.59824.COM");
        properties.setProperty("KerberosConfigPath", "resources/59824/krb5.conf");
        properties.setProperty("KerberosKeytabPath", "resources/59824/clientuser.keytab");
        // 创建连接对象。
        connection = DriverManager.getConnection(url, properties);
        // 创建Statement对象。
        statement = connection.createStatement();
        // 执行查询。
        ResultSet rs = statement.executeQuery("select * from table1");
        // 获取结果。
        int columnNum = rs.getMetaData().getColumnCount();
        int rowIndex = 0;
        while (rs.next()) {
            rowIndex++;
            for(int i = 1; i <= columnNum; i++) {
                System.out.println("Row " + rowIndex + ", Column " + i + ": " + rs.getString(i));
            }
        }
    } catch(SQLException e) {
        LOG.error("Exception thrown.", e);
    } finally {
        // 销毁Statement对象。
        if (statement != null) {
            try {
                statement.close();
                } catch(Throwable t) {
                  // No-ops
                }
        }
       // 关闭连接。
       if (connection != null) {
              try {
               connection.close();
           } catch(Throwable t) {
             // No-ops
           }
        }
    }
    ```


