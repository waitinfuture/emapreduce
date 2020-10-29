# 通过JDBC连接HiveServer2来访问Hive数据

本文介绍如何通过JDBC连接HiveServer2访问Hive数据。适用于无法通过Hive Client和HDFS访问Hive数据的场景。

-   已对Hive进行权限配置，详情请参见[Hive配置](/intl.zh-CN/集群类型/Hadoop集群/Ranger/组件集成/Hive配置.md)。
-   因为HiveServer2默认不校验用户和密码，所以当您需要用户和密码认证时，请进行用户认证配置，详情请参见[如何设置HiveServer2的认证方式为LDAP？]()。

JDBC连接HiveServer2的方法如下：

-   Beeline：通过HiveServer2的JDBC客户端进行连接。
-   Java：编写Java代码进行连接。

**说明：** E-MapReduce集群中，Hue通过HiveServer2方式来访问Hive数据。

在E-MapReduce集群中，HiveServer2的JDBC连接地址如下：

-   标准集群：jdbc:hive2://emr-header-1:10000
-   高安全集群：jdbc:hive2://$\{master1\_fullhost\}:10000/;principal=hive/$\{master1\_fullhost\}@EMR.$id.COM

## Beeline客户端连接HiveServer2

1.  登录集群主节点，详情请参见[使用SSH连接主节点](/intl.zh-CN/集群管理/集群配置/连接集群/使用SSH连接主节点.md)。

2.  执行如下命令，进入Beeline客户端。

    ```
    [root@emr-header-1 ~]# beeline
    ```

    返回如下信息。

    ```
    Beeline version 1.2.1.spark2 by Apache Hive
    ```

3.  执行如下命令，连接HiveServer2。

    ```
    beeline> !connect jdbc:hive2://emr-header-1:10000
    ```

    返回如下信息。

    ```
    Connecting to jdbc:hive2://emr-header-1:10000
    ```

4.  输入用户名和密码。

    ```
    Enter username for jdbc:hive2://emr-header-1:10000: your_username
    Enter password for jdbc:hive2://emr-header-1:10000: your_password
    ```

    返回如下信息。

    ```
    log4j:WARN No such property [datePattern] in org.apache.log4j.RollingFileAppender.
    18/12/17 18:09:16 INFO Utils: Supplied authorities: emr-header-1:10000
    18/12/17 18:09:16 INFO Utils: Resolved authority: emr-header-1:10000
    18/12/17 18:09:16 INFO HiveConnection: Will try to open client transport with JDBC Uri: jdbc:hive2://emr-header-1:10000
    Connected to: Apache Hive (version 2.3.3)
    Driver: Hive JDBC (version 1.2.1.spark2)
    Transaction isolation: TRANSACTION_REPEATABLE_READ
    ```

5.  查询Hive数据。

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


## Java连接HiveServer2

在执行本操作前，确保您已安装Java环境和Java编程工具，并且已配置环境变量。

1.  在pom.xml文件中配置项目依赖（[hadoop-common](https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-common)和[hive-jdbc](https://mvnrepository.com/artifact/org.apache.hive/hive-jdbc)）。

    本示例新增的项目依赖如下：

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

2.  编写代码，连接HiveServer2并操作Hive数据。

    示例如下。

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
                    "jdbc:hive2://emr-header-1:10000", "root", "");   //代码打包后，运行JAR包的环境需要在hosts文件中把emr-header-1映射到E-MapReduce集群的公网IP地址（或内网IP地址）。
    
            Statement stmt = con.createStatement();
    
            String sql = "select * from emrusers limit 10";
            ResultSet res = stmt.executeQuery(sql);
    
            while (res.next()) {
                System.out.println(res.getString(1) + "\t" + res.getString(2));
            }
    
        }
    }
    ```

3.  打包项目工程（即生成JAR包），并上传JAR包至运行环境。

    **说明：** JAR包的运行需要依赖[hadoop-common](https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-common)和[hive-jdbc](https://mvnrepository.com/artifact/org.apache.hive/hive-jdbc)。如果运行环境的环境变量中未包含这两个依赖包，则您需要下载依赖包并配置环境变量，或者直接一起打包这两个依赖包。运行JAR包时，如果缺少这两个依赖包，则会提示以下错误：

    -   缺失hadoop-common：提示java.lang.NoClassDefFoundError: org/apache/hadoop/conf/Configuration
    -   缺失hive-jdbc：提示java.lang.ClassNotFoundException: org.apache.hive.jdbc.HiveDriver
    本示例生成的JAR包为emr-hiveserver2-1.0.jar，上传到E-MapReduce集群的**emr-header-1**。

4.  运行JAR包，测试是否可正常运行。

    **说明：** 运行JAR包的服务器与E-MapReduce集群需要在同一个VPC和安全组下，并且网络可达。如果两者的VPC不同或网络环境不同，则需要通过公网地址访问，或先使用网络产品打通两者的网络，再通过内网访问。网络连通性测试方法：

    -   公网：telnetemr-header-1的公网IP地址 10000
    -   内网：telnetemr-header-1的内网IP地址 10000
    ```
    [root@emr-header-1 xxx-test]# java -jar emr-hiveserver2-1.0.jar
    ```

    返回如下信息。

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


