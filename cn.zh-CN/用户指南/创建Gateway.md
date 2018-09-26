# 创建Gateway {#concept_b4j_pvn_1fb .concept}

Gateway是与EMR集群处于同一个内网中的ECS服务器，用户可以使用Gateway实现负载均衡和安全隔离，也可以通过Gateway向E-MapReduce集群提交作业。

您可以通过以下两种方式创建Gateway：

-   （推荐）通过[EMR控制台](https://emr.console.aliyun.com/console#/cn-hangzhou/cluster/gateway/create)创建。
-   手动搭建。

## 通过E-MapReduce控制台创建Gateway {#section_jlz_nxn_1fb .section}

在创建Gateway前，请确保您已经创建了E-MapReduce集群。创建Gateway，请按照如下步骤进行操作：

1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/#/cluster/region/cn-hangzhou/)，进入**集群列表**页面。
2.  单击页面右上角的**创建Gateway**按钮。
3.  在**创建Gateway**页面中进行配置。
    -   **付费类型**：包年包月是一次性支付一段时间的费用，价格相对来说会比较便宜，特别是包三年的时候折扣会比较大。按量付费是根据实际使用的小时数来支付费用，每个小时计一次费用。
    -   **我的集群**：为该集群创建Gateway，即创建的Gateway可以向哪个集群提交作业。Gateway将会自动配置与该集群一致的Hadoop环境。
    -   **配置**：该区域内可选择的ECS实例规格。
    -   **系统盘配置**：Gateway节点使用的系统盘类型。系统盘有2种类型：SSD云盘和高效云盘，根据不同机型和不同的Region，系统盘显示类型会有不同。系统盘默认随着集群的释放而释放。
    -   **系统盘大小**：最小为40GB，最大为500GB。默认值为300GB。
    -   **数据盘配置**：Gateway节点使用的数据盘类型。数据盘有2种类型：SSD云盘和高效云盘，根据不同机型和不同的Region，数据盘显示类型会有不同。数据盘默认随着集群的释放而释放。
    -   **数据盘大小**：最小为200GB，最大为4000GB。默认值为300GB。
    -   **数量**：数据盘的数量，最小设置为1台，最大设置为10台。
    -   **集群名称**：创建的Gateway的名称，长度限制为1~64个字符，只允许包含中文、字母、数字、连接号（-）、下划线（\_）。
    -   **密码/密钥对**：
        -   **密码模式**：在文本框中输入登录Gateway的密码。
        -   **密钥对模式**：在下拉菜单中选择登录Gateway的密钥对名称。如果还未创建过密钥对，单击右侧的**创建密钥对**链接，进入ECS控制台创建。请妥善保管好密钥对所对应的私钥文件（.pem文件）。在Gateway创建成功后，该密钥对的公钥部分会自动绑定到Gateway所在的云服务器ECS上。在您通过SSH登录Gateway时，请输入该私钥文件中的私钥。
4.  单击**创建**完成Gateway的创建。

    新创建的Gateway会显示在集群列表中，创建完成后，**状态**列会变为**空闲**状态。


## 手动搭建Gateway { .section}

-   网络环境

    首先要保证Gateway节点在EMR对应集群的安全组中，Gateway节点可以顺利的访问EMR集群。设置节点的安全组请参考[创建安全组](https://help.aliyun.com/document_detail/25468.html)。

-   软件环境
    -   系统环境：推荐使用CentOS 7.2及以上版本。
    -   Java环境：安装JDK 1.7及以上版本，推荐使用OpenJDK version 1.8.0版本。
-   搭建步骤
    -   EMR 2.7及以上版本，3.2及以上版本

        这些版本推荐直接使用EMR控制台来创建Gateway。

        如果您选择手动搭建，请先创建一个脚本，脚本内容如下所示，然后在Gataway节点上执行。执行命令为：sh deploy.sh <masteri\_ip\> master\_password\_file。

        -   deploy.sh：脚本名称，内容见下面代码。
        -   masteri\_ip：集群的master节点的IP，请确保可以访问。
        -   master\_password\_file：保存master节点的密码文件，将master节点的密码直接写在文件内即可。
        ```
        #!/usr/bin/bash
        if [ $# != 2 ]
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

    -   EMR 2.7以下版本，3.2以下版本

        创建一个脚本，脚本内容如下所示，然后在Gataway节点上执行。执行命令为：sh deploy.sh <masteri\_ip\> master\_password\_file。

        -   deploy.sh：脚本名称，内容见下面代码。
        -   masteri\_ip：集群的master节点的IP，请确保可以访问。
        -   master\_password\_file：保存master节点的密码文件，将master节点的密码直接写在文件内即可。
        ```
        !/usr/bin/bash
        if [ $# != 2 ]
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
        echo "Start to copy conf from $masterip to local gateway(/etc/emr)"
        sshpass -f $masterpwdfile scp -r root@$masterip:/etc/emr/hadoop-conf  /etc/emr/hadoop-conf
        sshpass -f $masterpwdfile scp -r root@$masterip:/etc/emr/hive-conf /etc/emr/hive-conf
        sshpass -f $masterpwdfile scp -r root@$masterip:/etc/emr/spark-conf /etc/emr/spark-conf
        echo "Start to copy environment from $masterip to local gateway(/etc/profile.d)"
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

-   测试
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

    -   运行Hadoop作业

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


