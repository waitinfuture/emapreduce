# Ram Certification {#concept_nsg_mb5_1fb .concept}

The Kerberos server in the EMR cluster supports not only the authentication method compatible with MIT Kerberos, but also the identity authentication by using RAM as the identity information.

## RAM ID authentication {#section_hrr_mc5_1fb .section}

[RAM](https://www.aliyun.com/product/ram) product supports creating/managing subaccounts and using subaccounts to implement access control for various resources on the cloud.

Administrator of the master account may create a subaccount on the RAM user management page \(subaccount name must comply with Linux username specifications\) and download the subaccount AccessKey for the corresponding developer. The developer can then configure the AccessKey to pass Kerberos authentication and access the cluster service.

Unlike using the first type MIT Kerberos authentication, RAM identity authentication does not require adding principle to the Kerberos server in advance.

The following example uses subaccount test that has already been created to access the Gateway:

-   Add the test subaccount into the EMR cluster

    The EMR security cluster’s yarn uses LinuxContainerExecutor. Running the yarn job on a cluster requires all cluster nodes to add the user account that is going to run the job. LinuxContainerExecutor conducts the related permission validation based on the user account during the execution process.

    The EMR cluster administrator executes the following code on the EMR cluster's master node:

    ```
    sudo su hadoop
    sh adduser.sh test 1 2
    ```

    Attachment: adduser.sh code

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

-   The Gateway administrator adds the test user account on the Gateway machine

    ```
    useradd test
    ```

-   The Gateway administrator configures the basic Kerberos environment

    ```
    sudo su root
     sh config_gateway_kerberos.sh 10.27.230.10 /pathto/emrheader1_pwd_file
     # Ensures the value of the /etc/ecm/hadoop-conf/core-site.xml file on the Gateway is true
     <property>
        <name>hadoop.security.authentication.use.has</name>
        <value>true</value>
     property
    ```

    Attachment: config\_gateway\_kerberos.sh script

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

-   test user logs on to Gateway and configures AccessKey

    ```
    Log on the test account of Gateway
     # Run the script
     sh add_accesskey.sh test
    ```

    Attachment: add\_accesskey.sh script \(modify the AccessKey\)

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

-   Test user executes the command

    After taking the preceding steps, the test user is now able to execute the relevant commands to access the cluster service.

    Execute HDFS commands

    ```
    [test@gateway ~]$ hadoop fs -ls /
      17/11/19 12:32:15 INFO client.HasClient: The plugin type is: RAM
      Found 4 items
      drwxr-x---   - has    hadoop          0 2017-11-18 21:12 /apps
      drwxrwxrwt   - hadoop hadoop          0 2017-11-19 12:32 /spark-history
      drwxrwxrwt   - hadoop hadoop          0 2017-11-18 21:16 /tmp
      drwxrwxrwt   - hadoop hadoop          0 2017-11-18 21:16 /user
    ```

    Run the hadoop job

    ```
    [test@gateway ~]$ hadoop jar /usr/lib/hadoop-current/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.2.jar pi 10 1
    ```

    Run the spark job

    ```
    [test@gateway ~]$ spark-submit --conf spark.ui.view.acls=* --class org.apache.spark.examples.SparkPi --master yarn-client --driver-memory 512m --num-executors 1 --executor-memory 1g --executor-cores 2 /usr/lib/spark-current/examples/jars/spark-examples_2.11-2.1.1.jar 10
    ```


