# Cluster alert management

This topic describes how to use CloudMonitor to configure alert rules for E-MapReduce \(EMR\) clusters. CloudMonitor provides monitoring metrics such as CPU idleness, memory capacity, and disk capacity to help you monitor the running status of EMR clusters. If an alert rule is triggered, CloudMonitor notifies all contacts in the notification group.

## Configure alert rules

To configure alert rules for EMR clusters, follow these steps:

1.  Log on to the [CloudMonitor console](https://cloudmonitor.console.aliyun.com/#/home/ecs).
2.  In the left-side navigation pane, choose **Cloud Service Monitoring** \> **E-MapReduce**.
3.  On the E-MapReduce Monitoring List page that appears, click the **Alarm Rules** tab.
4.  Click **Create Alarm Rule** in the upper-right corner.
5.  On the Create Alarm Rule page that appears, configure parameters in the **Related Resource** section.
    -   **Product**: the product. Select E-MapReduce from the drop-down list.
    -   **Resource Range**: the scope of alert rules. Valid values: All Resources and Cluster. If you set Resource Range to All Resources, the alert rule applies to 1,000 instances at most.
        -   **All Resources**: indicates that the alert rule applies to all your instances of a specific service. For example, you set Resource Range to All Resources and set the alert threshold for instance CPU utilization to 80%. CloudMonitor generates an alert when the CPU utilization of an instance under your account is greater than 80%.
        -   **Cluster**: indicates that the alert rule applies to instances of a specific cluster. For example, you set Resource Range to Cluster and set the alert threshold for instance CPU utilization to 80%. CloudMonitor generates an alert when the CPU utilization of an instance in a specific cluster is greater than 80%.
6.  Configure parameters in the **Set Alarm Rules** section.
    -   **Alarm Rule**: the name of the alert rule.
    -   **Rule Description**: the content of the alert rule. This parameter defines the metric conditions that trigger the rule. For example, the rule is described as follows: The average CPU utilization, detected at an interval of one minute, is greater than or equal to 90%. CloudMonitor generates an alert when the average CPU utilization is greater than or equal to 90%.
    -   **Role**: Anyrole is selected by default.

        You can click **Add Alarm Rule** to configure several alert rules \(all these rules are charged\). If a rule is triggered, CloudMonitor sends a notification to the notification group.

    -   **Mute for**: specifies the interval at which an alert notification is sent if the issue is not rectified after the first notification.
    -   **Effective Period**: the period each day for when the rule takes effect. CloudMonitor checks whether the monitoring data requires an alert only during the effective period.
7.  Configure parameters in the **Notification Method** section.
    -   **Notification Contact**: You can perform the following operations to add a notification group to the Selected Groups list: Enter the name keyword of a target notification group in the search box to find the target notification group and click the right arrow. If no appropriate notification group is created, click **Quickly create a contact group**. You can select a notification group from the Selected Groups list and click the left arrow to delete it.
    -   **Notification Methods**: Valid values: Phone + Text Message + Email + DingTalk \(Critical\), Text Message + Email + DingTalk \(Warning\), and Email + DingTalk \(Info\). Different severities require different notification methods. If the critical level is required, purchase the phone alert resource package.
    -   **Email Remark**: optional. You can add remarks to email notifications. CloudMonitor sends the remarks along with the email.
    -   **HTTP CallBack**: optional. This feature allows you to integrate alert notifications from CloudMonitor to existing O&M or message notification systems. CloudMonitor pushes alert notifications to a specific Internet URL with HTTP POST requests. You can take actions based on received notifications. For more information, see [Create an alert callback](/intl.en-US/Alarm service/Alarm rules/Use alert callbacks.md).
8.  Click **Confirm**.

## Configure alert rules based on application groups

[Application groups](/intl.en-US/Application groups/Overview.md) indicates that the alert rule applies to all instances in an application group. For example, you set the alert threshold for CPU utilization to 80% for instances in an application group. CloudMonitor generates an alert when the CPU utilization of an instance in the application group is greater than 80%. If an alert is generated for more than 1,000 instances, the alert may not be reported when the alert threshold is reached. We recommend that you create application groups to divide instances by service and then configure alert rules. For detailed operations, see [Create application groups](/intl.en-US/Application groups/Create an application group.md) and [Apply an alert template to an application group](/intl.en-US/Application groups/Apply an alert template to an application group.md).

