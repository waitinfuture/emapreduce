# Introduction to Kerberos {#concept_anp_pgl_z2b .concept}

E-MapReduce supports creating secure clusters from EMR-2.7.x and EMR-3.5.x where open source components in the cluster are started in the Kerberos security mode. In this mode, only authenticated clients can access the cluster service such as HDFS.

## Prerequisites {#section_yrh_qhl_z2b .section}

Kerberos components supported by the current E-MapReduce version are shown in the following table:

|Component name|Component version|
|:-------------|:----------------|
|YARN|2.7.2|
|SPARK|2.1.1/1.6.3|
|HIVE|2.0.1|
|TEZ|0.8.4|
|ZOOKEEPER|3.4.6|
|HUE|3.12.0|
|ZEPPELIN|0.7.1|
|OOZIE|4.2.0|
|SQOOP|1.4.6|
|HBASE|1.1.1|
|PHOENIX|4.7.0|

**Note:** Remarks: Currently Kafka, Presto, and Storm do not support Kerberos.

Create a save cluster

You can turn on **High Security Mode** switch on the software configuration tab of the cluster creation page as shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/20194/154201042430950_en-US.png)

## Kerberos identity authentication principle {#section_gf3_xkl_z2b .section}

Kerberos is an identity authentication protocol based on the symmetric key technology. As an independent third-party identity authentication service, Kerberos can provide its ID authentication function for other services, and it supports SSO \(the client ican access multiple services, such as HBase and HDFS, after ID authentication\).

The Kerberos protocol process is mainly divided into two stages where KDC authenticates the Client identity in the first stage, and the Service authenticates the Client identity in the second stage.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17934/154201042511118_en-US.png)

-   KDC

    Kerberos server

-   Client

    If a user \(principal\) needs to access the service, KDC and Service authenticates the principal’s identity.

-   Service

    Services that have integrated with Kerberos include HDFS, YARN, and HBase.


-   KDC ID authentication

    Before a client user \(principal\) can access a service integrated with Kerberos, it must first pass the KDC ID authentication.

    After passing the KDC ID authentication, the client receives a TGT \(Ticket Granting Ticket\), which can be used to access a service that has integrated Kerberos.

-   Service ID authentication

    When a principal receives the TGT in step 2.1, it can access the Service. It uses the TGT and the name of the service that it must access \(such as HDFS\) to obtain an SGT \(Service Granting Ticket\) from KDC, and use the SGT to access Service, which uses the relevant information to conduct ID authentication on the client. After passing the ID authentication, the client can normally access the Service.


## EMR practice {#section_rls_lml_z2b .section}

Services in the EMR Kerberos security cluster starts in the Kerberos security mode when creating a cluster.

-   The Kerberos server is HasServer
    -   Log on to the [EMR Console](https://emr.console.aliyun.com),choose **Cluster** \> ** ** \> **Configuration Management** \> **HAS**，and conduct operations including View, Modify configuration, and Restart.
    -   Non-HA clusters are deployed on emr-header-1, while HA clusters are deployed on both the emr-header-1 and emr-header-2 nodes.
-   Supports four ID authentication methods

    HasServer supports the following four ID authentication methods. The client can specify the method to be used by HasServer through configuring the related parameters.

    -   ID authentication compatible with MIT Kerberos

        Client configuration:

        ```
        If you want to execute a client request on a cluster node, you must set
        hadoop.security.authentication.use.has in /etc/ecm/hadoop-conf/core-site.xml to false.
        In case of any jobs are running through the execution plan of the console, then values in the /etc/ecm/hadoop-conf/core-site.xml file on the master node must not be modified. Otherwise, the job in the execution plan fails because of the authentication failure. You can follow these steps:
        export HADOOP_CONF_DIR=/etc/has/hadoop-conf Export a temporary environment variable. The hadoop.security.authentication.use.has value under this path has already been set to false.
        ```

        Access method:You can use open source clients to access Service, such as HDFS client. For more information, [click here.](https://help.aliyun.com/document_detail/62966.html?spm=a2c4g.11186623.2.6.10266ca0NDVY35)

    -   RAM ID authentication

        Client configuration:

        ```
        If you want to run a client request on a cluster node, you must set
        hadoop.security.authentication.use.has in /etc/ecm/hadoop-conf/core-site.xml to true, and auth_type in /etc/has/has-client.conf to RAM.
        In case of any jobs are running through the execution plan of the console, then values in the /etc/ecm/hadoop-conf/core-site.xml and /etc/has/has-client.conf files on the master node must not be modified. Otherwise, the job in the execution plan fails because of the authentication failure. You can use the following method:
        export HADOOP_CONF_DIR=/etc/has/hadoop-conf; export HAS_CONF_DIR=/path/to/has-client.conf Export a temporary environment variable, and then set the auth_type in the has-client.conf file of the HAS_CONF_DIR folder to RAM.
        ```

        Access method: The client must use a software package of the cluster \(such as Hadoop and HBase\). For more information, click [here](EN-US_TP_17935.dita#concept_qk1_5pl_z2b).

    -   LDAP ID authentication

        Client configuration:

        ```
        If you want to execute a client request on a cluster node, you must set
        hadoop.security.authentication.use.has in /etc/ecm/hadoop-conf/core-site.xml to true, and auth_type in /etc/has/has-client.conf to LDAP.
        In case of any jobs are running through the execution plan of the console, then values in the /etc/ecm/hadoop-conf/core-site.xml and /etc/has/has-client.conf files on the master node must not be modified. Otherwise, the job in the execution plan fails because of the authentication failure. You can follow these steps:
        export HADOOP_CONF_DIR=/etc/has/hadoop-conf; export HAS_CONF_DIR=/path/to/has-client.conf Export temporary environment viarables, and then set the auth_type in the has-client.conf file of the HAS_CONF_DIR folder to LDAP.
        ```

        Access method: The client must use a software package of the cluster \(such as Hadoop and HBase\). For more information, click[here](EN-US_TP_17937.dita#concept_bbf_bg5_1fb).

    -   Execution plan authentication

        If you have jobs submitted through the execution plan of the EMR console, you must not modify the default configuration of the emr-header-1 node.

        Client configuration:

        ```
        Set hadoop.security.authentication.use.has in /etc/ecm/hadoop-conf/core-site.xml to true, and auth_type in /etc/has/has-client.conf on emr-header-1 to EMR.
        ```

        For more information, click [here](EN-US_TP_17938.dita#concept_x2b_q3v_1fb).

-   Others

    Log on to the master node to access the cluster

    The cluster administrator can also log on to the master node to access the cluster service. The administrator can use the has account \(the default logon method is the MIT-Kerberos-compatible method\) to log on to the master node and access the cluster service, which is convenient to conduct some troubleshooting or O&M tasks.

    ```
    &gt;sudo su has
    &gt;hadoop fs -ls /
    ```

    **Note:** Other accounts can also be used to log on the master node, provided that such accounts have already passed Kerberos authentication. In addition, if you must use the MIT-Kerberos-compatible method on the master node, you must first export an environment variable under this account.

    `export HADOOP_CONF_DIR=/etc/has/hadoop-conf/`


