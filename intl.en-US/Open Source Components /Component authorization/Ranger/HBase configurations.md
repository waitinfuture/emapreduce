# HBase configurations {#concept_gsf_rmj_bfb .concept}

The Ranger introduction topic describes how to start a Ranger cluster in the EMR console and the preparations. This topic describes how to integrate HBase with Ranger.

## Integrate HBase in Ranger {#section_gcm_wnj_bfb .section}

## Enable HBase Plugin {#section_hnm_b4j_bfb .section}

1.  On the Cluster Management page, click **Clusters and Services** in the Actions column for the cluster that you want to operate.
2.  In the Services list, click **RANGER** and click the Configuration tab to go to the Configuration tab page.
3.  On the Configuration tab page, select **EnableHBase** from the **Actions** drop-down list.

    ![Enable HBase PLUGIN](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17951/155712215911513_en-US.png)

4.  \[DO NOT TRANSLATE\]****
5.  Click **View Operation Logs** to view the status of operations.![View the status of operations](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17951/155712215911514_en-US.png)

## Add HBase Service in Ranger UI {#section_elq_cpj_bfb .section}

For access to Ranger UI, see [Ranger introduction](reseller.en-US/Open Source Components /Component authorization/Ranger/Introduction to Ranger.md#).

Add the HBase service in Ranger UI.

![Add HBase service](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17951/155712215911521_en-US.png)

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17951/155712215911522_en-US.png)

**Note:** $\{id\}: You can log on to the host and run the `host`command. The number in hostname is the value of $\{id\}.

## Restart HBase {#section_nkb_sqj_bfb .section}

Restart HBase for the preceding procedures to take effect. Perform the following steps.

1.  On the Ranger page, click RANGER. From the drop-down list, select HBase.
2.  From the **Actions** drop-down list, select **RESTART All Components**.
3.  \[DO NOT TRANSLATE\]****
4.  Click **View Operation Logs** to view the status of operations.

## Set Administrator Account {#section_twd_xqj_bfb .section}

You need to set permissions of the administrator accounts \(administrator permissions\) for running administrative commands such as `balance/compaction/flush/split`.

![Set the administrator account](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17951/155712215911523_en-US.png)

Click the Edit icon in the Action column of the policy that you want to set users for. Add user accounts in the Users column as needed. You can also modify the permissions. For example, only retain the default admin permissions. You need to set hbase as the administrator account.

If you use Phoenix, you also need to add the following policies in HBase for Ranger.

|Table|SYSTEM. \*|
|-----|----------|
|Column Family|`*`|
|Column|`*`|
|Groups|`public`|
|Permissions|`Read` `Write`, `Create`, `Admin`|

![Add permission policies](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17951/155712216032580_en-US.png)

## Permission configuration examples {#section_rcd_2rj_bfb .section}

After HBase is integrated into Ranger, you can perform permission configurations. For example, grant user test the `Create/Write/Read` permissions on the foo\_ns:test table.

![Permission configuration examples](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17951/155712216011524_en-US.png)

Click **emr-hbase** as shown in this figure to go to the configurations page. Configure permissions.

![Configure permissions](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17951/155712216011525_en-US.png)

It takes about one minute to synchronize the user and group information of the cluster.

Follow the steps as shown in the figure to complete the adding of the policy. Then user test can access the foo\_ns:test table.

**Note:** A policy will not take effect until it has been added over one minute.

