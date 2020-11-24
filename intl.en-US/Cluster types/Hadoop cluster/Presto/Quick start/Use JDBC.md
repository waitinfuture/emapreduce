# Use JDBC

Java applications can connect to databases by using the Java Database Connectivity \(JDBC\) driver provided by Presto.

## Use Maven to connect to the Presto JDBC driver

You can add the following configurations to the pom.xml file to connect to the Presto JDBC driver:

```
<dependency>
    <groupId>com.facebook.presto</groupId>
    <artifactId>presto-jdbc</artifactId>
    <version>0.187</version>
</dependency>
```

## Driver class name

The class of the Presto JDBC driver is `com.facebook.presto.jdbc.PrestoDriver`.

## Connection string

The following connection string format is supported:

```
jdbc:presto://<COORDINATOR>:<PORT>/[CATALOG]/[SCHEMA]
```

Examples:

```
jdbc:presto://emr-header-1:9090               # Connect to a database by using a specific catalog and schema.
jdbc:presto://emr-header-1:9090/hive          # Connect to a database by using the hive catalog and a specific schema.
jdbc:presto://emr-header-1:9090/hive/default  # Connect to a database by using the hive catalog and the default schema.
```

## Connection parameters

The Presto JDBC driver supports various parameters that can be configured as URL parameters or as properties and passed to DriverManager.

-   Example of passing parameters to DriverManager as properties:

    ```
    String url = "jdbc:presto://emr-header-1:9090/hive/default";
    Properties properties = new Properties();
    properties.setProperty("user", "hadoop");
    Connection connection = DriverManager.getConnection(url, properties);
    ......
    ```

-   Example of passing parameters to DriverManager as URL parameters:

    ```
    String url = "jdbc:presto://emr-header-1:9090/hive/default? user=hadoop";
    Connection connection = DriverManager.getConnection(url);
    ......
    ```


The following table describes the connection parameters.

|Parameter|Format|Description|
|:--------|:-----|:----------|
|user|STRING|The username.|
|password|STRING|The password.|
|socksProxy|\\:\\|The address and port of the SOCKS proxy server. Example: localhost:1080.|
|httpProxy|\\:\\|The address and port of the HTTP proxy server. Example: localhost:8888.|
|SSL|true\\|Specifies whether to use HTTPS for connections. Default value: false.|
|SSLTrustStorePath|STRING|The path used to store the Java truststore file.|
|SSLTrustStorePassword|STRING|The password of the Java truststore file.|
|KerberosRemoteServiceName|STRING|The name of the remote Kerberos service.|
|KerberosPrincipal|STRING|The Kerberos principal name.|
|KerberosUseCanonicalHostname|true\\|Specifies whether to use a normalized hostname. Default value: false.|
|KerberosConfigPath|STRING|The path used to store the Kerberos configuration file.|
|KerberosKeytabPath|STRING|The path used to store the Kerberos keytab file.|
|KerberosCredentialCachePath|STRING|The path used to cache the Kerberos credential.|

## Java example

In this example, the Presto JDBC driver is used for Java.

```
.....
// Load the class of the Presto JDBC driver.
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
    // Create a connection object.
    connection = DriverManager.getConnection(url, properties);
    // Create a statement object.
    statement = connection.createStatement();
    // Execute a query statement.
    ResultSet rs = statement.executeQuery("select * from t1");
    // Return results.
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
  // Destroy the statement object.
  if (statement ! = null) {
      try {
        statement.close();
    } catch(Throwable t) {
        // No-ops
    }
  }
  // Close the connection.
  if (connection ! = null) {
      try {
        connection.close();
    } catch(Throwable t) {
        // No-ops
    }
  }
}
```

## Use a reverse proxy

You can use the HAProxy reverse proxy Coodinator to access the Presto service.

To configure a proxy for a non-security cluster, perform the following steps:

1.  Install HAProxy on the proxy node.
2.  Add the following content to the haproxy.cfg file in the /etc/haproxy/ directory:

    ```
    ......
    
    listen prestojdbc :9090
        mode tcp
        option tcplog
        balance source
        server presto-coodinator-1 emr-header-1:9090
    ```

3.  Restart HAProxy.

