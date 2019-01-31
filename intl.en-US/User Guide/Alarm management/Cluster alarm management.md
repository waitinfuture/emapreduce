# Cluster alarm management {#concept_f22_tp5_y2b .concept}

To help you monitor cluster operations, CloudMonitor offers multiple monitoring metrics for E-MapReduce clusters, including CPU idleness, memory capacity, and disk capacity. It also allows you to set alarm rules for these monitoring metrics. Once an alarm rule is triggered, CloudMonitor promptly notifies the contacts in the notification group, allowing you to deal with the problem in time.

## Configure alarm rules {#section_dyw_xp5_y2b .section}

To set an alarm rule for an E-MapReduce cluster, perform the following steps:

1.  Log on to [CloudMonitor console](https://cms-intl.console.aliyun.com/).
2.  In the navigation pane on the left, click **Cloud Service Monitoring** \> **E-MapReduce** to go to the E-MapReduce monitoring list page.
3.  Click the **Alarm Rules** tab.
4.  In the upper-right corner of the page, click **Create Alarm Rule** to set an alarm rule for a monitoring metric.
5.  In the **Related Resource** area, set **Products** and **Resource Range**.
    -   **Products**: Select E-MapReduce in the drop-down list.
    -   **Resource Range**: The scope of the alarm rule, which is divided into three areas: all resources, application group, and cluster. When all resources is selected, the maximum number of resources that can be monitored is 1,000. If you have more than 1,000 resources, an alarm may not occur when the threshold is reached. We recommend that you set the alarm rules first before using application group to divide resources according to your needs.
        -   **All Resources**: Indicates that the rule works on all E-MapReduce instances under the account. For example, if you set an alarm for when the CPU usage becomes greater than 80%, as long as there is an instance whose CPU usage is greater than 80% in the account, the rule is triggered.
        -   [**Application Group**](../../../../../intl.en-US/User Guide/Application groups/Application group overview.md#): Indicates that the rule works on all instances of a certain application group. For example, if you set an alarm for when the CPU usage becomes greater than 80%, as long as there is a host whose CPU usage is greater than 80% in this group, the rule is triggered.
        -   **Cluster**: Indicates that the rule only works on a specific E-MapReduce cluster. For example, if you set an alarm for when the CPU usage becomes greater than 80%, as long as there is an instance whose CPU usage is greater than 80% in this cluster, the rule is triggered.
6.  Configure the **Set Alarm Rules** area.
    -   **Alarm Rule**: Sets the name of an alarm rule.
    -   **Rule Description**: The part of the alarm rule that defines whether the conditions have been met to trigger the alarm rule. If they have been met, the alarm rule is triggered. For example, if the rule is set as an average CPU usage within 1 minute of greater than or equal to 90%, the rule is triggered if the average CPU usage within 1 minute is greater than or equal to 90%. For more information about the monitoring metrics in E-MapReduce clusters, see [E-MapReduce monitoring](../../../../../intl.en-US/User Guide/Cloud service monitoring/E-MapReduce monitoring.md#).
    -   **Role**: By default, any role is applicable.

        If you click **Add Alarm Rule**, you can set multiple alarm rules \(which are billed as such\). If one of the rules is triggered, the system sends notifications to the notification group.

    -   **Mute for**: The interval at which alarm notifications are sent again in the event that the monitoring metrics are not returned to normal after an alarm occurs.
    -   **Triggered when threshold is exceeded for**: Number of times the rule is triggered to send alarm notifications. For example, if the rule is set as an average CPU usage within 1 minute of greater than or equal 90%, and this happens on 3 occasions, the rule is triggered if the average CPU usage within 1 minute is greater than or equal to 90% on 3 occasions.
    -   **Effective Period**: The effective time of the alarm rule. CloudMonitor only checks whether the monitoring metric triggers an alarm rule during this effective period. The system also only checks whether the monitoring data requires an alarm during the effective time.
7.  Configure the **Notification Method** area.
    -   **Notification Contact**: The contact group that receives the notification. Enter a keyword for the contact group in the search box to quickly locate the group that you want to associate. By clicking the right arrow icon, the contact group is added to the contact list on the right. If you have not yet created a contact group, click **Quickly create a contact group**. If you want to delete a contact group from the contact list on the right, click the left arrow icon.
    -   **Notification Methods**: Alarm information is divided into three levels: critical, warning, and information. These different levels correspond to different notification methods. Before you can configure the critical level, you must first purchase the phone alarm resource package.
    -   **Email Remark** \(Optional\): Customizes additional information for alarm emails. The remarks you enter are included in the notification message sent to contacts.
    -   **HTTP Callback** \(Optional\): This feature allows you to integrate the alarm notifications sent by CloudMonitor into existing maintenance systems or message notification systems. CloudMonitor pushes alarm notifications to a specified public URL through the POST request method supported by HTTP. When you receive an alarm notification, you can process it further according to its content. For more information, see [Alarm callback](../../../../../intl.en-US/User Guide/Alarm service/Create an alarm callback.md#).
8.  Click **Confirm** to complete the alarm rule configuration

