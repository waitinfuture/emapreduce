# Oozie {#concept_xqx_svw_y2b .concept}

The following section provides an overview of how to use Oozie in a E-MapReduce cluster.

**Note:** E-MapReduce version 2.0.0 and later support Oozie. If you need to use Oozie in a cluster, make sure that the version you are using is 2.0.0 or higher.

## Preparations {#section_wgp_pww_y2b .section}

Before you create a cluster, you must first open an SSH tunnel. For more information, see [Connect to clusters using SSH](intl.en-US/User Guide/Connect to clusters using SSH.md#).

In the following, which uses a MAC environment as an example, the IP address of the public network for the cluster's master node is assumed to be **xx.xx.xx.xx**:

1.  Log on to the master node.

    ```
    ssh root@xx.xx.xx.xx
    ```

2.  Enter your password.
3.  Check the id\_rsa.pub content of the local machine. Note that this is executed on the local machine, not the remote master node.

    ```
    cat ~/.ssh/id_rsa.pub
    ```

4.  Write the id\_rsa.pub content of the local machine in ~/.ssh/authorized\_keys on the local master node, which is executed on the remote master node.

    ```
    mkdir ~/.ssh/
    vim ~/.ssh/authorized_keys
    ```

5.  Copy and paste the content observed in [Step 2](#step2). You should now be able to log on to the master node without a password using `ssh root@xx.xx.xx.xx`.
6.  Execute the following command on the local machine to perform port forwarding:

    ```
    ssh -i ~/.ssh/id_rsa -ND 8157 root@xx.xx.xx.xx
    ```

7.  Execute the following command to enable Chrome to in the new terminal on the local machine:

    ```
    /Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --proxy-server="socks5://localhost:8157" --host-resolver-rules="MAP * 0.0.0.0 , EXCLUDE localhost" --user-data-dir=/tmp
    ```


## Access the Oozie UI interface {#section_ez4_lyw_y2b .section}

Access the following in Chrome to perform port forwarding: xx.xx.xx.xx:11000/oozie, localhost:11000/oozie, or intranet ip: 11000/oozie.

## Submit a workflow job {#section_pnw_qyw_y2b .section}

Before you run Oozie, you first have to install [Oozie's ShareLib](https://oozie.apache.org/docs/4.2.0/WorkflowFunctionalSpec.html#ShareLib).

In E-MapReduce clusters, ShareLib is installed by default for Oozie users. If you are using Oozie to submit a workflow job, you do not need to install ShareLib again.

Clusters with HA enabled use different methods to access NameNode and ResourceManager than clusters with HA disabled. Therefore, when you submit an Oozie workflow job, you need to specify a different NameNode and JobTracker \(ResourceManager\) in job.properties files. To do so, complete the following steps:

-   Non-HA clusters

    ```
    nameNode=hdfs://emr-header-1:9000
    jobTracker=emr-header-1:8032
    ```

-   HA clusters

    ```
    nameNode=hdfs://emr-cluster
    jobTracker=rm1,rm2
    ```


In the following examples, configurations are made for both non-HA and HA clusters. For operations that do not require modification, the sample code can be used directly. For the specific format of a workflow file, see the relevant documentation on the official Oozie website: [https://oozie.apache.org/docs/4.2.0/](https://oozie.apache.org/docs/4.2.0/).

-   **Submit a workflow job on a non-HA cluster**
    1.  Log on to the main master node of the cluster.

        ```
        ssh root@publicIp_of_master
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

    4.  Submit a sample Oozie workflow job.

        ```
        [oozie@emr-header-1 tmp]$ $OOZIE_HOME/bin/oozie job -config examples/apps/map-reduce/job.properties -run
        ```

        After submitting the job successfully, a jobId is returned, for example:

        ```
        job: 0000000-160627195651086-oozie-oozi-W
        ```

    5.  Go to the Oozie UI page to view the submitted Oozie workflow job.
-   **Submit a workflow job on an HA cluster**
    1.  Log on to the main master node of the HA cluster.

        ```
        ssh root@main_master_ip
        ```

        To determine the current main master node, check whether the Oozie UI can be accessed or not. By default, the Oozie server service is enabled on the main master node `xx.xx.xx.xx:11000/oozie`.

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

    4.  Submit a sample Oozie workflow job.

        ```
        [oozie@emr-header-1 tmp]$ $OOZIE_HOME/bin/oozie job -config examples/apps/map-reduce/job.properties -run
        ```

        After submitting the job successfully, a jobId is returned. This should be similar to:

        ```
        job: 0000000-160627195651086-oozie-oozi-W
        ```

    5.  Go to the Oozie UI page to view the submitted Oozie workflow job.

