# Create a Gateway {#concept_b4j_pvn_1fb .concept}

A Gateway is an ECS instance in the same intranet as the E-MapReduce cluster. You can use a Gateway to achieve load balancing and security isolation, or submit jobs to the E-MapReduce cluster.

You can use the following methods to create a Gateway.

-   \(Recommended\) Use the [Alibaba Cloud E-MapReduce console](https://partners-intl.console.aliyun.com/#/emr).
-   Create a Gateway manually.

## Create a Gateway on the E-MapReduce console {#section_jlz_nxn_1fb .section}

E-MapReduce Gateway only supports E-MapReduce Hadoop clusters. Before creating a Gateway, make sure that you have created an E-MapReduce Hadoop cluster. To create a Gateway, perform the following steps.

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://partners-intl.console.aliyun.com/#/emr).
2.  Click **Create Gateway**.
3.  Perform configurations on the **Create Gateway** page.
    -   **Cluster Name**: the name of the Gateway. The length can be 1 to 64 characters. It only allows Chinese characters, letters, numbers, hyphens \(-\), and underscores \(\_\).
    -   **Password/Key Pair**:
        -   **Password**: the password used to log on to the Gateway. The password must be 8 to 30 characters in length and can contain uppercase letters, lowercase letters, numbers , and special characters including exclamation marks \(!\), at signs \(@\), number signs \(\#\), dollar signs \($\), percent signs \(%\), ampersands \(&\), and asterisks \(\*\).
        -   **Key Pair**: the name of the key pair used to log on to the Gateway. If no key pair has been created, click **Create Key Pair**to go to the ECS console for creating a key pair. Keep the private key file \(PEM format\) safe. After the Gateway is created, the public key is bounded to the ECS instance on which the Gateway is deployed. When you log on to the Gateway by using SSH, enter the private key provided by the private key file.
    -   **Billing Method** 
        -   **Subscription**: one-time payment for a period of time. You get a lower price by choosing Subscription as the billing method. A big discount is offered for a three-year Subscription.
        -   **Pay-As-You-Go**: payment is made according to the number of hours actually used and is calculated on an hourly basis.
    -   **Associated Cluster**: the cluster to which the Gateway can submit jobs. The Gateway is automatically configured with the same Hadoop environment as the cluster.
    -   **Zone**: the zone in which the associated cluster is.
    -   **Network Type**: the network type used by the associated cluster.
    -   **VPC**: the VPC where the associated cluster is in.
    -   **VSwitch**: a VSwitch in the VPC.
    -   **Security Group Name**: the security group to which the associated cluster belongs.
    -   **Assign Public Network IP**: indicates whether the Gateway is assigned with a public network IP address.
    -   **Gateway Instance**: the available **ECS instance specifications** in the region.
    -   **System Disk Type**: the system disk type that the Gateway node uses. System disk types include SSD and Ultra Disk. The system disk is released with the release of the cluster by default.
    -   **Disk Size**: the minimum is 40 GB and the maximum is 500 GB. The default value is 300 GB.
    -   **Data Disk Type**: the data disk type that the Gateway uses. Data disk types include SSD and Ultra Disk. The data disk is released with the release of the cluster by default.
    -   **Disk Size**: the minimum is 200 GB and the maximum is 4,000 GB. The default value is 300 seconds.
    -   **Count**: the number of data disks. The minimum is 1 and the maximum is 10.
4.  Click **Create** to save the configurations.

    The new Gateway is displayed in the cluster list and **Idle** appears in the **Status** column.


## Create a Gateway manually { .section}

-   Network environment

    Make sure that the Gateway node is in the security group of the corresponding E-MapReduce cluster and the Gateway node can access the E-MapReduce cluster. For more information, see [Create a security group](../../../../reseller.en-US/Security/Security groups/Create a security group.md#).

-   Software environment
    -   System environment: we recommend that you use CentOS 7.2 or later versions.
    -   Java environment: JDK 1.7 or later versions. We recommend that you use OpenJDK 1.8.0.
-   Procedure
    -   E-MapReduce V2.7 and later versions or V3.2 and later versions

        We recommend that you use the E-MapReduce console to create a Gateway when you use these versions of EMR.

        If you want to create a Gateway manually, create a script file, enter the following commands, and then run it on the Gateway node. Run the sh deploy.sh <masteri\_ip\> master\_password\_file command.

        -   deploy.sh: the script name. The commands are as follows.
        -   masteri\_ip: the IP address of the master node in the cluster. Make sure that the IP address can be accessed.
        -   master\_password\_file: the file that stores the password of the master node.
        ```
        #! /usr/bin/bash
        if [ $# ! = 2 ]
        then
           echo "Usage: $0 master_ip master_password_file"
           exit 1;
        fi
        masterip=$1
        masterpwdfile=$2
        if ! type sshpass >/dev/null 2>&1; then
           yum install -y sshpass
        fi
        if ! type java >/dev/null 2>&1; then
           yum install -y java-1.8.0-openjdk
        fi
        mkdir -p /opt/apps
        mkdir -p /etc/ecm
        echo "Start to copy package from $masterip to local gateway(/opt/apps)"
        echo " -copying hadoop-2.7.2"
        sshpass -f $masterpwdfile scp -r -o 'StrictHostKeyChecking no' root@$masterip:/usr/lib/hadoop-current /opt/apps/
        echo " -copying hive-2.0.1"
        sshpass -f $masterpwdfile scp -r root@$masterip:/usr/lib/hive-current /opt/apps/
        echo " -copying spark-2.1.1"
        sshpass -f $masterpwdfile scp -r root@$masterip:/usr/lib/spark-current /opt/apps/
        echo "Start to link /usr/lib/\${app}-current to /opt/apps/\${app}"
        if [ -L /usr/lib/hadoop-current ]
        then
           Unlink/usr/lib/hadoop-Current
        fi
        ln -s /opt/apps/hadoop-current  /usr/lib/hadoop-current
        if [ -L /usr/lib/hive-current ]
        then
           unlink /usr/lib/hive-current
        fi
        ln -s /opt/apps/hive-current  /usr/lib/hive-current
        if [ -L /usr/lib/spark-current ]
        then
           unlink /usr/lib/spark-current
        fi
        ln -s /opt/apps/spark-current /usr/lib/spark-current
        echo "Start to copy conf from $masterip to local gateway(/etc/ecm)"
        sshpass -f $masterpwdfile scp -r root@$masterip:/etc/ecm/hadoop-conf  /etc/ecm/hadoop-conf
        sshpass -f $masterpwdfile scp -r root@$masterip:/etc/ecm/hive-conf /etc/ecm/hive-conf
        sshpass -f $masterpwdfile scp -r root@$masterip:/etc/ecm/spark-conf /etc/ecm/spark-conf
        echo "Start to copy environment from $masterip to local gateway(/etc/profile.d)"
        sshpass -f $masterpwdfile scp root@$masterip:/etc/profile.d/hdfs.sh /etc/profile.d/
        sshpass -f $masterpwdfile scp root@$masterip:/etc/profile.d/yarn.sh /etc/profile.d/
        sshpass -f $masterpwdfile scp root@$masterip:/etc/profile.d/hive.sh /etc/profile.d/
        sshpass -f $masterpwdfile scp root@$masterip:/etc/profile.d/spark.sh /etc/profile.d/
        if [ -L /usr/lib/jvm/java ]
        then
           unlink /usr/lib/jvm/java
        fi
        echo "" >>/etc/profile.d/hdfs.sh
        echo export JAVA_HOME=/usr/lib/jvm/jre-1.8.0 >>/etc/profile.d/hdfs.sh
        echo "Start to copy host info from $masterip to local gateway(/etc/hosts)"
        sshpass -f $masterpwdfile scp root@$masterip:/etc/hosts /etc/hosts_bak
        cat /etc/hosts_bak | grep emr | grep cluster >>/etc/hosts
        if ! id hadoop >& /dev/null
        then
           useradd hadoop
        fi
        ```

    -   E-MapReduce versions earlier than V2.7 or V3.2

        Create a script file, enter the following commands, and then run it on the Gateway node. Run the sh deploy.sh <masteri\_ip\> master\_password\_file command.

        -   deploy.sh: the script name. The commands are as follows.
        -   masteri\_ip: the IP address of the master node in the cluster. Make sure that the IP address can be accessed.
        -   master\_password\_file: the file that stores the password of the master node.
        ```
        ! /usr/bin/bash
        if [ $# ! = 2 ]
        then
           echo "Usage: $0 master_ip master_password_file"
           exit 1;
        fi
        masterip=$1
        masterpwdfile=$2
        if ! type sshpass >/dev/null 2>&1; then
           yum install -y sshpass
        fi
        if ! type java >/dev/null 2>&1; then
           yum install -y java-1.8.0-openjdk
        fi
        mkdir -p /opt/apps
        mkdir -p /etc/emr
        echo "Start to copy package from $masterip to local gateway(/opt/apps)"
        echo " -copying hadoop-2.7.2"
        sshpass -f $masterpwdfile scp -r -o 'StrictHostKeyChecking no' root@$masterip:/usr/lib/hadoop-current /opt/apps/
        echo " -copying hive-2.0.1"
        sshpass -f $masterpwdfile scp -r root@$masterip:/usr/lib/hive-current /opt/apps/
        echo " -copying spark-2.1.1"
        sshpass -f $masterpwdfile scp -r root@$masterip:/usr/lib/spark-current /opt/apps/
        echo "Start to link /usr/lib/\${app}-current to /opt/apps/\${app}"
        if [ -L /usr/lib/hadoop-current ]
        then
           Unlink/usr/lib/hadoop-Current
        fi
        ln -s /opt/apps/hadoop-current  /usr/lib/hadoop-current
        if [ -L /usr/lib/hive-current ]
        then
           unlink /usr/lib/hive-current
        fi
        ln -s /opt/apps/hive-current  /usr/lib/hive-current
        if [ -L /usr/lib/spark-current ]
        then
           unlink /usr/lib/spark-current
        fi
        Ln-S/opt/apps/spark-current/usr/lib/spark-Current
        echo "Start to copy conf from $masterip to local gateway(/etc/emr)"
        sshpass -f $masterpwdfile scp -r root@$masterip:/etc/emr/hadoop-conf  /etc/emr/hadoop-conf
        sshpass -f $masterpwdfile scp -r root@$masterip:/etc/emr/hive-conf /etc/emr/hive-conf
        sshpass -f $masterpwdfile scp -r root@$masterip:/etc/emr/spark-conf /etc/emr/spark-conf
        Echo "start to copy environment from $ masterip to local Gateway (/etc/profile. d )"
        sshpass -f $masterpwdfile scp root@$masterip:/etc/profile.d/hadoop.sh /etc/profile.d/
        if [ -L /usr/lib/jvm/java ]
        then
           unlink /usr/lib/jvm/java
        fi
        ln -s /usr/lib/jvm/java-1.8.0-openjdk-1.8.0.131-3.b12.el7_3.x86_64/jre /usr/lib/jvm/java
        echo "Start to copy host info from $masterip to local gateway(/etc/hosts)"
        sshpass -f $masterpwdfile scp root@$masterip:/etc/hosts /etc/hosts_bak
        cat /etc/hosts_bak | grep emr | grep cluster >>/etc/hosts
        if ! id hadoop >& /dev/null
        then
           useradd hadoop
        fi
        ```

-   Test
    -   Hive

        ```
        [hadoop@iZ23bc05hrvZ ~]$ hive
        hive> show databases;
        OK
        default
        Time taken: 1.124 seconds, Fetched: 1 row(s)
        hive> create database school;
        OK
        Time taken: 0.362 seconds
        hive>
        ```

    -   Run the Hadoop job

        ```
        [hadoop@iZ23bc05hrvZ ~]$ hadoop  jar /usr/lib/hadoop-current/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.6.0.jar pi 10 10
        Number of Maps  = 10
        Samples per Map = 10
        Wrote input for Map #0
        Wrote input for Map #1
        Wrote input for Map #2
        Wrote input for Map #3
        Wrote input for Map #4
        Wrote input for Map #5
        Wrote input for Map #6
        Wrote input for Map #7
        Wrote input for Map #8
        Wrote input for Map #9
          File Input Format Counters 
              Bytes Read=1180
          File Output Format Counters 
              Bytes Written=97
        Job Finished in 29.798 seconds
        Estimated value of Pi is 3.20000000000000000000
        ```


