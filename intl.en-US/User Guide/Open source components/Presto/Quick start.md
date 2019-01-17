# Quick start {#concept_vpp_y22_z2b .concept}

This section provides an overview of Presto and describes to use it to develop applications.

## Architecture {#section_iqr_cf2_z2b .section}

The following figure shows the architecture of Presto:

![architecture of Presto](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17915/154770981110899_en-US.png)

Presto has a typical mobile/server architecture comprising a coordinator node and multiple worker nodes. Coordinator is responsible for the following:

-   Receiving and parsing your query requests, generating execution plans, and sending the execution plans to the worker nodes for execution.
-   Monitoring the running status of the worker nodes. Each worker node maintains a heartbeat connection with the coordinator node, reporting the node statuses.
-   Maintaining the metastore data

Worker nodes run the tasks assigned by the coordinator node, read data from external storage systems through connectors, process the data, and send the results to the coordinator node.

## Basic concepts {#section_wbc_2f2_z2b .section}

The basic concepts of Presto are as follows:

-   Data model

    The data model indicates the data organization form. To manage data, Presto uses a three-level structure that consists of catalogs, schemas, and tables.

    -   Catalog

        A catalog contains multiple schemas and is physically directed to an external data source, which can be accessed through connectors. When you run an SQL statement in Presto, you are running it against one or more catalogs.

    -   Schema

        A schema is a database instance that contains multiple data tables.

    -   Table

        A data table is the same as a general database table.

    The relationships between catalogs, schemas, and tables are shown in the following figure.

    ![The relationships between catalogs, schemas, and tables](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17915/154770981110900_en-US.png)

-   Connector

    Presto uses connectors to connect to various external data sources. To access customized data sources, Presto provides a standard [SPI](https://prestodb.io/docs/current/develop/spi-overview.html), which allows you to develop your own connectors using this standard API.

    A catalog is typically associated with a specific connector \(which can be configured in the Properties file of the catalog\). Presto contains multiple built-in connectors.

-   Query-related concepts
    -   Statement

        Statement refers to an SQL statement that you enter via JDBC or CLI.

    -   Query

        Query refers to the execution process of a query. When Presto receives an SQL statement, the coordinator parses this statement, generates an execution plan, and sends this plan to a worker for execution. A query is logically made up of several components, namely stages, tasks, drivers, splits, operators, and data sources, which are shown in the following figure.

        ![Components of query](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17915/154770981110901_en-US.png)

    -   Stage

        A Presto query contains multiple stages. A stage is a logical concept that indicates the stage of a query process, and comprises one or more execution tasks. Presto uses a tree-like structure to organize stages, the root node of which is Single Stage. This stage aggregates data output from the upstream stages and sends the results to the coordinator. The leaf node of this tree is Source Stage. This stage receives data from the connector for processing.

    -   Task

        A task refers to a specific task to be executed and is the smallest Presto task scheduling unit. During the execution process, the Presto task scheduler distributes these tasks to individual workers for execution. Tasks in one stage can be executed in parallel. Tasks in two different stages transmit data by means of the exchange module. `Task` is also a logical concept that contains the parameters and contents of the task. The actual task execution is done by the driver.

    -   Driver

        Driver is responsible for executing the specific tasks. A task may contain multiple driver instances so as to achieve parallel processing within the same task. Each driver processes a split. A driver is made up of a set of operators and is responsible for specific data operations, such as conversion and filtering.

    -   Operator

        The operator is the smallest execution unit and is responsible for processing each page of a split, such as weighting and conversion. It is similar to a logical operator in concept. A page is a column-based data structure, and is the smallest data unit that an operator can process. A page object consists of multiple blocks, with each block representing multiple data rows of a field. Pages can be of a maximum of 1 MB and can contain data of up to 16 x 1024 rows.

    -   Exchange

        Two stages exchange data through the exchange module. The data transmission process is completed between two tasks. A downstream task typically fetches data from the output buffer of an upstream task using an exchange client. The fetched data is then transmitted to driver in splits for processing.


## Command line tool {#section_u2b_fg2_z2b .section}

The command line tool uses [SSH to log on to an EMR cluster](reseller.en-US/User Guide/Connect to clusters using SSH.md#) and executes the following command to enter the Presto console:

```
$ presto --server emr-header-1:9090 --catalog hive --schema default --user hadoop
```

High-security clusters use the following command:

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

-   XXXX is the ECM ID of the cluster, a string of numbers that can be obtained through cat /etc/hosts .
-   81ba14ce6084 is the default password of /etc/ecm/presto-conf/keystore . We recommend that you use your own keystore after deployment.

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

You can then execute the presto --help command to obtain help from the console. The parameters and definitions are as follows:

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

Java applications can access databases using the JDBC driver provided by Presto. The procedure is the same as that for general RDBMS databases.

-   Introduction to Maven

    You can add the following configuration to the POM file to introduce the Presto JDBC driver:

    ```
    <dependency>
        <groupId>com.facebook.presto</groupId>
        <artifactId>presto-jdbc</artifactId>
        <version>0.187</version>
    </dependency>
    ```

-   Driver class name

    The Presto JDBC driver class is `com.facebook.presto.jdbc.PrestoDriver`.

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

    The Presto JDBC driver supports various parameters that may be set as URL parameters or as Properties and passed to DriverManager.

    Example of passing parameters to DriverManager as Properties:

    ```
    String url = "jdbc:presto://emr-header-1:9090/hive/default";
    Properties properties = new Properties();
    properties.setProperty("user", "hadoop");
    Connection connection = DriverManager.getConnection(url, properties);
    ......
    ```

    Example of passing parameters to DriverManager as URL parameters:

    ```
    String url = "jdbc:presto://emr-header-1:9090/hive/default? user=hadoop";
    Connection connection = DriverManager.getConnection(url);
    ......
    ```

    The parameters are described as follows:

    |Parameter name|Format|Description|
    |:-------------|:-----|:----------|
    |user|STRING|User name.|
    |password|STRING|Password.|
    |Socksproxy|\\:\\|SOCKS proxy server address and port. For example, localhost:1080.|
    |httpProxy|\\:\\|HTTP proxy server address and port. For example, localhost:8888.|
    |SSL|true\\|Whether or not to use HTTPS for connections. This is false by default.|
    |SSLTrustStorePath|STRING|Java TrustStore file path.|
    |SSLTrustStorePassword|STRING|Java TrustStore password.|
    |KerberosRemoteServiceName|STRING|Kerberos service name.|
    |KerberosPrincipal|STRING|Kerberos principal.|
    |KerberosUseCanonicalHostname|true\\|Whether or not to use the canonical hostname. This is false by default.|
    |KerberosConfigPath|STRING|Kerberos configuration file path.|
    |KerberosKeytabPath|STRING|Kerberos KeyTab file path.|
    |KerberosCredentialCachePath|STRING|Kerberos credential cache path|

-   Java example

    The following is an example of using the Presto JDBC driver with Java.

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


