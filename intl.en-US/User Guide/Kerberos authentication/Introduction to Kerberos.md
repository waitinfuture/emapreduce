# Introduction to Kerberos {#concept_anp_pgl_z2b .concept}

Kerberos is a network authentication protocol that allows nodes communicating over a non-secure network to securely prove their identity. From versions 2.7.x and 3.5.x onwards, E-MapReduce supports the creation of clusters in which open source components are started in the Kerberos security mode. In this mode, only authenticated clients can access the cluster service, such as HDFS.

## Prerequisites {#section_yrh_qhl_z2b .section}

The Kerberos components supported by the latest E-MapReduce version are shown in the following table:

|Component name|Component version|
|:-------------|:----------------|
|YARN|2.7.2|
|Spark|2.1.1/1.6.3|
|Hive|2.0.1|
|Tez|0.8.4|
|ZooKeeper|3.4.6|
|Hue|3.12.0|
|Zeppelin|0.7.1|
|Oozie|4.2.0|
|Sqoop|1.4.6|
|HBase|1.1.1|
|Phoenix|4.7.0|

**Note:** Kafka, Presto, and Storm do not currently support Kerberos.

Create a security cluster

In the software configuration tab on the cluster creation page, you can turn on **High Security Mode**, as shown in the following figure:

![Create a security cluster](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/20194/155255222930950_en-US.png)

## Kerberos authentication {#section_gf3_xkl_z2b .section}

Kerberos is an identity authentication protocol based on symmetric key cryptography. As a third-party authentication service, Kerberos can provide its authentication function for other services. It also supports SSO, and the client can access multiple services, such as HBase and HDFS, after authentication.

The Kerberos protocol process is mainly divided into two stages: the KDC authenticates the client ID, and the service authenticates the client ID.

![Kerberos authentication](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17934/155255222911118_en-US.png)

-   KDC

    Kerberos server

-   Client

    If a user \(principal\) needs to access the service, the KDC and service authenticate the principal's identity.

-   Service

    Services that have integrated with Kerberos include HDFS, YARN, and HBase.


-   KDC ID authentication

    Before a principal can access a service integrated with Kerberos, it must first pass KDC ID authentication.

    After doing so, the client receives a ticket-granting ticket \(TGT\), which can be used to access a service that has integrated Kerberos.

-   Service ID authentication

    When a principal receives the TGT, it can access the service. It uses the TGT and the name of the service that it must access \(such as HDFS\) to obtain a service-granting ticket \(SGT\) from the KDC, and uses the SGT to access the service. This then uses the relevant information to conduct ID authentication on the client. After passing authentication, the client can access the service as normal.


## Kerberos and E-MapReduce {#section_rls_lml_z2b .section}

When you create a cluster, services in the E-MapReduce Kerberos security cluster start in the Kerberos security mode.

-   The Kerberos server is a HasServer
    -   Log on to the [Alibaba Cloud E-MapReduce console](https://partners-intl.console.aliyun.com/#/emr), choose **Cluster** \> ** ** \> **Configuration Management** \> **HAS**, and conduct operations such as view, modify configuration, and restart.
    -   Non-HA clusters are deployed on the emr-header-1 node, whereas HA clusters are deployed on both the emr-header-1 and emr-header-2 nodes.
-   Supports four ID authentication methods

    HasServer supports the following four ID authentication methods. The client can specify the method that is used by HasServer by configuring the relevant parameters.

    -   ID authentication compatible with MIT Kerberos

        Client configuration:

        ```
        If you want to execute a client request on a cluster node, you must set
        hadoop.security.authentication.use.has in /etc/ecm/hadoop-conf/core-site.xml to false.
        In case of any jobs are running through the execution plan of the console, then values in the /etc/ecm/hadoop-conf/core-site.xml file on the master node must not be modified. Otherwise, the job in the execution plan fails because of the authentication failure. You can follow these steps:
        export HADOOP_CONF_DIR=/etc/has/hadoop-conf Export a temporary environment variable. The hadoop.security.authentication.use.has value under this path has already been set to false.
        ```

        Access method: You can use open source clients to access the service, such as an HDFS client. For more information, [click here](EN-US_TP_17935.dita#concept_qk1_5pl_z2b).

    -   RAM ID authentication

        Client configuration:

        ```
        If you want to run a client request on a cluster node, you must set
        hadoop.security.authentication.use.has in /etc/ecm/hadoop-conf/core-site.xml to true, and auth_type in /etc/has/has-client.conf to RAM.
        In case of any jobs are running through the execution plan of the console, then values in the /etc/ecm/hadoop-conf/core-site.xml and /etc/has/has-client.conf files on the master node must not be modified. Otherwise, the job in the execution plan fails because of the authentication failure. You can use the following method:
        export HADOOP_CONF_DIR=/etc/has/hadoop-conf; export HAS_CONF_DIR=/path/to/has-client.conf Export a temporary environment variable, and then set the auth_type in the has-client.conf file of the HAS_CONF_DIR folder to RAM.
        ```

        Access method: The client must use a software package of the cluster, such as Hadoop or HBase. For more information, [click here](EN-US_TP_17936.dita#concept_nsg_mb5_1fb).

    -   LDAP ID authentication

        Client configuration:

        ```
        If you want to execute a client request on a cluster node, you must set
        hadoop.security.authentication.use.has in /etc/ecm/hadoop-conf/core-site.xml to true, and auth_type in /etc/has/has-client.conf to LDAP.
        In case of any jobs are running through the execution plan of the console, then values in the /etc/ecm/hadoop-conf/core-site.xml and /etc/has/has-client.conf files on the master node must not be modified. Otherwise, the job in the execution plan fails because of the authentication failure. You can follow these steps:
        export HADOOP_CONF_DIR=/etc/has/hadoop-conf; export HAS_CONF_DIR=/path/to/has-client.conf Export temporary environment viarables, and then set the auth_type in the has-client.conf file of the HAS_CONF_DIR folder to LDAP.
        ```

        Access method: The client must use a software package of the cluster, such as Hadoop or HBase. For more information, [click here](EN-US_TP_17937.dita#concept_bbf_bg5_1fb).

    -   Execution plan authentication

        If you have jobs submitted through the execution plan of the E-MapReduce console, you must not modify the default configuration of the emr-header-1 node.

        Client configuration:

        ```
        Set hadoop.security.authentication.use.has in /etc/ecm/hadoop-conf/core-site.xml to true, and auth_type in /etc/has/has-client.conf on emr-header-1 to EMR.
        ```

        For more information, [click here](EN-US_TP_17938.dita#concept_x2b_q3v_1fb).

-   Others

    Log on to the master node to access the cluster

    The administrator can use the Has account \(the default logon method is MIT-Kerberos-compatible\) to log on to the master node and access the cluster service. This facilitates troubleshooting and O&M tasks.

    ```
    &gt;sudo su has
    &gt;hadoop fs -ls /
    ```

    **Note:** Other accounts can also be used to log on to the master node, provided that such accounts have already passed Kerberos authentication. In addition, if you have to use the MIT-Kerberos-compatible method on the master node, you must first export an environment variable under this account.

    `export HADOOP_CONF_DIR=/etc/has/hadoop-conf/`


