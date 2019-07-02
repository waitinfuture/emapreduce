# Manage a workflow project {#concept_rqw_qz2_z2b .concept}

After creating an E-MapReduce cluster, you can create workflow projects so that multiple jobs can be run simultaneously or sequentially.

## Create a project {#section_bdl_sz2_z2b .section}

1.  At the top of the page, click the Data Platform tab to enter the **Projects** page.

    Under the master account, you can view all ofits projects and RAM useraccounts. RAM users can only view projects if they have development permissions. The granting of project development permissions must be configured in the master account. For more information about authorization, see [User management](#section_qvv_yz2_z2b) below.

2.  In the upper-right corner, click **New Project**. The **New Project** dialog box is displayed.
3.  Enter the project name and description and click **Create**.

    **Note:** You can only create a project with the master account. **New Project**is only visible to the administrator of the master account.


## User management {#section_qvv_yz2_z2b .section}

After creating a new project, you can grant operational permissions for the project to RAM user accounts.

1.  In the **Project List** page, click **View Details**in the **Actions** column.
2.  Click the **User Management** tab.
3.  Click **Add User**to add RAM users to the project under the master account.

    The added RAM users become members of the project and are able to view and develop the jobs and workflows under the project. If you remove aRAM user from a project, click **Delete** in the **Actions** column.

    **Note:** You can only add project members with the master account. The **User Management** tab is only visible to the administrator of the master account.


## Associate clusters {#section_qr2_21f_z2b .section}

After creating a new project, you need to associate it with a cluster so that the workflow in the project can run on it.

1.  In the **Projects** page, click **View Details**in the **Actions** column.
2.  Click the **Cluster Settings** tab.
3.  Click **Add Cluster**. From the drop-down menu, you can select a Subscription or Pay-As-You-Go cluster. \(Clusters created by temporary jobs are not listed here.\)
4.  Click **OK**.

    To disassociate the cluster, click **Delete** in the Operation column.

    **Note:** You can only associate cluster with the master account. The **Cluster Settings** tab is only visible to the administrator of the masteraccount.


To set both the queue and user to submit jobs to the cluster, click **Modify Configuration** in the Operation column. Theconfiguration items are as follows:

-   Default Submit Job User: Sets the default Hadoop user who submits the job to the selected cluster in the project. The default value is hadoop. There can only be one default user.
-   Default Submit Job Queue: Sets the default queue that the jobs are submitted to in the project. If you leave this blank, the job will be submitted to the default queue.
-   Submit Job User Whitelist: Sets Hadoop users who can submit jobs to the selected cluster in the project. If there is more than one user, they can be separated by acomma \(,\).
-   Submit Job Queue Whitelist: Sets the queue of the selected cluster that jobs in the project can run in. If there is more than one queue, they can be separated by a comma \(,\).
-   Client whitelist: Configures the client that can submit jobs. You can select either the E-MapReduce master node or the E-MapReduce gateway. Gateways that you have built are not currently listed here.

