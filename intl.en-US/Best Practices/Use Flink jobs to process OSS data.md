# Use Flink jobs to process OSS data {#task_1358170 .task}

This topic describes how to create a Hadoop cluster by using E-MapReduce \(EMR\) and use Flink jobs to process Object Storage Service \(OSS\) data in the cluster.

-   You have registered an Alibaba Cloud account. For more information, see [Create an Alibaba Cloud account](https://www.alibabacloud.com/help/doc-detail/50482.htm).
-   You have activated EMR and OSS.
-   You have authorized the Alibaba Cloud account. For more information, see [Role authorization](../../../../intl.en-US/Cluster Planning and Configurations/Cluster planning/Role authorization.md#).

During practical applications, you always need to consume data stored in OSS. In EMR, you can run a Flink job to consume data stored in OSS buckets. You can perform the following steps to create a Flink job in EMR and run the Flink job on a Hadoop cluster to obtain and output the specified content of a file stored in OSS.

## Step 1: Prepare the environment {#section_sc4_0m8_rk0 .section}

Before creating a Flink job, you must prepare the Maven and Java environment on your local host and create a Hadoop cluster in EMR. If you are using Maven 3.0 or later, we recommend that you use Java 2.0 or earlier to ensure compatibility.

1.  Install Maven and Java on your local host.
2.  Log on to the [EMR console](https://emr.console.aliyun.com) and create a Hadoop cluster. You must select Flink in the **Optional Services** field. For more information, see [Step 3Create a cluster](../../../../intl.en-US/Quick Start/Step 3: Create a cluster.md#).

## Step 2: Prepare testing data {#section_54u_7zi_0rt .section}

Before creating a Flink job, you must upload testing data to OSS. The following is an example of uploading a file named test.txt. The file content is: Nothing is impossible for a willing heart. While there is a life, there is a hope.

1.  Log on to the [OSS console](https://oss.console.aliyun.com/).
2.  Create a bucket and upload the file to the bucket. For more information, see [Create a bucket](../../../../intl.en-US/Quick Start/Create a bucket.md#) and [Upload an object](../../../../intl.en-US/Quick Start/Upload an object.md#). 

    The sample path of the uploaded file is oss://emr-logs2/hengwu/test.txt. Keep the path for later use.

    **Note:** After uploading the file, keep the OSS logon window open for later use.


## Step 3: Build a JAR file and upload it to OSS or the Hadoop cluster {#section_lkr_3dw_7yu .section}

Download EMR sample code [aliyun-emapreduce-demo](https://github.com/aliyun/aliyun-emapreduce-demo) and compile the code to create a JAR file. You can upload the JAR file to the header node of the Hadoop cluster or an OSS bucket. The following is an example of uploading the JAR file to an OSS bucket.

1.  Download EMR sample code [aliyun-emapreduce-demo](https://github.com/aliyun/aliyun-emapreduce-demo) to your local disk.
2.  Run the mvn clean package -DskipTests command to create the JAR file. 

    The new JAR file is installed in the ../target/ directory, for example, target/examples-1.2.0.jar.

3.  Go to the [OSS console](https://oss.console.aliyun.com/) and upload the JAR file to an OSS directory. 

    The sample path of the JAR file is oss://emr-logs2/hengwu/examples-1.2.0.jar. Keep the path for later use.


## Step 4: Create and run a Flink job {#section_qzw_s78_er5 .section}

1.  Log on to the [EMR console](https://emr.console.aliyun.com).
2.  On the **Data Platform** tab, create a project. For more information, see [Manage a workflow project](../../../../intl.en-US/Data Development/Manage a workflow project.md#).
3.  Open the new project, select the Edit Job tab, and create a job with the type of **Flink**.
4.  After the new Flink job is created, specify the **content** of the job. 

    ![Content of the Flink job](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1082642/156514230253103_en-US.png)

    The **job content** is a snippet of code. An example is shown as follows.

    ``` {#codeblock_y24_sjw_1gy}
    run -m yarn-cluster  -yjm 1024 -ytm 1024 -yn 4 -ys 4 -ynm flink-oss-sample -c com.aliyun.emr.example.flink.FlinkOSSSample  ossref://emr-logs2/hengwu/examples-1.2.0.jar --input oss://emr-logs2/hengwu/test.txt
    ```

    Key parameters in the preceding code are described as follows:

    -   ossref://emr-logs2/hengwu/examples-1.2.0.jar: indicates the path of the uploaded JAR file.
    -   oss://emr-logs2/hengwu/test.txt: indicates the path of the testing data.
    **Note:** Replace the value of each parameter with values based on the configurations in the [Step 1: Prepare the environment](#section_sc4_0m8_rk0) and [Step 3: Build a JAR file and upload it to OSS or the Hadoop cluster](#section_lkr_3dw_7yu) topics.

5.  After the job configuration is complete, click **Run** in the upper-right corner, and select the name of the new Hadoop cluster in the **Target Cluster** field.
6.  Click **OK** to run the Flink job. 

    When the job is running, the Log window appears. After the job is complete, the file content is obtained from an OSS bucket and output to logs. At this point, the Flink job that runs on an EMR cluster to consume OSS data is complete.

    ![View the results of the Flink job](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1082642/156514230253150_en-US.png)


## Step 6: View the logs and details of the job {#section_xcd_kdu_25t .section}

You can view the logs and details of the job to identify the cause of a job failure and details of the job.

1.  View the logs of the job. You can view logs in the EMR console or on an SSH client.
    -   Log on to the [EMR console](https://emr.console.aliyun.com) to view logs.

        After submitting a job in the console, you can open the Details page of a job listed on the Records tab. On the Details page, you can view the results of the job.

        ![View the logs of the Flink job](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1082642/156514230353291_en-US.png)

    -   You can log on to the header node of a Hadoop cluster by using SSH to view logs.

        By default, the logs of a Flink job are stored in the /mnt/disk1/log/flink/flink-<user\>-client-<hostname\>.log file based on the configurations in the **log4j** file. For more information about detailed configurations, see the /etc/ecm/flink-conf/log4j-yarn-session.properties file.

        The user field indicates the account with which you submit the Flink job. The hostname field indicates the name of the instance to which you submit the job. Assume that you log on as a root user and submit a Flink job on the **emr-header-1** instance. In this case, the log path is /mnt/disk1/log/flink/flink-flink-historyserver-0-emr-header-1.cluster-126601.log.

2.  View the details of the job. 

    You can use **Yarn UI**. You can access **Yarn UI** by using SSH and Knox. For more information about SSH, see [Connect to a cluster using SSH](../../../../intl.en-US/Cluster Planning and Configurations/Configure clusters/Connect to a cluster using SSH.md#). For more information about Knox, see [../../../../dita-oss-bucket/SP\_159/DNEMAPREDUCE19100417/EN-US\_TP\_17921.md\#](../../../../intl.en-US/Open Source Components /Knox.md#) and [Access links and ports](https://www.alibabacloud.com/help/doc-detail/89065.htm). The following takes Knox as an example to describe how to view the details of a job.

    1.  On the **Connect Strings** tab of the Hadoop cluster, click the link next to **Yarn UI** to open the Hadoop console. 

        ![Connect string of YARN UI](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1082642/156514230353382_en-US.png)

    2.  In the Hadoop console, click the **ID** of the job to view the details of the job. 

        ![Flink job list in the Hadoop console](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1082642/156514230353394_en-US.png)

        ![Flink job details in the Hadoop console](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1082642/156514230453402_en-US.png)

    3.  If you need to view a list of running Flink jobs, you can click the link next to **Tracking URL** on the Details page. The Flink Dashboard page that appears displays the list of running Flink jobs. 

        You can also access http://emr-header-1:8082 to view a list of completed jobs.


