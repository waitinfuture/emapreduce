# Edit jobs

You can create jobs to develop tasks in a project. E-MapReduce \(EMR\) supports the following types of jobs in data development: Shell, Hive, Hive SQL, Spark, Spark SQL, Spark Shell, Spark Streaming, MapReduce, Sqoop, Pig, Flink, Streaming SQL, Presto SQL, and Impala SQL.

A project is created. For more information, see [Manage projects](/intl.en-US/Data Development/Manage projects.md).

## Create a job

1.  You have logged on to the [Alibaba Cloud EMR console](https://emr.console.aliyun.com) by using your Alibaba Cloud account.

2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

3.  Click the **Data Platform** tab.

4.  In the Projects section of the page that appears, find the project that you want to edit and click **Edit Job** in the Actions column.

5.  In the Edit Job pane on the left, right-click the folder on which you want to perform operations and select **Create Job**.

6.  In the **Create Job** dialog box, specify **Name** and **Description**, and select a specific job type from the **Job Type** drop-down list.

    **Note:** The job type cannot be changed after the job is created.

7.  Click **OK**.

    **Note:** You can also right-click the folder and select **Create Subfolder**, **Rename Folder**, or **Delete Folder** to perform the corresponding operation.


## Run a job

In the Edit Job pane, click a job. In the upper-right corner of the page, click **Run**.

## View logs

After you run a job, view the operational log on the **Records** tab in the lower part of the job page.

Click **Details** in the Action column that corresponds to a job instance to go to the **Scheduling Center** tab. On this tab, you can view detailed information about the job instance.

## Available operations on jobs

In the **Edit Job** pane, you can right-click a job name and perform the following operations:

-   Clone Job: Clone the configurations of a job that already exists in the same folder.
-   Rename Job: Rename a job.
-   Delete Job: Delete a job. You can delete a job only when the job is not associated with a workflow or the associated workflow is not running or being scheduled.

## Job submission modes

The spark-submit process, which is the launcher in a data development module, is used to submit Spark jobs. This process typically occupies more than 600 MiB of memory. The Memory \(MB\) parameter in the Job Settings panel specifies the size of the memory allocated to the launcher.

EMR of the latest version supports the following job submission modes:

-   **Header/Gateway Node**: In this mode, the spark-submit process runs on the master node and is not monitored by YARN. The spark-submit process requests a large amount of memory. A large number of jobs consume many resources of the master node, which causes an unstable cluster.
-   **Worker Node**: In this mode, the spark-submit process runs on a core node, occupies a YARN container, and is monitored by YARN. This mode reduces the resource usage on the master node.

In an EMR cluster, the memory consumed by a job instance consists of the memory consumed by the launcher and the memory consumed by a specific job. For a Spark job, the memory consumed by a job is further divided into the memory consumed by the spark-submit module \(not the process\), the memory consumed by the driver, and the memory consumed by the executor. The process where the driver runs varies based on the mode in which Spark applications are launched in YARN.

-   If Spark applications are launched in yarn-client mode, the driver runs in the same process as spark-submit. If you submit a job in LOCAL mode, the process runs on the master node and is not monitored by YARN. If you submit a job in YARN mode, the process runs on a core node, occupies a YARN container, and is monitored by YARN.
-   If Spark applications are launched in yarn-cluster mode, the driver runs in a separate process and occupies a YARN container. In this case, the driver and spark-submit run in different processes.

To sum up, the job submission mode determines whether the spark-submit process runs on the master or core node, and whether the spark-submit process is monitored by YARN. Whether the driver and spark-submit run in the same process depends on the launching mode of Spark applications, which can be yarn-client or yarn-cluster.

## Add annotations

You can add annotations to job scripts to configure job parameters in data development. Add an annotation in the following format:

```
!!! @<Annotation name>: <Annotation content>
```

**Note:** Do not indent the three exclamation points \(`!!!`\) that start an annotation. Add one annotation in a line.

The following table lists all supported annotations.

|Annotation name|Description|Example|
|---------------|-----------|-------|
|rem|Adds a comment.|```
!!! @rem: This is a comment.
``` |
|env|Adds an environment variable.|```
!!! @env: ENV_1=ABC
``` |
|var|Adds a custom variable.|```
!!! @var: var1="value1 and \"one string end with 3 spaces\"   "
!!! @var: var2=${yyyy-MM-dd}
``` |
|resource|Adds a resource file.|```
!!! @resource: oss://bucket1/dir1/file.jar
``` |
|sharedlibs|Adds dependency libraries. This annotation is valid only in Streaming SQL jobs. Separate multiple dependency libraries with commas \(,\).|```
!!! @sharedlibs: sharedlibs:streamingsql:datasources-bundle:1.7.0,...
``` |
|scheduler.queue|Specifies the queue to which the job is submitted.|```
!!! @scheduler.queue: default
``` |
|scheduler.vmem|Specifies the memory required to run the job. Unit: MiB.|```
!!! @scheduler.vmem: 1024
``` |
|scheduler.vcores|Specifies the number of vCores required to run the job.|```
!!! @scheduler.vcores: 1
``` |
|scheduler.priority|Specifies the priority of the job. Valid values: 1 to 100.|```
!!! @scheduler.priority: 1
``` |
|scheduler.user|Specifies the user who submits the job.|```
!!! @scheduler.user: root
``` |

**Note:**

When you add annotations, take note of the following points:

-   Invalid annotations are automatically skipped. For example, an unknown annotation or an annotation whose content is in an invalid format will be skipped.
-   Job parameters specified in annotations take precedence over job parameters specified in the Job Settings panel. If a parameter is specified both in an annotation and in the Job Settings panel, the parameter setting specified in the annotation takes effect.

