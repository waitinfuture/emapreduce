# Create your gateway {#concept_b4j_pvn_1fb .concept}

Gateway is an ECS server in the same intranet as the E-MapReduce cluster. You can use Gateway to achieve load balancing and security isolation, or submit jobs to the E-MapReduce cluster.

You can create Gateway in the following two ways:

-   \(Recommended\) Create in [E-MapReduce](https://emr.console.aliyun.com/console#/cn-hangzhou/cluster/gateway/create).
-   Set up a Gateway manually.

## Create a gateway on the E-MapReduce Console {#section_jlz_nxn_1fb .section}

E-MapReduce gateways only support Hadoop clusters. You must create a Hadoop cluster before creating an E-MapReduce gateway. To create an E-MapReduce gateway, follow these steps.

1.  Log on to the [E-MapReduce console](https://emr.console.aliyun.com/#/cluster/region/cn-hangzhou/).
2.  Click **Create Gateway**.
3.  Configure in the **Create Gateway** page.
    -   **Billing Method**:
        -   **Subscription**: Subscription billing method pays for a period one time, the price is relatively cheap compared to Pay-As-You-Go method, especially when you pay for three years one time, the discount is larger.
        -    **Pay-As-You-Go**: Pay-As-You-Go method is based on the actual number of hours that you used the product to calculate the order, and it charges by hour.
    -   **Cluster**: Create a gateway for the cluster, that is, the created gateway can submit jobs to which cluster. Gateway will automatically configure the Hadoop environment that is consistent with the cluster.
    -   **Configuration**: The available ECS instance specifications in the zone.
    -   **System Disk Type**: The system disk type of the gateway node. There are two types of system disk: SSD cloud disk and efficient cloud disk. The displayed type of system disk varies according to different server models and different regions. The system disk is released with the release of the cluster by default.
    -   **System Disk Size**: The minimum is 40GB and the maximum is 500GB. The default is 300 GB.
    -   **Data Disk Type**: The data disk type of the gateway node. There are two types of data disk: SSD Disk and efficient cloud disk. The displayed type of data disk varies according to different server models and different regions. The data disk is released with the release of the cluster by default.
    -   **Data Disk Size**: The minimum is 200GB and the maximum is 4000GB. The default value is 300GB.
    -   **Quantity**: The number of data disks. The minimum is 1 and the maximum is 10.
    -   **Cluster Name**: The name of a gateway. The length can be 1~64 characters. It only allows Chinese character, letter, number, hyphen \(-\), and underscore \(\_\).
    -   **Password/Key Pair**:
        -   **Password Mode**: Enter the password for gateway login in the text box.
        -   **Key Pair Mode**: Select the key pair name for Gateway login in the drop-down menu. If no key pair has been created yet, click **Create Key Pair** on the right to go to the ECS console to create. Do not disclose the private key file with .pem format corresponding to the key pair. After Gateway is created successfully, the public key of the key pair will be bound to the ECS in which Gateway is located automatically. When you log on to Gateway via SSH, enter the private key in the private key file.
4.  Click **Create** to save the configurations.

    The newly created gateway will be displayed in the cluster list, and the status in the **Status** column becomes **Idle** when the creation is successful.


## Set up a gateway manually { .section}

-   **Network environment**

    Make sure that the Gateway machine is in the security group of the corresponding EMR cluster, so that the Gateway nodes can smoothly access the EMR cluster. For setting the security group of the machine, see the [Create a security](../../../../intl.en-US/User Guide/Security groups/Create a security group.md#).

-   **Software environment**
    -   System environment: CentOS 7.2+ is recommended.
    -   Java environment: JDK 1.7 or later version must be installed and OpenJDK 1.8.0 is recommended.
-   **Procedure**
    -   **EMR 2.7 or later version, 3.2 or later version**

        To create Gateway in these versions, we recommend that you use the EMR console.

        If you want to set up a Gateway manually, copy the following script to the gateway host and run it. The command is sh deploy.sh <masteri\_ip\> master\_password\_file.

        -   deploy.sh is the script name, and the content is as follows.
        -   masteri\_ip is the IP address of the master node in the cluster, which needs to be accessible.
        -   master\_password\_file is the file for storing password of the master node, which is directly written in the file.
        ```
        #! /usr/bin/bash
        If [$ #! = 2]
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
           unlink /usr/lib/hadoop-current
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

    -   **EMR 2.7 earlier version, 3.2 earlier version**

        Copy the following script to the Gateway host and run it. The command is sh deploy.sh <masteri\_ip\> master\_password\_file.

        -   deploy.sh is the script name, and the content is as follows.
        -   masteri\_ip is the IP address of the master node in the cluster, which needs to be accessible.
        -   master\_password\_file is the file for storing password of the master node, which is directly written in the file.
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

-   **Test**
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


