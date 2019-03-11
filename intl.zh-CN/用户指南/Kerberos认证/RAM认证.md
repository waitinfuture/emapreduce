# RAM认证 {#concept_nsg_mb5_1fb .concept}

E-MapReduce（以下简称 EMR）集群中的 Kerberos 服务端除了可以支持第一种 MIT Kerberos 兼容的使用方式，也可以支持 Kerberos 客户端使用 RAM 作为身份信息进行身份认证。

## RAM身份认证 {#section_hrr_mc5_1fb .section}

[RAM](https://www.alibabacloud.com/product/ram)产品可以创建/管理子账号，通过子账号实现对云上各个资源的访问控制。

主账号的管理员可以在 RAM 的用户管理界面创建一个子账号\(子账户名称必须符合linux用户的规范\)，然后将子账号的 AccessKey 下载下来提供给该子账号对应的开发人员，后续开发人员可以通过配置AccessKey，从而通过Kerberos认证访问集群服务。

使用 RAM 身份认证不需要像第一部分MIT Kerberos使用方式一样，提前在 Kerberos 服务端添加 principle 等操作

下面以已经创建的子账号 test 在 Gateway 访问为例:

-   EMR 集群添加 test 账号

    EMR 的安全集群的 yarn 使用了 LinuxContainerExecutor，在集群上跑 yarn 作业必须要在集群所有节点上面添加跑作业的用户账号，LinuxContainerExecutor 执行程序过程中会根据用户账号进行相关的权限校验。

    EMR 集群管理员在 EMR 集群的master节点上执行:

    ```
    sudo su hadoop
    sh adduser.sh test 1 2
    ```

    附:adduser.sh 代码

    ```
    #添加的账户名称
     user_name=$1
     #集群master节点个数,如HA集群有2个master
     master_cnt=$2
     #集群worker节点个数
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

-   Gateway 管理员在 Gateway 机器上添加test用户

    ```
    useradd test
    ```

-   Gateway 管理员配置 Kerberos 基础环境

    ```
    sudo su root
     sh config_gateway_kerberos.sh 10.27.230.10 /pathto/emrheader1_pwd_file
     #确保Gateway上面/etc/ecm/hadoop-conf/core-site.xml中值为true
     <property>
        <name>hadoop.security.authentication.use.has</name>
        <value>true</value>
     </property>
    ```

    附: config\_gateway\_kerberos.sh 脚本代码

    ```
    #EMR集群的emr-header-1的ip
     masterip=$1
     #保存了masterip对应的root登录密码文件
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
     #修改Kerberos客户端配置,将默认的auth_type从EMR改为RAM
     #也可以手工修改该文件
     sed -i 's/EMR/RAM/g' /etc/has/has-client.conf
    ```

-   test 用户登录 Gateway 配置 AccessKey

    ```
    登录Gateway的test账号
     #执行脚本
     sh add_accesskey.sh test
    ```

    附: add\_accesskey.sh 脚本\(修改一下AccessKey\)

    ```
    user=$1
     if [[ `cat /home/$user/.bashrc | grep 'export AccessKey'` == "" ]];then
     echo "
     #修改为test用户的AccessKeyId/AccessKeySecret
     export AccessKeyId=YOUR_AccessKeyId
     export AccessKeySecret=YOUR_AccessKeySecret
     " >>~/.bashrc
     else
        echo $user AccessKey has been added to .bashrc
     fi
    ```

-   test 用户执行命令

    经过以上步骤，test 用户可以执行相关命令访问集群服务了。

    执行 hdfs 命令

    ```
    [test@gateway ~]$ hadoop fs -ls /
      17/11/19 12:32:15 INFO client.HasClient: The plugin type is: RAM
      Found 4 items
      drwxr-x---   - has    hadoop          0 2017-11-18 21:12 /apps
      drwxrwxrwt   - hadoop hadoop          0 2017-11-19 12:32 /spark-history
      drwxrwxrwt   - hadoop hadoop          0 2017-11-18 21:16 /tmp
      drwxrwxrwt   - hadoop hadoop          0 2017-11-18 21:16 /user
    ```

    运行 hadoop 作业

    ```
    [test@gateway ~]$ hadoop jar /usr/lib/hadoop-current/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.7.2.jar pi 10 1
    ```

    运行 spark 作业

    ```
    [test@gateway ~]$ spark-submit --conf spark.ui.view.acls=* --class org.apache.spark.examples.SparkPi --master yarn-client --driver-memory 512m --num-executors 1 --executor-memory 1g --executor-cores 2 /usr/lib/spark-current/examples/jars/spark-examples_2.11-2.1.1.jar 10
    ```


