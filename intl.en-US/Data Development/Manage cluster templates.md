# Manage cluster templates

Cluster templates contain saved configurations that you can use to create clusters.

Cluster templates are mainly used for the system to automatically create temporary clusters for data development workflows. If you are concerned only about the completion of the jobs in a data development workflow, you can specify a cluster template. The system creates a cluster based on the template you specified and delivers the jobs to the cluster. The cluster is automatically released after the workflow is completed.

-   Cluster templates can be used to create only Hadoop and Kafka clusters. If you want to create a cluster of another type, submit a ticket. For more information, see [submit a ticket](https://workorder-intl.console.aliyun.com/#/ticket/createIndex).
-   You cannot set a logon password for a cluster that is created from a cluster template.

## Create a cluster template

1.  Log on to the [Alibaba Cloud EMR console](https://emr.console.aliyun.com) by using your Alibaba Cloud account.

2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

3.  Click the **Data Platform** tab.

4.  In the **Projects** section of the page that appears, click **Cluster Template** in the upper-right corner. On the cluster template list page that appears, modify or delete a cluster template as needed:

    -   To modify a cluster template, click **Edit** in the Actions column. After the modification is saved, the modification immediately takes effect on all workflows that are using the cluster template.
    -   To delete a cluster template, click **Delete** in the Actions column.
5.  Click **Create Cluster Template** in the upper-right corner.

6.  On the **Create Cluster Template** page, configure relevant parameters.

    The operations required to create a cluster template are similar to those required to create a cluster. For more information, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

    In the **Hardware Settings** step of the Create Cluster Template page, you can configure multiple instance types. This allows you to prevent cluster creation failures caused by insufficient inventory of instances of a specified type, which may affect the jobs that you want to run.

    ![Multi-instance Type](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/1555560061/p88914.png)

    **Note:** When you delete a cluster template, the system does not check whether the cluster template is being referenced by a workflow. After the cluster template is deleted, all the workflows that use the cluster template fail.

7.  Read and agree to E-MapReduce Service Terms, and click **Save**.

    For more information about how to use cluster templates in workflows, see [Manage workflows](/intl.en-US/Data Development/Edit a workflow.md).


