# Use JDBC to connect to HiveServer2

This topic describes how to use JDBC to connect to HiveServer2 for Hive data access. This method is suitable for the scenarios where you cannot use a Hive client or HDFS to access Hive data.

-   Hive permissions are configured. For more information, see [Hive](/intl.en-US/Cluster types/Hadoop cluster/Ranger/Integrate components with Ranger/Hive.md).
-   A user authentication method is configured. By default, HiveServer2 does not verify accounts and passwords. Therefore, if account and password verification is required, you must configure a user authentication method in advance. For more information, see [How do I set the authentication method of HiveServer2 to LDAP?]().

You can use one of the following JDBC methods to connect to HiveServer2:

-   Use Beeline to connect to HiveServer2. Beeline is the JDBC client of HiveServer2.
-   Write Java code to connect to HiveServer2.

**Note:** In an EMR cluster, the Hue service accesses Hive data over HiveServer2.

JDBC connection strings of HiveServer2 in EMR clusters:

-   Standard cluster: jdbc:hive2://emr-header-1:10000
-   High-security cluster: jdbc:hive2://$\{master1\_fullhost\}:10000/;principal=hive/$\{master1\_fullhost\}@EMR.$id.COM

## Use Beeline to connect to HiveServer2

1.  Log on to the master node of the cluster. For more information, see [Connect to the master node of an EMR cluster in SSH mode](/intl.en-US/Cluster Management/Configure clusters/Connect to a cluster/Connect to the master node of an EMR cluster in SSH mode.md).

2.  Run the following command to access the Beeline client:

    ```
    [root@emr-header-1 ~]# beeline
    ```

    The following information is returned:

    ```
    Beeline version 1.2.1.spark2 by Apache Hive
    ```

3.  Run the following command to connect to HiveServer2:

    ```
    beeline> ! connect jdbc:hive2://emr-header-1:10000
    ```

    The following information is returned:

    ```
    Connecting to jdbc:hive2://emr-header-1:10000
    ```

4.  Enter the username and password.

    ```
    Enter username for jdbc:hive2://emr-header-1:10000: your_username
    Enter password for jdbc:hive2://emr-header-1:10000: your_password
    ```

    The following information is returned:

    ```
    log4j:WARN No such property [datePattern] in org.apache.log4j.RollingFileAppender.
    18/12/17 18:09:16 INFO Utils: Supplied authorities: emr-header-1:10000
    18/12/17 18:09:16 INFO Utils: Resolved authority: emr-header-1:10000
    18/12/17 18:09:16 INFO HiveConnection: Will try to open client transport with JDBC Uri: jdbc:hive2://emr-header-1:10000
    Connected to: Apache Hive (version 2.3.3)
    Driver: Hive JDBC (version 1.2.1.spark2)
    Transaction isolation: TRANSACTION_REPEATABLE_READ
    ```

5.  Query Hive data.

    ```
    0: jdbc:hive2://emr-header-1:10000> select * from emrusers limit 10;
    +------------------+-------------------+------------------+--------------------+--------------+--+
    | emrusers.userid  | emrusers.movieid  | emrusers.rating  | emrusers.unixtime  | emrusers.dt  |
    +------------------+-------------------+------------------+--------------------+--------------+--+
    | 196              | 242               | 3                | 881250949          | 2018100102   |
    | 186              | 302               | 3                | 891717742          | 2018100102   |
    | 22               | 377               | 1                | 878887116          | 2018100102   |
    | 244              | 51                | 2                | 880606923          | 2018100102   |
    | 166              | 346               | 1                | 886397596          | 2018100102   |
    | 298              | 474               | 4                | 884182806          | 2018100102   |
    | 115              | 265               | 2                | 881171488          | 2018100102   |
    | 253              | 465               | 5                | 891628467          | 2018100102   |
    | 305              | 451               | 3                | 886324817          | 2018100102   |
    | 6                | 86                | 3                | 883603013          | 2018100102   |
    +------------------+-------------------+------------------+--------------------+--------------+--+
    10 rows selected (1.455 seconds)
    ```


## Write Java code to connect to HiveServer2

Before you perform the following steps, make sure that you have set up a Java environment, installed a Java programming tool, and configured environment variables.

1.  Configure the project dependencies [hadoop-common](https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-common) and [hive-jdbc](https://mvnrepository.com/artifact/org.apache.hive/hive-jdbc) in the pom.xml file.

    Example:

    ```
    <dependencies>
            <dependency>
                <groupId>org.apache.hive</groupId>
                <artifactId>hive-jdbc</artifactId>
                <version>1.2.1</version>
            </dependency>
            <dependency>
                <groupId>org.apache.hadoop</groupId>
                <artifactId>hadoop-common</artifactId>
                <version>2.7.2</version>
            </dependency>
    </dependencies>
    ```

2.  Write code to connect to HiveServer2 and perform operations on Hive data.

    Example:

    ```
    import java.sql.*;
    
    public class App 
    {
        private static String driverName = "org.apache.hive.jdbc.HiveDriver";
    
        public static void main(String[] args) throws SQLException {
    
            try {
                Class.forName(driverName);
            } catch (ClassNotFoundException e) {
                e.printStackTrace();
            }
    
            Connection con = DriverManager.getConnection(
                    "jdbc:hive2://emr-header-1:10000", "root", "");   // After the code is packaged to a JAR file, you must map emr-header-1 to the public or internal IP address of the EMR cluster in the hosts file on the host for running the JAR file.
    
            Statement stmt = con.createStatement();
    
            String sql = "select * from emrusers limit 10";
            ResultSet res = stmt.executeQuery(sql);
    
            while (res.next()) {
                System.out.println(res.getString(1) + "\t" + res.getString(2));
            }
    
        }
    }
    ```

3.  Package the project to generate a JAR file, and upload the JAR file to the host for running the JAR file.

    **Note:** The [hadoop-common](https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-common) and [hive-jdbc](https://mvnrepository.com/artifact/org.apache.hive/hive-jdbc) dependencies are required to run the JAR file. If the two dependencies are not configured in the environment variables on the host, you must download the dependencies and configure the environment variables on the host. Alternatively, you can package the two dependencies and the project to the same JAR file. If one of the dependencies is missing when you run the JAR file, an error message appears.

    -   If hadoop-common is missing, the error message "java.lang.NoClassDefFoundError: org/apache/hadoop/conf/Configuration" appears.
    -   If hive-jdbc is missing, the error message "java.lang.ClassNotFoundException: org.apache.hive.jdbc.HiveDriver" appears.
    In this example, the JAR file emr-hiveserver2-1.0.jar is generated and uploaded to the **emr-header-1** node of the EMR cluster.

4.  Run the JAR file. Check whether it properly runs.

    **Note:** We recommend that you run the JAR file on a host that is in the same Virtual Private Cloud \(VPC\) and security group as the EMR cluster. Make sure that the host and the EMR cluster can communicate with each other. If the host and the EMR cluster are in different VPCs or of different network types, they can communicate only over a public IP address. In this case, you can also connect them by using an Alibaba Cloud network service. This way, they can communicate over an internal IP address. Use the following methods to test the connectivity:

    -   Internet: telnetPublic IP address of the emr-header-1 node 10000
    -   Internal network: telnetInternal IP address of the emr-header-1 node 10000
    ```
    [root@emr-header-1 xxx-test]# java -jar emr-hiveserver2-1.0.jar
    ```

    The following information is returned:

    ```
    log4j:WARN No appenders could be found for logger (org.apache.hive.jdbc.Utils).
    log4j:WARN Please initialize the log4j system properly.
    log4j:WARN See http://logging.apache.org/log4j/1.2/faq.html#noconfig for more info.
    196    242
    186    302
    22     377
    244    51
    166    346
    298    474
    115    265
    253    465
    305    451
    6      86
    ```


