# Workflow project management {#concept_rqw_qz2_z2b .concept}

After creating an E-MapReduce cluster, you can create workflow projects so that multiple jobs can be run simultaneously or sequentially.

## Create a project {#section_bdl_sz2_z2b .section}

1.  At the top of the page, click the Data Platform tab on the top to enter the **Projects** page.

    Under the master account, you can view projects of itself and its all sub-accounts. You can only view projects with development permission in the sub-account. To authorize project development permission, you need to configure it in the primary account. For more information about authorization, see [User Management](#section_qvv_yz2_z2b).

2.  In the upper right corner, click the **New Project** button to see the **New Project** dialog box.
3.  Enter the project name and description, and click **Create**.

    **Note:** You can only create a project with the master account, that is, the **Create Project** button is only visible to the master account administrator.


## User management {#section_qvv_yz2_z2b .section}

After creating a new project, you can authorize the RAM sub-account operational permission of the project.

1.  In the **Project List** page, click the **View Details** link in the **Actions** column.
2.  Click the **User Management** tab.
3.  Click the **Add User** button to add RAM sub-accounts under the master account to the project.

    The added sub-accounts will be a member of the project and can be able to view and develop the jobs and workflows under the project. If you don't want to set the sub-account to be the selected project member any more, click **Delete** in the **Actions** column.

    **Note:** You can only add project members with the primary account , that is, the **User Management** tab is only visible to the primary account administrator.


## Associate clusters {#section_qr2_21f_z2b .section}

After creating a new project, you need to associate a cluster for the project so that the workflow in the project can run on it.

1.  In the **Projects** page, click the **View Details** link in the **Actions** column.
2.  Click the **Cluster Settings** tab.
3.  Click the **Add Cluster** button, from the drop-down menu, you can select the created Subscription and Pay-As-You-Go clusters \(clusters created by running temporary jobs are not listed here\).
4.  Click **OK**.

    You can click **Delete** in the Operation column to unassociate the cluster.

    **Note:** You can only associate cluster with the primary account , that is, the **Cluster Settings** tab is only visible to the primary account administrator.


Click **Modify Configuration** in the Operation column to set up the queue and user to submit jobs to the cluster. The specific configuration items are described as follows:

-   Default Submit Job User: Sets the default Hadoop user who submits the job to the selected cluster in the project. The default value is hadoop, and there only can be one default user.
-   Default Submit Job Queue: Sets the default queue that the jobs are submited to in the project. If you leave this blank, the job will be submitted to the default queue.
-   Submit Job User Whitelist: Sets Hadoop users who can submit jobs to the selected cluster in the project. If there are more than one users, they can be separated by a half-width comma \(,\).
-   Submit Job Queue Whitelist: Sets the queue of the selected cluster that jobs in the project can run in. If there are more than one queues, they can be separated by a half-width comma \(,\).
-   Client whitelist: Configures the client that you can submit jobs. The E-MapReduce master node or the E-MapReduce Gateway can be selected. Currently, your self-built gateways are not listed here.

