# Create an execution plan {#concept_lz1_dhp_y2b .concept}

In this tutorial, you will learn how execution plans are created in E-MapReduce.

After job creation, if you want to run the job defined on the cluster, you need to create an execution plan. An execution plan can contain more than one job, and you can define their order. For example, if one of your scenarios is: prepare data \> process data \> clean up data, you can define three jobs named **prepare-data**, **process-data**, and **cleanup-data**, and then create an execution plan to include these three jobs.

The steps to create an execution plan are as follows:

1.  Log on to the [Execution Plan page of Alibaba Cloud E-MapReduce Console](https://emr.console.aliyun.com).
2.  Select a region for your cluster.
3.  At the top of the navigation bar, click **Old EMR Scheduling**.
4.  In the right-side panel, click **Execution plan**.
5.  In the upper-right corner, click **Create an execution plan**.
6.  In the **Select the cluster mode** pane, there are two options,
    -   **Create as needed**: The cluster is created on demand, and the cluster is released immediately after the execution of the plan. If this option is selected, you must execute the same steps as creating a cluster, configure a cluster as needed, and then click OK.
    -   **Existing clusters**: Select a cluster from the cluster you have created. If this option is selected, you will go to the **Select the cluster** panel where you can set an associated cluster and see the cluster information.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17842/154046142410520_en-US.png)

7.  Click **Next** to enter the **Configure the job** page.

    The page will display the previously defined job list, and the job list to be run as per the newly created execution plan on the right. Select jobs on the left to populate the right as per the execution order. You can click the question mark to view the detailed parameters.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17842/154046142510526_en-US.jpg)

8.  Click **Next**.
9.  In the **Configure the scheduling** pane, enter the execution plan name and select a scheduling policy.
    -   **Manually executed**: Execution plans are executed manually.
    -   **Periodic scheduling execution plan**: Set the **scheduling cycle** and **first execute time** to run execution plans automatically.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17842/154046142510527_en-US.jpg)

10. Click **OK**.

