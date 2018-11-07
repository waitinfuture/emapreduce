# Oozie {#concept_xqx_svw_y2b .concept}

Oozie Instructions

**Note:** Alibaba Cloud E-MapReduce version 2.0.0 and the later versions support Oozie. If it is necessary to use Oozie in a cluster, make sure that the cluster version is higher than 2.0.0.

## Preparations {#section_wgp_pww_y2b .section}

Before you create a cluster, it is required to open an SSH tunnel. For detailed steps, see [Connect to the cluster using SSH](intl.en-US/User Guide/Connect to the cluster using SSH.md#).

Take the MAC environment as an example. Assuming the IP address of the public network for the master node of the cluster is **xx.xx.xx.xx**\):

1.  Log on to the master node.

    ```
    ssh root@xx.xx.xx.xx
    ```

2.  Enter your password.
3.  Check id\_rsa.pub content of the local machine \(note that this will be executed on the local machine rather than the remote master node\).

    ```
    cat ~/.ssh/id_rsa.pub
    ```

4.  Write id\_rsa.pub content of the local machine in ~/.ssh/authorized\_keys on the local master node to execute on the far-end master node.

    ```
    mkdir ~/.ssh/
    vim ~/.ssh/authorized_keys
    ```

5.  Paste the content observed in [Step 2](#step2). Now, `ssh root@xx.xx.xx.xx` can be used directly to log on to the master node without a password.
6.  Execute the below commands on the local machine for port forwarding.

    ```
    ssh -i ~/.ssh/id_rsa -ND 8157 root@xx.xx.xx.xx
    ```

7.  Enable Chrome to execute in the new terminal on the local machine.

    ```
    /Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --proxy-server="socks5://localhost:8157" --host-resolver-rules="MAP * 0.0.0.0 , EXCLUDE localhost" --user-data-dir=/tmp
    ```


## Access Oozie UI interface {#section_ez4_lyw_y2b .section}

Access in Chrome browser for port forwarding: “xx.xx.xx.xx:11000/oozie”, “localhost:11000/oozie”, or intranet “ip: 11000/oozie”.

## Submit workflow job {#section_pnw_qyw_y2b .section}

Before running Oozie, it is required to install the sharelib: [https://oozie.apache.org/docs/4.2.0/WorkflowFunctionalSpec.html\#ShareLib](https://oozie.apache.org/docs/4.2.0/WorkflowFunctionalSpec.html#ShareLib).

In E-MapReduce clusters, Oozie users are installed with sharelib by default, and there is no need to install sharelib again even though users using Oozie are to submit workflow job.

Since clusters with and without enabled HA have different modes to access NameNode and ResourceManager, when submitting an oozie workflow job, it is required to specify different NameNode and JobTracker \(ResourceManager\) in job.properties files. Specific steps are as follows:

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


For the following operation examples, configurations are made for both non-HA and HA clusters, and the sample code can be used directly for operations without any modification. For the specific format of a workflow file, see the official documents for Oozie at [https://oozie.apache.org/docs/4.2.0/](https://oozie.apache.org/docs/4.2.0/).

-   **Submit a workflow job on a non-HA cluster**
    1.  Log on to the main master node of the cluster.

        ```
        ssh root@publicIp_of_master
        ```

    2.  Download sample code.

        ```
        [root@emr-header-1 ~]# su oozie
        [oozie@emr-header-1 root]$ cd /tmp
        [oozie@emr-header-1 tmp]$ wget http://emr-sample-projects.oss-cn-hangzhou.aliyuncs.com/oozie-examples/oozie-examples.zip
        [oozie@emr-header-1 tmp]$ unzip oozie-examples.zip
        ```

    3.  Synchronize Oozie workflow code to hdfs.

        ```
        [oozie@emr-header-1 tmp]$ hadoop fs -copyFromLocal examples/ /user/oozie/examples
        ```

    4.  Submit an Oozie workflow sample job.

        ```
        [oozie@emr-header-1 tmp]$ $OOZIE_HOME/bin/oozie job -config examples/apps/map-reduce/job.properties -run
        ```

        After a successful execution, a jobId will be returned and is similar to:

        ```
        job: 0000000-160627195651086-oozie-oozi-W
        ```

    5.  Visit the Oozie UI page to see the submitted Oozie workflow job.
-   **Submit a workflow job on an HA cluster**
    1.  Log on to the main master node of the HA cluster.

        ```
        ssh root@main_master_ip
        ```

        The current main master node can be determined by checking whether the Oozie UI can be accessed or not. By default, the Oozie server service is enabled on the main master node `xx.xx.xx.xx:11000/oozie`.

    2.  Download sample codes of HA cluster.

        ```
        [root@emr-header-1 ~]# su oozie
        [oozie@emr-header-1 root]$ cd /tmp
        [oozie@emr-header-1 tmp]$ wget http://emr-sample-projects.oss-cn-hangzhou.aliyuncs.com/oozie-examples/oozie-examples-ha.zip
        [oozie@emr-header-1 tmp]$ unzip oozie-examples-ha.zip
        ```

    3.  Synchronize Oozie workflow code to hdfs.

        ```
        [oozie@emr-header-1 tmp]$ hadoop fs -copyFromLocal examples/ /user/oozie/examples
        ```

    4.  Submit Oozie workflow sample job.

        ```
        [oozie@emr-header-1 tmp]$ $OOZIE_HOME/bin/oozie job -config examples/apps/map-reduce/job.properties -run
        ```

        After a successful execution, a jobId will be returned and is similar to:

        ```
        job: 0000000-160627195651086-oozie-oozi-W
        ```

    5.  Visit the Oozie UI page to see the submitted Oozie workflow job.

