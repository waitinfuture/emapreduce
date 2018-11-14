# Zeppelin {#concept_vpf_j1x_y2b .concept}

Alibaba Cloud E-MapReduce can access Zeppelin through Apache Knox.

## Preparation {#section_zsr_k1x_y2b .section}

1.  In the [Security group](intl.en-US/User Guide/Cluster/Security group.md#) cluster, [set security group rules](../../../../intl.en-US/User Guide/Security groups/Add security group rules.md#), and open the 8080 port.
2.  In Knox, add user name and password. For details, see [Knox guide](intl.en-US/User Guide/Open source components/Knox guide.md#) to set Knox users. The user name and password are used only to log on to Knox's various services which have no relationship with Alibaba Cloud Ram user names.

**Note:** Set security group rules for limited IP ranges. It is prohibited to open rules to 0.0.0.0/0 when configuring.

## Access zzeppelin {#section_btr_k1x_y2b .section}

Access links can be viewed as follows:

1.  On the right-side of the cluster list page, click **Manage**.
2.  In the left-side pane, click **Access Links and Ports**.

