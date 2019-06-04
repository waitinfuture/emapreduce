# 使用 JDBC {#concept_tjs_xzt_xgb .concept}

Java 应用可以使用 Presto 提供的 JDBC driver 连接数据库，使用方式与一般 RDBMS 数据库差别不大。

## 在 Maven 中引入 {#section_mkr_bc5_xgb .section}

可以在 pom 文件中加入如下配置引入 Presto JDBC driver：

```
<dependency>
    <groupId>com.facebook.presto</groupId>
    <artifactId>presto-jdbc</artifactId>
    <version>0.187</version>
</dependency>
```

## Driver 类名 {#section_nlw_2c5_xgb .section}

Presto JDBC driver 类为`com.facebook.presto.jdbc.PrestoDriver`。

## 连接字串 {#section_wx3_hc5_xgb .section}

可以使用如下格式的连接字串：

```
jdbc:presto://<COORDINATOR>:<PORT>/[CATALOG]/[SCHEMA]
```

例如：

```
jdbc:presto://emr-header-1:9090               # 连接数据库，使用默认的Catalog和Schema
jdbc:presto://emr-header-1:9090/hive          # 连接数据库，使用Catalog(hive)和默认的Schema
jdbc:presto://emr-header-1:9090/hive/default  # 连接数据库，使用Catalog(hive)和Schema(default)
```

## 连接参数 {#section_qdt_3c5_xgb .section}

Presto JDBC driver 支持很多参数，这些参数既可以通过 Properties 对象传入，也可以通过 URL 参数传入，这两种方式是等价的。

通过 Properties 对象传入示例：

```
String url = "jdbc:presto://emr-header-1:9090/hive/default";
Properties properties = new Properties();
properties.setProperty("user", "hadoop");
Connection connection = DriverManager.getConnection(url, properties);
......
```

通过 URL 参数传入示例：

```
String url = "jdbc:presto://emr-header-1:9090/hive/default?user=hadoop";
Connection connection = DriverManager.getConnection(url);
......
```

各参数说明如下：

|参数名称|格式|参数说明|
|:---|:-|:---|
|user|STRING|用户名|
|password|STRING|密码|
|socksProxy|\\:\\|SOCKS 代理服务器地址，如 localhost:1080|
|httpProxy|\\:\\|HTTP 代理服务器地址，如 localhost:8888|
|SSL|true\\|是否使用 HTTPS 连接，默认为 false|
|SSLTrustStorePath|STRING|Java TrustStore 文件路径|
|SSLTrustStorePassword|STRING|Java TrustStore 密码|
|KerberosRemoteServiceName|STRING|Kerberos 服务名称|
|KerberosPrincipal|STRING|Kerberos principal|
|KerberosUseCanonicalHostname|true\\|是否使用规范化主机名，默认为 false|
|KerberosConfigPath|STRING|Kerberos 配置文件路径|
|KerberosKeytabPath|STRING|Kerberos KeyTab 文件路径|
|KerberosCredentialCachePath|STRING|Kerberos credential 缓存路径|

## Java 示例 {#section_l5w_4c5_xgb .section}

下面给出一个 Java 使用 Presto JDBC driver 的例子。

```
.....
// 加载JDBC Driver类
try {
    Class.forName("com.facebook.presto.jdbc.PrestoDriver");
} catch(ClassNotFoundException e) {
    LOG.ERROR("Failed to load presto jdbc driver.", e);
    System.exit(-1);
}
Connection connection = null;
Statement statement = null;
try {
    String url = "jdbc:presto://emr-header-1:9090/hive/default";
    Properties properties = new Properties();
    properties.setProperty("user", "hadoop");
    // 创建连接对象
    connection = DriverManager.getConnection(url, properties);
    // 创建Statement对象
    statement = connection.createStatement();
    // 执行查询
    ResultSet rs = statement.executeQuery("select * from t1");
    // 获取结果
    int columnNum = rs.getMetaData().getColumnCount();
    int rowIndex = 0;
    while (rs.next()) {
        rowIndex++;
        for(int i = 1; i <= columnNum; i++) {
            System.out.println("Row " + rowIndex + ", Column " + i + ": " + rs.getInt(i));
        }
    }
} catch(SQLException e) {
    LOG.ERROR("Exception thrown.", e);
} finally {
  // 销毁Statement对象
  if (statement != null) {
      try {
        statement.close();
    } catch(Throwable t) {
        // No-ops
    }
  }
  // 关闭连接
  if (connection != null) {
      try {
        connection.close();
    } catch(Throwable t) {
        // No-ops
    }
  }
}
```

## 使用反向代理 {#section_wgr_525_xgb .section}

可以使用 HAProxy 反向代理 Coodinator，实现通过代理服务访问 Presto 服务。

-   非安全集群代理配置

    非安全集群配置集群代理步骤如下：

    1.  在代理节点安装 HAProxy
    2.  修改 HAProxy 配置\(/etc/haproxy/haproxy.cfg\), 添加如下内容：

        ```
        ......
        
        listen prestojdbc :9090
            mode tcp
            option tcplog
            balance source
            server presto-coodinator-1 emr-header-1:9090
        ```

    3.  重启 HAProxy 服务
    至此，可以使用代理服务器来访问 Presto 了，只需要将连接的服务器 IP 改为代理服务的 IP 即可。


