# 通过JDBC连接HiveServer2来访问Hive数据 {#task_2167243 .task}

本节介绍如何通过JDBC连接HiveServer2来访问Hive数据，适用于无法通过Hive Client和HDFS访问Hive数据的场景。

-   已对Hive进行了权限等配置，详情请参见[Hive 配置](../../../../cn.zh-CN/开源组件介绍/组件授权/RANGER/Hive 配置.md#)。
-   HiveServer2默认不进行用户和密码校验，如果需要用户和密码认证，请预先进行用户认证配置，详情请参见[如何将HiveServer2的认证方式设置为LDAP](../../../../cn.zh-CN/常见问题/如何将HiveServer2的认证方式设置为LDAP.md#)。

本节介绍以下两种JDBC连接HiveServer2的方法：

-   Beeline：HiveServer2的JDBC客户端，E-MapReduce集群已集成，可直接使用。
-   Java：编写Java代码进行连接。

**说明：** 在E-MapReduce集群中，Hue也是通过HiveServer2方式来访问Hive数据的。

在E-MapReduce集群中，HiveServer2的JDBC连接地址如下：

-   标准集群：jdbc:hive2://emr-header-1:10000
-   高安全集群：jdbc:hive2://$\{master1\_fullhost\}:10000/;principal=hive/$\{master1\_fullhost\}@EMR.$id.COM

## Beeline客户端连接HiveServer2 {#section_baz_xsx_szd .section}

1.  [以SSH方式登录E-MapReduce集群的Master主机](../../../../cn.zh-CN/集群规划与配置/集群配置/SSH 登录集群.md#)。
2.  运行beeline命令进入Beeline客户端。 

    ``` {#codeblock_fqv_qt4_vcp}
    [root@emr-header-1 ~]# beeline
    Beeline version 1.2.1.spark2 by Apache Hive
    beeline> 
    ```

3.  运行!connecturl命令连接HiveServer2。 

    ``` {#codeblock_hjl_1tr_j2l}
    beeline> !connect jdbc:hive2://emr-header-1:10000
    Connecting to jdbc:hive2://emr-header-1:10000
    Enter username for jdbc:hive2://emr-header-1:10000:
    ```

4.  输入用户名和密码。 

    ``` {#codeblock_i6p_ne9_af7}
    Enter username for jdbc:hive2://emr-header-1:10000: root
    Enter password for jdbc:hive2://emr-header-1:10000:
    log4j:WARN No such property [datePattern] in org.apache.log4j.RollingFileAppender.
    18/12/17 18:09:16 INFO Utils: Supplied authorities: emr-header-1:10000
    18/12/17 18:09:16 INFO Utils: Resolved authority: emr-header-1:10000
    18/12/17 18:09:16 INFO HiveConnection: Will try to open client transport with JDBC Uri: jdbc:hive2://emr-header-1:10000
    Connected to: Apache Hive (version 2.3.3)
    Driver: Hive JDBC (version 1.2.1.spark2)
    Transaction isolation: TRANSACTION_REPEATABLE_READ
    ```

5.  对Hive数据进行查询等操作。 

    ``` {#codeblock_0pa_ljt_hxx}
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
    0: jdbc:hive2://emr-header-1:10000>
    ```


## Java连接HiveServer2 {#section_npy_8nf_6qk .section}

在进行本操作前，您需要保证已安装了Java环境和Java编程工具，并且配置好了环境变量等。

1.  在pom.xml文件中配置项目依赖（[hadoop-common](https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-common)和[hive-jdbc](https://mvnrepository.com/artifact/org.apache.hive/hive-jdbc)）。 

    本示例新增的项目依赖如下：

    ``` {#codeblock_qdv_4q5_nye}
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

    **说明：** 实际配置时，[hadoop-common](https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-common)版本需要与E-MapReduce集群的[Hadoop版本](../../../../cn.zh-CN/产品简介/产品发行版本说明/产品发行版本总览.md#)一致。

2.  编写代码连接HiveServer2并对Hive数据进行操作。 

    以下是连接HiveServer2并查询Hive数据的一个代码示例：

    ``` {#codeblock_9mx_2yr_vn3}
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
                    "jdbc:hive2://emr-header-1:10000", "root", "");   //代码打包后，运行jar包的环境需要在hosts文件中把emr-header-1映射到E-MapReduce集群的公网IP地址（或内网IP地址）。
    
            Statement stmt = con.createStatement();
    
            String sql = "select * from emrusers limit 10";
            ResultSet res = stmt.executeQuery(sql);
    
            while (res.next()) {
                System.out.println(res.getString(1) + "\t" + res.getString(2));
            }
    
        }
    }
    ```

3.  打包项目工程（即生成jar包），并上传到运行环境。 

    **说明：** jar包的运行需要依赖[hadoop-common](https://mvnrepository.com/artifact/org.apache.hadoop/hadoop-common)和[hive-jdbc](https://mvnrepository.com/artifact/org.apache.hive/hive-jdbc)，如果运行环境的环境变量中未包含这两个依赖包，则您需要下载依赖包并配置环境变量或者您也可直接把这两个依赖包一起打包。运行jar包时，如果缺少这两个依赖包，则会提示以下错误：

    -   缺失hadoop-common：提示java.lang.NoClassDefFoundError: org/apache/hadoop/conf/Configuration
    -   缺失hive-jdbc：提示java.lang.ClassNotFoundException: org.apache.hive.jdbc.HiveDriver
    本示例生成的jar包为emr-hiveserver2-1.0.jar，上传到E-MapReduce集群的**emr-header-1**机器中。

4.  运行jar包，测试是否可正常运行。 

    **说明：** 

    建议运行jar包的机器（实例）与E-MapReduce集群在同一个VPC和安全组下，并且网络可达。如果两者的VPC不同或网络环境不同，则需要通过公网地址访问，或先使用网络产品打通过两者的网络再通过内网访问。网络连通性测试方法：

    -   公网：telnetemr-header-1的公网IP地址 10000
    -   内网：telnetemr-header-1的内网IP地址 10000
    ``` {#codeblock_y8u_qkf_5f7}
    [root@emr-header-1 xxx-test]# java -jar emr-hiveserver2-1.0.jar
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


