# Create an execution plan {#concept_lz1_dhp_y2b .concept}

This section describes how to create an execution plan in an earlier E-MapReduce version.

After you have created a job, you need to create an execution plan if you want to run a predefined job on a cluster. An execution plan may contain multiple jobs. You can define the execution sequence of these jobs. For example, if a scenario handles data in the following order: prepare data, process data, and clean up data, then you can define the following three jobs: **prepare-data**, **process-data**, and **cleanup-data**, and then you can create an execution plan that includes these three jobs.

To create an execution plan, follow these steps:

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://emr.console.aliyun.com/).
2.  Select a region.
3.  Click Old EMR Scheduling.
4.  Click the left-side Execution Plan tab to go to the execution plan page.
5.  Click **Create an execution plan** in the upper-right corner to go to the execution plan creation page.
6.  The following two options are available on the Select the cluster page: **Create as needed** and **Existing Clusters**. **Create as needed** indicates that you have not created any clusters. You will run the execution plan in a temporary cluster, which will be automatically released after you have executed the plan. **Existing Cluster** indicates that you have at least one running cluster, and you have to submit the execution plan to the existing cluster for execution.

    1.  If you select **Create as needed**, you can follow the same steps as creating a cluster to create an on-demand cluster. Select configuration of the on-demand cluster, and click OK.
    2.  If you select **Existing Cluster**, you will be directed to the cluster selection page. You can select clusters that you want to associate with the execution plan.

        ![Create an E-MapReduce excution plan](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17842/154814135010520_en-US.png)

    Currently, you can only submit clusters that are in **Running** or **Idle** status.

7.  Click **Next** to go to the Configure the job page. The page displays the predefined jobs on the left-side list, and the jobs to be executed by the newly created execution plan on the right-side list. To define the execution plan, select jobs from the left-side list based on the execution sequence and add them to the right-side list. You can click the question mark \(?\) to view parameters of the jobs. After you have defined the execution plan, click **Next**.

    ![Configure the job](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17842/154814135010526_en-US.jpg)

8.  Enter an execution plan name.
9.  Select a scheduling policy.

    -   Manually executed indicates that the execution plan will be executed only when you click the execution plan.
    -   Periodic scheduling execution plan indicates that you must set the scheduling cycle and the scheduling start time.
    ![Configure the scheduling](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17842/154814135010527_en-US.jpg)

10. Click **OK** to complete the creation of the execution plan.

