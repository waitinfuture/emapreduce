# PAI-Alink

The PAI-Alink component in E-MapReduce \(EMR\) refers to Alink, which is a general algorithm platform developed by the Machine Learning Platform for Artificial Intelligence team based on Flink or Blink. This topic describes how to configure and use Alink in the EMR console.

-   An EMR Data Science cluster is created. For more information, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).
-   A Knox account is created. For more information, see [Manage users](/intl.en-US/Cluster Management/Cluster planning/Manage users.md).
-   Port 8443 is enabled. For more information, see [Grant access to a security group](/intl.en-US/Cluster Management/Configure clusters/Access open-source components.md).

## Access Alink

1.  Log on to the [Alibaba Cloud EMR console](https://emr.console.aliyun.com/).

2.  In the top navigation bar, select the region where your cluster residesand select a resource group based on your business requirements.

3.  Click the **Cluster Management** tab.

4.  Find the cluster from which you want to access Alink, and click **Details** in the **Actions** column.

5.  Configure Flink-VVP.

    1.  In the left-side navigation pane of the Cluster Overview page, click **Connect Strings**.

    2.  Click the link of **Flink-Vvp UI**.

    3.  On the logon page, enter the **username** and **password** of the created Knox account and click **Sign in**.

    4.  In the left-side navigation pane of the page that appears, choose **Administration** \> **Deployment Targets**.

    5.  Click **Add Deployment Target**.

    6.  In the **Add Deployment Target** dialog box, specify **Deployment Target Name**, set **Yarn Queue** to default, and then click **OK**.

        ![Add deployment target](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/6955693061/p127001.png)

6.  Configure Alink.

    1.  Go back to the EMR console. In the left-side navigation pane of the Cluster Overview page, click **Connect Strings**.

    2.  Click the link of **PAI-Alink UI**.

    3.  In the left-side navigation pane of the page that appears, click **Settings**.

    4.  Select the added deployment target from the **Switch Running Deployment Target** drop-down list and click **Submit**.


## Use Alink

On the homepage of Alink, you can click **Create from Template** for a template to create an experiment based on the template.

Select the required widgets and drag them to the canvas. Use the Scaring Card Function template as an example. To read local HDFS data, you only need to enter the URL of the local HDFS data on the Read CSV widget.

