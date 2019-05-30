# Edit a workflow {#concept_o54_wbf_z2b .concept}

An E-MapReduce workflow can be represented as a directed acyclic graph \(DAG\). You can pause, stop, and resume workflows. You can also view the running status of workflows in the web UI.

## Create a workflow {#section_sjt_1cf_z2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://partners-intl.console.aliyun.com/#/emr).
2.  Click the Data Platform tab to go to the Projects page.
3.  Click **Workflows** in the Actions column for a project. Click the **Workflows** tab and go to the **Workflows** page.
4.  Right-click the target folder and click **Create Workflow**.
5.  In the **Create Workflow** dialog box, enter a name and description for the workflow. Select a cluster from the Target Cluster drop-down list.

    You can select an existing cluster \(Subscription or Pay-As-You-Go\) that is associated with the project to run the workflow, or you can use a cluster template to create a temporary cluster for running the workflow.

6.  Click **OK**.

## Edit a workflow {#section_k4y_bcf_z2b .section}

You can drag and drop multiple types of job nodes on the Workflows canvas and connect the nodes to schedule jobs. After the jobs are scheduled, drag the **END** controller node, drop it on the canvas, and connect it to a job node to complete the workflow design.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17963/155918557210925_en-US.png)

## Configure a workflow {#section_m4y_bcf_z2b .section}

In the upper-right corner of the **Workflows** canvas, click **Configure** to schedule a workflow.

-   Target Cluster

    Select a cluster for running the jobs in the workflow. From the Target Cluster drop-down list, select Select Existing Cluster or Create Cluster from Template.

    -   Select Existing Cluster: When the workflow is executed, the jobs run on the existing cluster that you have selected.
    -   Create Cluster from Template: When the workflow is executed, the jobs run on the temporary cluster created by using the [Cluster template](reseller.en-US/Data Development/Cluster template.md#) that you have selected. The cluster is released when the workflow finishes.
-   Scheduling: When scheduling is enabled, time-based scheduling is applied by default. You can also configure rules for dependency-based scheduling.
    -   Time-based Scheduling: Sets a start time, an end time, and a cycle for scheduling the workflow. During this period, the workflow is executed according to the cycle you have set.
    -   Dependency-based Scheduling: Selects a project and selects a dependent workflow. The workflow is scheduled only when the dependent workflow finishes. You can select a maximum of one dependent workflow.
-   Configure alerts

    Alerts can be sent by SMS, email, and DingTalk group. You can configure the following alerting rules:

    -   Execution Failed: alerts that are sent when a workflow fails.
    -   Actions on Failures: alerts that are sent when job nodes of a workflow fail.
    -   Executed: notifications that are sent when a workflow succeeds.
    -   Action on Startup Timeout: When a job node fails to start 30 minutes after it is assigned to a cluster, an alert is sent and the job is canceled.

## Run a workflow {#section_p4y_bcf_z2b .section}

After the design and configuration are complete, click **Run** to run the workflow.

## View and operate a workflow instance {#section_q4y_bcf_z2b .section}

After you run the workflow, click the **Records** tab page to view the running status of the workflow instance. Click **Details** for the workflow instance to view the running status of the workflow instance. You can also pause, resume, stop, or rerun the workflow instance.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17963/155918557210926_en-US.png)

-   Pause Workflow: The job node that is running continues. The subsequent job nodes do not start. You can click **Resume Workflow** to start the subsequent job nodes again.
-   Stop Workflow: All running job nodes stop running.
-   Rerun Workflow: The workflow runs from the START node.

