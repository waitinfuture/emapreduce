# Status list {#concept_zkb_3cc_pfb .concept}

## Cluster status list {#section_cyh_jcc_pfb .section}

**Note:** You can view the cluster status in the cluster list or on the cluster details page.

|Status|Status code|Description|
|------|-----------|-----------|
|Creating|CREATING|The cluster is being created. The creation task includes two stages: creating physical ECS machines and activating Spark clusters. It takes a moment for the clusters to start running.|
|Failed|CREATE\_FAILED|An exception occurred during creation. The ECS instance that you have created automatically rolls back. You can click the question mark \(?\) to the right of the status on the cluster list page to view exception details.|
|Running|RUNNING|The computing cluster is running.|
|Idle|IDLE|The cluster is not running any execution plan.|
|Releasing|RELEASING|Click Release in the status list to set the cluster to this status. This status indicates that the cluster is in the releasing process. It may take a moment to complete this process.|
|Release Failed|RELEASE\_FAILED|An exception occurred when releasing the cluster. You can click the question mark \(?\) to the right of the status on the cluster list page to view the exception details. When the cluster is in this status, click Release to release the cluster again.|
|Released|RELEASED|The computing cluster and the ECS instance that hosts the computing cluster have been released.|
|Abnormal|ABNORMAL|Unrecoverable errors occurred on one or more nodes in the computing cluster. Click Release to release the cluster.|

## Job status list {#section_oh2_1dc_pfb .section}

**Note:** View job status in the job status list

|Status|Description|
|------|-----------|
|Ready|The creation information is complete, correct, and successfully saved. The job is ready to be added to the submission queue. It may take a moment for the job to change its status to Submitting.|
|Submitting|The job is in the submission queue of the computing cluster. It has not been submitted to the computing cluster.|
|Failed|An exception occurred when submitting the job to the computing cluster. You need to clone and submit the job if you want to submit this job again.|
|Running|The job is running in the cluster. Wait a moment and click the corresponding log button in the job list to view output log entries in real time.|
|Succeeded|The job has been successfully executed in the cluster. Click the corresponding log button to view the log entry.|
|Failed|An exception occurred when executing the job. Click the corresponding log button to view the log entry.|

