# Workflow management {#concept_o54_wbf_z2b .concept}

You can run big data jobs parallelly in a DAG manner by E-MapReduce workflow. In addition, you can suspend, stop, rerun workflow, and view the running status of workflow in WebUI.

## Creating a workflow {#section_sjt_1cf_z2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce Console](https://emr.console.aliyun.com/?spm=5176.8250060.103.1.417717dcWrUkDq#/cn-hangzhou) to enter the Cluster List page with primary account.
2.  Click the **Data Platform** tab on the top to enter the **Project List** page.
3.  Click **Design Workflow** of the specified project in the **Operation** column, and then select **Workflow Design** tab.
4.  On the left side, right-click on the folder you want to operate and select **New Workflow**.
5.  In the **Create Workflow** dialog box, enter the workflow name, workflow description, and select the E-MapReduce cluster where the workflow is to run.

    You can select a Subscription and Pay-As-You-Go E-MapReduce cluster that has been created and is associated with the project for the workflow running, or a new temporary cluster can be created through the cluster template to run the workflow.

6.  Click **Ok**.

## Editing workflow {#section_k4y_bcf_z2b .section}

You can drag different types of jobs to the workflow editing canvas, and specify the order of job instances by curve. After the jobs needed to run has been dragged, drag the **END** component from the control node area to the canvas, which indicates that the entire workflow is complete.

![](images/10925_en-US.png)

## Configure workflow {#section_m4y_bcf_z2b .section}

On the right side of the **Workflow Design** page, click **Configure** button to configure the workflow scheduling.

-   Execution Cluster: The E-MapReduce cluster where the workflow is to run can be modified.
-   Scheduling Policy: After workflow scheduling is enabled, you can choose a scheduling policy, including time scheduling and dependency scheduling.
    -   Time Scheduling: Sets the start time and end time of the workflow scheduling, the system will run the workflow according to the schedule you set during the time scope.
    -   Dependency Scheduling: From the selected project, select the dependency workflow of the current workflow. After the dependency workflow is completed, the current workflow will be scheduled to run. Currently, only one workflow can be selected.

## Run a workflow {#section_p4y_bcf_z2b .section}

Once the workflow is designed and configured, you can run the workflow by clicking the **Run** button in the top right corner.

## View and operate workflow instances {#section_q4y_bcf_z2b .section}

After the workflow is running, click the **Running Records** tab on the left to view the running status of the workflow instance. Click the **Details** of the workflow instance to view the running status of the job instance. You can also suspend, resume, stop, and rerun the workflow instance.

![](images/10926_en-US.png)

-   Suspend Workflow: The running job instance will continue to run, but the subsequent job instances will not. You can click **Resume Workflow** and the system will continue to run the subsequent jobs after the job instance is suspended.
-   Stop Workflow: All running job instances stop immediately.
-   Rerun Workflow Instance: The system will run the workflow from the start component.

