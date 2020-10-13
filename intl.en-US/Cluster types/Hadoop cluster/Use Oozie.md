# Use Oozie

This topic describes how to use Oozie in EMR.

An EMR Hadoop cluster is created, and Oozie is selected from the optional services during the cluster creation. For more information, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

## Preparations

This topic uses Google Chrome to perform port forwarding in macOS.

1.  Log on to the master node. For more information, see [Log on to a cluster by using SSH](/intl.en-US/Cluster Management/Configure clusters/Log on to a cluster by using SSH.md).

    ```
    ssh root@xx.xx.xx.xx
    ```

    `xx.xx.xx.xx` indicates the public IP address of the master node.

2.  Enter your password.

3.  Check the content of the id\_rsa.pub file on your on-premises machine.

    ```
    cat ~/.ssh/id_rsa.pub
    ```

4.  Run the following commands to open the ~/.ssh/authorized\_keys file on the master node:

    ```
    mkdir ~/.ssh/
    vim ~/.ssh/authorized_keys
    ```

5.  Copy the information returned in [Step 3](#step_514_waw_28u) to the authorized\_keys file. Then, run the `ssh root@xx.xx.xx.xx` command to log on to the master node in password-free mode.

6.  Run the following command on your on-premises machine to perform port forwarding:

    ```
    ssh -i ~/.ssh/id_rsa -ND 8157 root@xx.xx.xx.xx
    ```

7.  Re-open the terminal and run the following command to start Google Chrome:

    ```
    /Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --proxy-server="socks5://localhost:8157" --host-resolver-rules="MAP * 0.0.0.0 , EXCLUDE localhost" --user-data-dir=/tmp
    ```


## Access the Oozie web UI

Enter one of the following URLs into the address bar of Google Chrome to access the Oozie web UI:

-   Public IP address:11000/oozie
-   localhost:11000/oozie
-   Internal IP address:11000/oozie

## Submit a workflow job

By default, ShareLib is installed in EMR clusters. When you submit an Oozie workflow job, you do not need to install ShareLib.

1.  In job.properties, specify NameNode and JobTracker \(ResourceManager\).

    -   Non-HA cluster

        ```
        nameNode=hdfs://emr-header-1:9000
        jobTracker=emr-header-1:8032
        ```

    -   HA cluster

        ```
        nameNode=hdfs://emr-cluster
        jobTracker=rm1,rm2
        ```

2.  Submit an Oozie workflow job.

    -   Non-HA cluster
        1.  Log on to the master node. For more information, see [Log on to a cluster by using SSH](/intl.en-US/Cluster Management/Configure clusters/Log on to a cluster by using SSH.md).

            ```
            ssh root@Public IP address of the master node
            ```

        2.  Download the sample code.

            ```
            [root@emr-header-1 ~]# su oozie
            [oozie@emr-header-1 root]$ cd /tmp
            [oozie@emr-header-1 tmp]$ wget http://emr-sample-projects.oss-cn-hangzhou.aliyuncs.com/oozie-examples/oozie-examples.zip
            [oozie@emr-header-1 tmp]$ unzip oozie-examples.zip
            ```

        3.  Synchronize the Oozie workflow code to HDFS.

            ```
            [oozie@emr-header-1 tmp]$ hadoop fs -copyFromLocal examples/ /user/oozie/examples
            ```

        4.  Submit an Oozie workflow job.

            ```
            [oozie@emr-header-1 tmp]$ $OOZIE_HOME/bin/oozie job -config examples/apps/map-reduce/job.properties -run
            ```

            If the command is executed successfully, the following information is returned:

            ```
            job: 0000000-160627195651086-oozie-oozi-W
            ```

        5.  [Access the Oozie web UI](#section_x8e_gqy_ys9).

            You can view the submitted Oozie workflow job.

    -   HA cluster
        1.  Log on to the primary master node. For more information, see [Log on to a cluster by using SSH](/intl.en-US/Cluster Management/Configure clusters/Log on to a cluster by using SSH.md).

            ```
            ssh root@Public IP address of the primary master node
            ```

        2.  Download the sample code.

            ```
            [root@emr-header-1 ~]# su oozie
            [oozie@emr-header-1 root]$ cd /tmp
            [oozie@emr-header-1 tmp]$ wget http://emr-sample-projects.oss-cn-hangzhou.aliyuncs.com/oozie-examples/oozie-examples-ha.zip
            [oozie@emr-header-1 tmp]$ unzip oozie-examples-ha.zip
            ```

        3.  Synchronize the Oozie workflow code to HDFS.

            ```
            [oozie@emr-header-1 tmp]$ hadoop fs -copyFromLocal examples/ /user/oozie/examples
            ```

        4.  Submit an Oozie workflow job.

            ```
            [oozie@emr-header-1 tmp]$ $OOZIE_HOME/bin/oozie job -config examples/apps/map-reduce/job.properties -run
            ```

            If the command is executed successfully, the following information is returned:

            ```
            job: 0000000-160627195651086-oozie-oozi-W
            ```

        5.  [Access the Oozie web UI](#section_x8e_gqy_ys9).

            You can view the submitted Oozie workflow job.


