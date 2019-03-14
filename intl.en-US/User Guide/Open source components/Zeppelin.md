# Zeppelin {#concept_vpf_j1x_y2b .concept}

E-MapReduce can access Zeppelin through Apache Knox.

## Preparation {#section_zsr_k1x_y2b .section}

1.  In the [Security groups](intl.en-US/User Guide/Clusters/Security groups.md#) cluster, [set the security group rules](../../../../../intl.en-US/Security/Security groups/Add security group rules.md#), and open port 8080.
2.  In Knox, add a user name and password. For more information on how to set Knox users, see [Knox](intl.en-US/User Guide/Open source components/Knox.md#). The user name and password are only used to log on to the various Knox services. They are not related to Alibaba Cloud RAM user names.

**Note:** Set security group rules for limited IP ranges. IP 0.0.0.0/0 is not allowed to add into the security group.

## Access Zeppelin {#section_btr_k1x_y2b .section}

To view the access links for Zeppelin, complete the following steps:

1.  On the right of the cluster list page, click **Manage**.
2.  In the pane on the left, click **Access Links and Ports**.

