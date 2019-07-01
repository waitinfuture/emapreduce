# User management {#concept_ebw_rpl_z2b .concept}

User management allows you to manage the accounts required to create services on specified clusters. E-MapReduce currently supports the creation of two types of accounts: Knox and Kerberos. This topic explains how to manage Knox accounts.

## Create a RAM account {#section_wlj_tpl_z2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce console](https://partners-intl.console.aliyun.com/#/emr) and go to the **Cluster Management** page.
2.  Click **Manage** on the right side of the target cluster ID.
3.  In the navigation panel on the left, click **User Management**.
4.  In the upper-right corner of the page, click **Create RAM User**.

## Add a Knox account {#section_tgv_tpl_z2b .section}

1.  In the **User Management** page, select the account you want to add to a cluster, and then click **Set Knox Account Password** in the **Actions** column.
2.  In the **Add Knox User** dialog box, enter a password to use for logon and click **OK**.
3.  Refresh the **User Management** page. When **Synchronized** is displayed in the **Knox Account** column, you have successfully added the Knox account.

    You can then sign in to Knox using the **User Name** and the password set in the preceding step.

    For more information about how to use Knox, see [../../../../dita-oss-bucket/SP\_159/DNEMAPREDUCE19100417/EN-US\_TP\_17921.md\#](../../../../reseller.en-US/Open Source Components /Knox.md#).


## Delete a Knox account {#section_vqv_tpl_z2b .section}

1.  In the **User Management** panel, select the account you want to delete from a cluster, and then click **Delete Knox Account** in the **Actions** column.
2.  Refresh the **User Management** page. When **Unsynchronized** is displayed in the **Knox Account** column, you have successfully deleted the Knox account.

## Kerberos account {#section_opm_spn_7v7 .section}

When this Kerberos mode is enabled, the software on the cluster runs in Kerberos security mode. For more information, please refer to [Introduction to Kerberos](../../../../reseller.en-US/Open Source Components /Kerberos authentication/Introduction to Kerberos.md#).

## FAQs {#section_r2w_tpl_z2b .section}

-   Different clusters cannot share the same Knox account. For example, Knox account A that you added to cluster-1 cannot be used in cluster-2. If you want to use Knox account A in cluster-2, you must re-add account A to cluster-2.

-   If the message **An error occurred while synchronizing the status** is displayed when you add a Knox account, click **Retry** to add it again.

-   If you try to add an account multiple times but it fails each time, click **Clusters and Services** on the left side of the page to check if ApacheDS is stopped. If it is, start ApacheDS and go back to **User Management** to try again.


