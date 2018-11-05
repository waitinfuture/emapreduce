# Cluster alarm management {#concept_f22_tp5_y2b .concept}

To help you monitor the operation of clusters, CloudMonitor offers multiple monitoring metrics for E-MapReduce clusters, including CPU idleness, memory capacity, and disk capacity. It also allows you to set alarm rules for these monitoring metrics. Once an alarm rule is triggered, CloudMonitor notifies the contacts in the notification group timely so that you can deal with the problem in time.

## Configure alarm rules {#section_dyw_xp5_y2b .section}

To set up alarm rules for E-MapReduce cluster, take the following steps:

1.  Log on to [CloudMonitor console](https://cloudmonitor.console.aliyun.com/#/home/ecs).
2.  In the left navigation panel, click **Cloud Service Monitoring** \> **E-MapReduce** to go to the E-MapReduce Monitoring List page.
3.  Click **Alarm Rules** tab.
4.  In the upper-right corner of the page, click **Create Alarm Rule** to set an alarm rule for corresponding monitoring metrics.
5.  In the **Related Resource** region, set Products and Resource Range.
    -   **Products**: Select E-MapReduce in the drop-down list.
    -   **Resource Range**: The scope of action of the alarm rule. It is divided into three areas: all resources, application group and cluster. When all resources is selected, the maximum number of resources that can be monitored is 1000, problem that the threshold has been reached without alarming may occur if it exceeds 1000, we recommend that you use application group to divide resources according to business before setting up alarm rules.
        -   **All Resources**: Indicates that the rule works on all instances of E-MapReduce for the current account. For example, set The CPU usage is greater than 80% to alarm, as long as there is instance whose CPU usage is greater than 80% in the current account, it hits this rule.
        -   [**Application Group**](../../../../intl.en-US/User Guide/Application Groups/Application groups.md#): Indicates that the rule works on all instances of a certain application group. For example, set the CPU usage is greater than 80% to alarm, as long as there is a host whose CPU usage is greater than 80% in this group, it hits this rule.
        -   **Cluster**: indicates that the rule only works on a specific E-MapReduce cluster. For example, set the CPU usage is greater than 80% to alarm, as long as there is an instance whose CPU usage is greater than 80% in this cluster, it hits this rule.
6.  Configure the **Set Alarm Rules** region.
    -   **Alarm Rule**: Sets the name of alarm rules.
    -   **Rule Description**: The principal of the alarm rule that defines what conditions the item data meets, trigger alarm rules. For example, the rule is described as a 1 minute average of CPU usage \>= 90%, it means that the rule is hit if the average data within 1 minute is \>= 90%. For more information about monitoring metrics for E-MapReduce clusters, see [E-MapReduce monitoring](../../../../intl.en-US/User Guide/Cloud service monitoring/E-MapReduce monitoring.md#).
    -   **Role**: By default, any role is applicable.

        Click **Add Alarm Rule**, you can set multiple alarm rules \(charge as multiple alarm rules\). As long as one of the rules is triggered, the system sends notifications to the notification group.

    -   **Mute for**: The interval for sending the alarm notification again in case of the monitoring metrics is not restored.
    -   **Triggered when threshold is exceeded for**: Times that the rule is hit to send alarm notifications. For example, the rule is described as a 1 minute average of System State CPU usage \>= 90% and it appears for 3 times or more continuously, it means that the rule is hit if the average data within 1 minute is \>= 90% and it appears for 3 times continuously.
    -   **Effective Period**: The effective time of the alarm rule. CloudMonitor checks whether the monitoring metric hits the alarm rule only during the effective period. The system checks whether the monitoring data requires an alarm only during the effective time.
7.  Configure the **Notification Method** region.
    -   **Notification Contact**: The contact group that receives the notification. Enter the keyword of the contact group in the search box to locate the contactn group you want to associate quickly, and click the right arrow icon, then the contact group is added into the right contact list. If you haven’t created the appropriate contact group, click **Quickly create a contact group**. After you select a contact group in the right contact list, click the left arrow icon to delete the contact group.
    -   **Notification Methods**: Alarm information is divided into three levels: critical, warning, info. Different levels correspond to different notification methods. Different alarm levels correspond to different notification methods. Before Critical level is configured, you need to buy the phone alarm resource package.
    -   **Email Remark** \(Optional\): Customizes supplemental information for alarm email. When you fill in your email remarks, your comments are included in the notification message that is being sent to the contacts.
    -   **HTTP CallBack**\(Optional\): This feature allows you to integrate the alarm notifications sent by CloudMonitor into existing maintenance systems or message notification systems. CloudMonitor pushes alarm notifications to a specified public URL through the POST request of HTTP protocol. When you receive the alarm notification, you can make further process according to the notification content. For more information, see [Alarm callback](../../../../intl.en-US/User Guide/Alarm Service/Alarm callback.md#).
8.  Click **Confirm** to complete the alarm rule configuration

