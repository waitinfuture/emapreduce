# Submit a Flink job to process OSS data using EMR {#task_e4x_1cw_dgb .task}

This topic describes how to create a Hadoop cluster using EMR and run a Flink job to consume OSS data.

Consuming data stored in Alibaba Cloud OSS is a common scenario in the development process. This topic describes how to create a Hadoop cluster using EMR and run a Flink job to consume OSS data.

1.  Prepare the environment 

    Select Flink from the optional services when you create a Hadoop cluster.

2.  Develop a Flink job 
    1.  Run the `mvn clean package -DskipTests` command. 

        EMR provides the sample code for you to immediately use. You can download the sample code from [e-mapreduce-demo](https://github.com/aliyun/aliyun-emapreduce-demo). The path of the JAR file is target/examples-1.1.jar.

    2.  Prepare OSS test data 

        Create an OSS path and prepare test data.

    3.  Run a Flink job. 

        Upload `examples-1.1.jar` to the emr-header-1 node of the Hadoop cluster. Connect to the Hadoop cluster and submit a job. Run the following command.

        ```
        flink run -m yarn-cluster  -yjm 1024 -ytm 1024 -yn 4 -ys 4 -ynm flink-oss-sample -c com.aliyun.emr.example.flink.FlinkOSSSample examples-1.1.jar --input oss://bucket/path
        ```

        oss://bucket/path is the OSS path set in the previous step.

    4.  View the information of a Flink job 

        You can view the information of a Flink job using the YARN Web UI. You can access the YARN Web UI using the following methods: Using SSH. For more information, see [Connect to a cluster using SSH](../../../../../intl.en-US/User Guide/Connect to clusters using SSH.md#). Using Knox. For more information, see [How to use Knox](../../../../../intl.en-US/User Guide/Open source components/Knox.md#).

        -   View the information of a running job

            The following example uses SSH tunneling. The endpoint is http://emr-header-1:8088/index.html. On YARN Web UI, you can see the submitted job as the following figure.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80562/155134815034444_en-US.png)

            Click the tracking URL of the job to jump to Flink Dashboard for viewing the running job.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80562/155134815034445_en-US.png)

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80562/155134815034446_en-US.png)

        -   View history of jobs

            You can view a list of all completed jobs by accessing http://emr-header-1:8082.

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80562/155134815034447_en-US.png)


We have completed consuming OSS data by running a Flink job on an EMR cluster.

