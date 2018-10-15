# Zeppelin {#concept_vpf_j1x_y2b .concept}

E-MapReduce currently supports Appache Zeppelin. Zeppelin can be accessed and used in E-MapReduce just by selecting a Zeppelin-supported mirror image to create a cluster and enabling public network IP address.

## Preparation {#section_zsr_k1x_y2b .section}

1.  In the cluster [Security group](intl.en-US/User Guide/Cluster/Security group.md#), [set security group rules](https://www.alibabacloud.com/help/doc-detail/25471.html), and open the 8080 port.
2.  In Knox, add user name and password. For details, see [Knox guide](intl.en-US/User Guide/Open source assembly/Knox guide.md#) to set Knox users. The user name and password are used only to log on to Knox's various services which have no relationship with Alibaba Cloud Ram user names.

**Note:** Set security group rules for limited IP ranges. It is prohibited to open rules to 0.0.0.0/0 when configuring.

## Access zzeppelin {#section_btr_k1x_y2b .section}

Access links can be viewed as follows:

1.  On the right-side of the EMR management console, click **Manage**.
2.  On the left side of the page, click **Access links and ports**.

