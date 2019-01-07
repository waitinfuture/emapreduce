# RAM authentication {#concept_nsg_mb5_1fb .concept}

In addition to supporting an authentication method compatible with MIT Kerberos, the Kerberos server in the E-MapReduce cluster also supports using Alibaba Cloud Resource Access Management \(RAM\) as the identity information to perform authentication.

## RAM ID authentication {#section_hrr_mc5_1fb .section}

[RAM](https://www.alibabacloud.com/product/ram) supports creating and managing RAM user accounts, as well as using these accounts to control access to various resources on the cloud.

The administrator of the master account can create RAM users on the RAM user management page \(the user name must comply with Linux user name specifications\) and download their AccessKey for the corresponding developer. The developer can then configure the AccessKey to pass Kerberos authentication and access the cluster service.

Unlike the MIT Kerberos authentication, RAM identity authentication does not require adding principals to the Kerberos server in advance.

The following example uses a RAM user account that has already been created to access a gateway.

-   Add the RAM user to the E-MapReduce cluster.

    The E-MapReduce security cluster's YARN uses LinuxContainerExecutor. Running the YARN job on a cluster requires all cluster nodes to add the user account that is going to run the job. LinuxContainerExecutor conducts the related permission validation based on the user account during the execution process.

    The E-MapReduce cluster administrator executes the following code on the cluster's master node:

    ```
    sudo su hadoop
    sh adduser.sh test 1 2
    ```

    The adduser.sh code is attached:

    ```
    # Username
     user_name=$1
     # Master node count in the cluster. For example, the HA cluster has two master nodes.
     master_cnt=$2
     # Worker node count in the cluster
     worker_cnt=$3
     for((i=1;i<=$master_cnt;i++))
     do
       ssh -o StrictHostKeyChecking=no emr-header-$i sudo useradd $user_name
     done
     for((i=1;i<=$worker_cnt;i++))
     do
       ssh -o StrictHostKeyChecking=no emr-worker-$i sudo useradd $user_name
    done
    ```

-   The gateway administrator adds the test user account on the gateway machine.

    ```
    useradd test
    ```

-   The gateway administrator configures the basic Kerberos environment.

    ```
    sudo su root
     sh config_gateway_kerberos.sh 10.27.230.10 /pathto/emrheader1_pwd_file
     # Ensures the value of the /etc/ecm/hadoop-conf/core-site.xml file on the Gateway is true
     <property>
        <name>hadoop.security.authentication.use.has</name>
        <value>true</value>
     property
    ```

    The config\_gateway\_kerberos.sh script is attached:

    ```
    # IP address of the emr-header-1 in the EMR cluster
     masterip=$1
     # Saves the corresponding root logon password file for masterip
     masterpwdfile=$2
     if ! type sshpass >/dev/null 2>&1; then
        yum install -y sshpass
    fi
      ## Kerberos conf
     sshpass -f $masterpwdfile scp root@$masterip:/etc/krb5.conf /etc/
     mkdir /etc/has
     sshpass -f $masterpwdfile scp root@$masterip:/etc/has/has-client.conf /etc/has
     sshpass -f $masterpwdfile scp root@$masterip:/etc/has/truststore /etc/has/
     sshpass -f $masterpwdfile scp root@$masterip:/etc/has/ssl-client.conf /etc/has/
     # Modifies Kerberos client configuration, changing the default auth_type from EMR to RAM
     # This file can be manually modified
     sed -i 's/EMR/RAM/g' /etc/has/has-client.conf
    ```

-   The test user logs on to the gateway and configures the AccessKey.

    ```
    Log on the test account of Gateway
     # Run the script
     sh add_accesskey.sh test
    ```

    The add\_accesskey.sh script is attached to modify the AccessKey:

    ```
    user=$1
     if [[ `cat /home/$user/.bashrc | grep 'export AccessKey'` == "" ]];then
     echo "
     # Change to the test user's AccessKeyId/AccessKeySecret
     export AccessKeyId=YOUR_AccessKeyId
     export AccessKeySecret=YOUR_AccessKeySecret
     " >>~/.bashrc
     else
        echo $user AccessKey has been added to .bashrc
     fi
    ```

-   The test user executes the command.

    The test user is now able to execute the relevant commands to access the cluster service.

    Execute HDFS commands:

    ```
    [test@gateway ~]$ hadoop fs -ls /
      17/11/19 12:32:15 INFO client.HasClient: The plugin type is: RAM
      Found 4 items
      drwxr-x---   - has    hadoop          0 2017-11-18 21:12 /apps
      drwxrwxrwt   - hadoop hadoop          0 2017-11-19 12:32 /spark-history
      drwxrwxrwt   - hadoop hadoop          0 2017-11-18 21:16 /tmp
      drwxrwxrwt   - hadoop hadoop          0 2017-11-18 21:16 /user
    ```

    Run the Hadoop job:

    ```
    [test@gateway ~]$ hadoop jar /usr/lib/hadoop-current/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.2.jar pi 10 1
    ```

    Run the Spark job:

    ```
    [test@gateway ~]$ spark-submit --conf spark.ui.view.acls=* --class org.apache.spark.examples.SparkPi --master yarn-client --driver-memory 512m --num-executors 1 --executor-memory 1g --executor-cores 2 /usr/lib/spark-current/examples/jars/spark-examples_2.11-2.1.1.jar 10
    ```


