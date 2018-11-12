# Quick start with Presto {#concept_vpp_y22_z2b .concept}

This article describes the basic usage and application development methods of the Presto database for developers to quickly start application development using Presto database.

## System structure {#section_iqr_cf2_z2b .section}

Presto’s system structure is shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17915/154201561010899_en-US.png)

Presto is a typical M/S architecture system, comprising a Coordinator node and multiple Worker nodes. Coordinator is responsible for the following:

-   Receiving and parsing users’ query requests, generating execution plans, and sending the execution plans to the Worker nodes for execution.
-   Monitoring the running status of the Worker nodes. Each Worker node maintains heartbeat connection with the Coordinator node, reporting the node statuses.
-   Maintaining the MetaStore data

Worker nodes run the tasks assigned by the Coordinator node, read data from external storage systems through connectors, process the data, and send the results to the Coordinator node.

## Basic concepts {#section_wbc_2f2_z2b .section}

This section describes the basic Presto concepts for a better understanding of the Presto work mechanism.

-   Data model

    Data model indicates to the data organization form. Presto uses a three-level structure, namely Catalog, Schema, and Table, to manage data.

    -   Catalog

        A Catalog contains multiple Schemas, and is physically directed to an external data source, which can be accessed through Connectors. When you run a SQL statement in Presto, you are running it against one or more Catalogs.

    -   Schema

        You can take a Schema as a database instance, which contains multiple data tables.

    -   Table

        Data table, which is the same as general database tables.

    Relations among Catalog, Schema, and Table are shown in the following figure.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17915/154201561010900_en-US.png)

-   Connector

    Presto uses Connector to connect to various external data sources. Presto provides a standard [SPI](https://prestodb.io/docs/current/develop/spi-overview.html), which allows users to develop their own Connectors using this standard API, to access customized data sources.

    Generally, a Catalog is associated with a specific Connector \(which can be configured in the Properties file of the Catalog\). Presto contains multiple built-in Connectors. For more information, see Connectors.

-   Query-related concepts

    This section mainly describes related concepts in the Presto query process, for users to understand better, the execution process of Presto statements and the performance optimization methods.

    -   Statement

        Statement refers to an SQL statement entered by a user via JDBC or CLI.

    -   Query

        Query refers to the execution process of a query. When Presto receives an SQL Statement, the Coordinator parses this statement, generates an execution plan, and sends this plan to a Worker for execution. A Query is logically made up by several components, namely Stage, Task, Driver, Split, Operator, and DataSource, which are shown in the following figure:

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17915/154201561010901_en-US.png)

    -   Stage

        A Presto query contains multiple Stages. Stage is a logical concept, which indicates a stage of the query process, comprising one or more execution tasks. Presto uses a tree structure to organize Stages, the root node of which is Single Stage. This Stage aggregates data output from the upstream Stages, and directly sends the results to Coordinator. The leaf node of this tree is Source Stage. The Source Stage receives data from Connector for processing.

    -   Task

        Task refers to a specific task to be executed, and it is the smallest Presto task scheduling unit. During the execution process, Presto task scheduler distributes these tasks to individual Workers for execution. Tasks in one Stage can be executed in parallel. Tasks in two different Stages transmits data via the Exchange module. `Task` is also a logical concept that contains the parameters and contents of the task, the actual task execution is done by the driver.

    -   Driver

        Driver is responsible for executing the specific tasks. A Task may contain multiple Driver instances, to achieve parallel processing within the same Task. Each Driver processes a Split. A Driver is made up by a set of Operators, and is responsible for specific data operations, such as conversion and filtering.

    -   Operator

        The Operator is the smallest execution unit, and is responsible for processing each Page of a Split, such as weighting and conversion. It is similar to logical operators in concept. Page is a column-based data structure, and is the smallest data unit that an Operator can process. A Page object constitutes of multiple Blocks, with each Block representing multiple data rows of a field. A Page can be of a maximum of 1 MB, and can contain data of up to 16 x 1024 rows.

    -   Exchange

        Two Stages exchange data through the Exchange module. The data transmission process is actually is completed between two Tasks. Generally, a downstream Task fetches data from the Output Buffer of an upstream Task using an Exchange Client. The fetched data is then transmitted to Driver in Splits for processing.


## The command line tool {#section_u2b_fg2_z2b .section}

The command line tool uses [SSH to log on to an EMR cluster](intl.en-US/User Guide/Connect to clusters using SSH.md#), and executes the following command to enter the Presto console:

```
$ presto --server emr-header-1:9090 --catalog hive --schema default --user hadoop
```

High-security clusters use the following command form:

```
$ presto  --server https://emr-header-1:7778  \
          --enable-authentication \
          --krb5-config-path /etc/krb5.conf \
          --krb5-keytab-path  /etc/ecm/presto-conf/presto.keytab \
          --krb5-remote-service-name presto \
          --keystore-path /etc/ecm/presto-conf/keystore \
          --keystore-password 81ba14ce6084 \
          --catalog hive --schema default \
          --krb5-principal  presto/emr-header-1.cluster-XXXX@EMR.XXXX.COM
```

-   XXXX are numbers as the ecm id of clusters that can be obtained through cat /etc/hosts .
-   81ba14ce6084 is the default password of /etc/ecm/presto-conf/keystore . It is recommended that you use your own keystore after the deployment.

You can execute the following command from the console:

```
Presto: Default> show schemas;
    schema.
--------------------
default
Hive
information_schema
tpch_100gb_orc
tpch_10gb_orc
tpch_10tb_orc
tpch_1tb_orc
(7 rows)
```

We can execute the presto --help command to obtain help from the console. The parameters and definitions are as follows:

```
--server <server>                       # Specifies the URI of a Coordinator
--user <user>                           # Sets the username
--catalog <catalog>                     # Specifies the default Catalog
--schema <schema>                       # Specifies the default Schema
--execute <execute>                     # Executes a statement and then exits
-f <file>, --file <file>                # Executes an SQL statement and then exits
--debug                                 # Shows debugging information
--client-request-timeout <timeout>      # Specifies the client timeout value, which is 2 minutes by default
--enable-authentication                 # Enables client authentication
--keystore-password <keystore password> # KeyStore password
--keystore-path <keystore path>         # KeyStore path
--krb5-config-path <krb5 config path>   # Kerberos configuration file path (default: /etc/krb5.conf)
--krb5-credential-cache-path <path>     # Kerberos credential cache path
--krb5-keytab-path <krb5 keytab path>   # Kerberos Key table path
--krb5-principal <krb5 principal>       # Kerberos principal to be used
--krb5-remote-service-name <name>       # Remote Kerberos node name
--log-levels-file <log levels>          # Configuration file path for debugging logs
--output-format <output-format>         # Bulk export data format, which is CSV by default
--session <session>                     # Specifies the session attribute, in the format key=value
--socks-proxy <socks-proxy>             # Sets the proxy server
--source <source>                       # Sets query source
--version                               # Shows version info
-h, --help                              # Shows help info
```

## Uses JDBC {#section_ydr_sg2_z2b .section}

Java applications can access databases using the JDBC driver provided by Presto. The usage is basically the same as that of the general RDBMS databases.

-   Introduction into Maven

    You can add the following configuration into the pom file to introduce Presto JDBC driver:

    ```
    <dependency>
        <groupId>com.facebook.presto</groupId>
        <artifactId>presto-jdbc</artifactId>
        <version>0.187</version>
    </dependency>
    ```

-   Driver class name

    Presto JDBC driver class is `com.facebook.presto.jdbc.PrestoDriver`.

-   Connection string

    The following connection string format is supported.

    ```
    jdbc:presto://<COORDINATOR>:<PORT>/[CATALOG]/[SCHEMA]
    ```

    For example:

    ```
    jdbc:presto://emr-header-1:9090               # Connects to data base, using the default Catalog and Schema
    jdbc:presto://emr-header-1:9090/hive          # Connects to data base, using Catalog(hive) and the default Schema
    jdbc:presto://emr-header-1:9090/hive/default  # Connects to data base, using Catalog(hive) and Schema(default)
    ```

-   Connection parameters

    Presto JDBC driver supports various parameters that may be set as URL parameters or as properties passed to DriverManager. Both of the following examples are equivalent:

    Example for passing to DriverManager as Properties :

    ```
    String url = "jdbc:presto://emr-header-1:9090/hive/default";
    Properties properties = new Properties();
    properties.setProperty("user", "hadoop");
    Connection connection = DriverManager.getConnection(url, properties);
    ......
    ```

    Example for passing to DriverManager as URL parameters:

    ```
    String url = "jdbc:presto://emr-header-1:9090/hive/default? user=hadoop";
    Connection connection = DriverManager.getConnection(url);
    ......
    ```

    The parameters are described as follows:

    |Parameter Name|Format|Description|
    |:-------------|:-----|:----------|
    |user|STRING|User Name|
    |password|STRING|Password|
    |Socksproxy|\\:\\|SOCKS proxy server address and port. Example: localhost:1080|
    |httpProxy|\\:\\|HTTP proxy server address and port. Example: localhost:8888|
    |SSL|true\\|Whether or not to use HTTPS for connections. Defaults to false.|
    |SSLTrustStorePath|STRING|Java TrustStore file path|
    |SSLTrustStorePassword|STRING|Java TrustStore password|
    |KerberosRemoteServiceName|STRING|Kerberos service name|
    |KerberosPrincipal|STRING|Kerberos principal|
    |KerberosUseCanonicalHostname|true\\|Whether or not to use the canonical hostname. Defaults to false.|
    |KerberosConfigPath|STRING|Kerberos configuration file path|
    |KerberosKeytabPath|STRING|Kerberos KeyTab file path|
    |KerberosCredentialCachePath|STRING|Kerberos credential cache path|

-   Java example:

    The following is an example of using Presto JDBC driver with Java.

    ```
    .....
    // Loads the JDBC Driver class
    try {
        Class.forName("com.facebook.presto.jdbc.PrestoDriver");
    } catch(ClassNotFoundException e) {
        LOG.ERROR("Failed to load presto jdbc driver.", e);
        System.exit(-1);
    }
    Connection connection = null;
    Statement stmt = null;
    try {
        String url = "jdbc:presto://emr-header-1:9090/hive/default";
        Properties properties = new Properties();
        properties.setProperty("user", "hadoop");
        // Creates the connection object
        Connection = drivermanager. getconnection (URL, properties );
        // Creates the Statement object
        statement = connection.createStatement();
        Executes the query
        ResultSet rs = statement.executeQuery("select * from t1");
        Returns results
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
      // Destroys Statement object
      If (statement! = null) {
          try {
            statement.close();
        } catch(Throwable t) {
            // No-ops
        }
      }
      Closes connection
      if (connection ! = null) {
          try {
            connection.close();
        } catch(Throwable t) {
            // No-ops
        }
      }
    }
    ```


