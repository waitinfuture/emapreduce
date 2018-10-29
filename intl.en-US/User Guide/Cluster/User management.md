# User management {#concept_ebw_rpl_z2b .concept}

User management allows you to manage accounts which are required to create the related services on specified clusters. E-MapReduce currently supports two types of accounts: Knox accounts and Kerberos accounts.

## Create a RAM account {#section_wlj_tpl_z2b .section}

1.  Log on to the [Alibaba Cloud E-MapReduce Console](https://emr.console.aliyun.com/console), and then go to the **Cluster Management** page.
2.  Click **Manage** on the right side of the target cluster ID.
3.  In the left-side navigation panel, click **User Management**.
4.  In the upper-right corner of the page, click **Create RAM User**.

## Add a Knox account {#section_tgv_tpl_z2b .section}

1.  In the **User Management** page, select the account you want to add to a cluster, and then click **Set Knox Account Password**in the **Actions** column.
2.  In the **Add Knox User** dialog box, enter a password to use for logon. Click **OK**.
3.  Refresh the **User Management** panel. When **Synchronized** appears in the **Knox Account** column, the Knox account is successfully added.

    After the addition is successful, you can sign in to Knox using the **User Name** and the password set in [step 2](#).


## Delete a Knox account {#section_vqv_tpl_z2b .section}

1.  In the **User Management** panel, select the account you want to add to a cluster, and then click **Delete Knox Account**in the **Actions** column.
2.  Refresh the **User Management** page. When **Unsynchronized** appears in the Knox Account column, the Knox account is successfully deleted.

## FAQs {#section_r2w_tpl_z2b .section}

-   Different clusters cannot share the same Knox account. For example, the Knox account A you added in cluster-1 cannot be used in cluster-2. If you want to use the Knox account A in cluster-2, you must re-add the account A in cluster-2. Knox accounts are created in clusters, so the Knox accounts of each cluster are not interoperable.

-   If the message An **error occurred** while synchronizing the status appears when you add a Knox account, click **Retry** to add it again.

-   If you **retry** adding an account multiple times but it continues to fail, on the left side of the page, click **Clusters and Services** to check if ApacheDS is stopped. If yes, start ApacheDS and go back to **User Management** then try again.


