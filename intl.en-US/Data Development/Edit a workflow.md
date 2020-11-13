# Edit a workflow

In a data development project of E-MapReduce \(EMR\), you can define a group of dependent jobs, and then create a workflow to allow the jobs to run in sequence based on their dependencies. An EMR workflow can be represented as a directed acyclic graph \(DAG\) that allows big data jobs to run in parallel. You can schedule workflows or view the running status of workflows in the EMR console.

-   You have logged on to the [Alibaba Cloud EMR console](https://emr.console.aliyun.com) by using your Alibaba Cloud account.

    **Note:** You can use only your Alibaba Cloud account to create projects, add project members, and associate clusters with projects. That is, the **Create Project** button and the **Users** and **Cluster Settings** pages are visible only when you log on to the EMR console by using your Alibaba Cloud account.

-   A project is created. For more information, see [Manage projects](/intl.en-US/Data Development/Manage projects.md).
-   Jobs are edited. For more information, see [Edit jobs](/intl.en-US/Data Development/Edit jobs.md).

## Create a workflow

Perform the following steps to create a workflow:

1.  Click the **Data Platform** tab.

2.  In the **Projects** section of the page that appears, find your project and click **Workflows** in the Actions column.

3.  In the **Workflows** pane on the left of the page that appears, right-click the folder on which you want to perform operations and select **Create Workflow**.

4.  In the **Create Workflow** dialog box, specify Workflow Name, Description, Select Resource Group, and Target Cluster.

    -   **Select Existing Cluster**: When the workflow is executed, the jobs run on the cluster that you selected.
    -   **Create Cluster from Template**: When the workflow is executed, the jobs run on a temporary cluster created by using the cluster template that you selected. The cluster is automatically released when the workflow ends. For more information, see [Manage cluster templates](/intl.en-US/Data Development/Manage cluster templates.md).

        **Note:** Only the clusters that are associated with the project are displayed in the **Select Existing Cluster** drop-down list. To select a different cluster, disassociate the existing clusters from the project first. For more information, see [Manage projects](/intl.en-US/Data Development/Manage projects.md).

5.  Click **OK**.


## Edit a workflow

Perform the following steps to edit a workflow:

1.  Drag and drop different types of job nodes to the canvas for editing a workflow.

    You need to associate each job node with jobs of the same type.

2.  Drag a line from the center in the lower part of each job node on the canvas to associate this job node with other job nodes based on the dependencies between the jobs. Arrows indicate the running direction of the workflow.

3.  After the job nodes are associated, drag the END widget from the Controller Node section to the canvas and associate the END widget with the job nodes at the end of the workflow to complete the design of the workflow.

    ![Edit a workflow](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1734204061/p10925.png)

    When you edit a workflow, you can click **Lock** in the upper-right corner to lock the workflow. This way, only you can edit or run the workflow. Other members in the project can edit this workflow only after it is unlocked.

    **Note:** Only the RAM user who performs the lock operation and the Alibaba Cloud account can unlock the workflow.


## Configure workflow scheduling

You can enable the workflow scheduling feature and configure the parameters related to scheduling. Then, relevant workflows run periodically based on the parameter settings, and jobs are delivered to a specified cluster for running. Perform the following steps to configure the parameters on the Basic Attributes, Scheduling Settings, and Alert Settings tabs in the Workflow Scheduling pane:

1.  Click the **Data Platform** tab.

2.  In the **Projects** section of the page that appears, find your project and click **Workflows** in the Actions column.

3.  On the workflow design page, click **Configure**.

4.  On the **Basic Attributes** tab of the Workflow Scheduling pane, modify the workflow description, resource group, and the cluster used to run the jobs in the workflow as needed.

5.  After the basic attributes are modified, click the **Scheduling Settings** tab to configure the parameters related to workflow scheduling.

    |Parameter|Description|
    |---------|-----------|
    |**Scheduling Status**|Start or stop workflow scheduling. After you select Start for Scheduling Status, **Scheduling** appears in the upper-right corner of the workflow editing canvas, which indicates that the workflow is being scheduled.|
    |**Time-based Scheduling**|In this section, you can specify **Start Time** and **Recurrence** for scheduling the workflow. During the specified period, the workflow runs based on the recurrence settings.|
    |**Dependency-based Scheduling**|Select a dependent workflow of the current workflow. The current workflow is executed only after the dependent workflow ends.     1.  Specify **Project**.
    2.  Then, select a dependent workflow from the **Dependent Workflow** drop-down list. |

6.  Click the **Alert Settings** tab to configure alert parameters.

    |Parameter|Description|
    |---------|-----------|
    |**Execution Failed**|Specifies whether to send a notification to a specific alert contact group or DingTalk alert group if the workflow fails.|
    |**Actions on Failures**|Specifies whether to send a notification to a specific alert contact group or DingTalk alert group if a job node in the workflow fails to run.|
    |**Executed**|Specifies whether to send a notification to a specific alert contact group or DingTalk alert group if the workflow succeeds.|
    |**Action on Startup Timeout**|Specifies whether to send a notification to a specific alert contact group or DingTalk alert group if a job node in the workflow does not start within 30 minutes after it is delivered to a cluster.|
    |**Node execution timed out**|Specifies whether to send a notification to a specific alert contact group or DingTalk alert group if the running time of a job node exceeds the expected maximum running duration in the job configuration.|


## Run a workflow

You can specify the business time of a workflow. Time variables in jobs of the workflow are calculated by using the specified business time. The business time is used for rerunning the workflow instance in a specific period of time. You can rerun a single workflow instance or multiple workflow instances at a time. If no time variables are configured for your jobs, you can select Execute.

1.  Click the **Data Platform** tab.

2.  In the **Projects** section of the page that appears, find your project and click **Workflows** in the Actions column.

3.  On the page that appears, select a specific workflow and click **Run** in the upper-right corner.

4.  Configure runtime parameters.

    -   **Execute**: Run a workflow immediately. You can use the specified time as the business time of the workflow. Time-related variables are calculated based on the business time.
    -   **Run Periodically**: Specify **Start Time** and **Recurrence**. After you turn on **Skip Successful Nodes**, if the workflow instance that runs at a specific business time is successful, the system skips this workflow instance and continues to run the workflow instances that fail at other business time.

        To run multiple workflows at the same time, you must specify the start time and recurrence. The trigger time of specific scheduling rules is used as the business time of the workflows, and time-related variables are calculated based on the business time. A maximum of 100 points in time are supported at a time.

5.  Click **OK**.


## View the running details of workflows

After you run a workflow, you can perform the following steps to view the running details of the workflow:

1.  Click the **Records** tab in the lower part of the workflow page.

    You can view the running status of a specific workflow instance.

2.  Find your workflow instance and click **Details** in the Actions column to go to the scheduling center.

    You can view the details of the workflow instance. You can also pause, resume, stop, or rerun the workflow instance. For more information, see [Scheduling center](/intl.en-US/Data Development/Scheduling center.md).

    -   View Details: You can view the details and running status of a workflow instance.
    -   Stop Workflow: When you click this button, all running job nodes stop running.
    -   Suspend current workflow: When you click this button, the running job nodes continue running, but the subsequent job nodes in the workflow will not start.
    -   Recovery Workflow: When you click this button, suspended workflow instances are resumed.
    -   Rerun Workflow: When you click this button, you can determine whether to rerun failed job nodes or rerun all job nodes from the START node.

## Operations that can be performed on workflows

In the **Workflows** pane, you can right-click a specific workflow and perform the following operations:

-   Clone Workflow: Clone a workflow with the same design to the same folder.

    **Note:** The settings of the scheduling parameters for the original workflow cannot be cloned.

-   Rename Workflow: Rename a workflow.
-   Delete Workflow: Delete a workflow. A running workflow cannot be deleted.

