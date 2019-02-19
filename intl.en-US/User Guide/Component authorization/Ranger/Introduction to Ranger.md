# Introduction to Ranger {#concept_gpl_jrc_z2b .concept}

Apache Ranger provides a centralized framework for permission management, implementing fine-grained access control for components in the Hadoop ecosystem, such as HDFS, Hive, YARN, Kafka, Storm, and Solr. It also provides a UI that allows administrators to perform operations more conveniently.

## Create a cluster {#section_e4j_sj3_bfb .section}

Select the Ranger service when you create a cluster in E-MapReduce 2.9.2/3.9.0 or later on the E-MapReduce console.

![Create a cluster](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17948/155054544511486_en-US.png)

If an E-MapReduce cluster 2.9.2/3.9.0 or later has been created without Ranger, you can go to the Clusters and Services page to add it.

![Clusters and Services](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17948/155054544611487_en-US.png)

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17948/155054544611488_en-US.png)

**Note:** 

-   When Ranger is enabled, there is no impact or limitation on the application until the security control policy is set.
-   The user policy set in Ranger is the cluster Hadoop account.

## Ranger UI {#section_mcp_pk3_bfb .section}

After installing Ranger on the cluster, click **Manage** in the **Actions** column, and then click **Access Links and Ports** in the navigation pane on the left. You can then access the Ranger UI by clicking on the link, as shown in the following figure.

![Access Links and Ports](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17948/155054544611489_en-US.png)

Enter the Ranger UI. The default user name and password are both **admin**, as shown in the following figure.

![Ranger UI](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17948/155054544611490_en-US.png)

![Service Manager](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17948/155054544611491_en-US.png)

## Modify the password {#section_igf_ll3_bfb .section}

After you first log on, the administrator needs to modify the password of the admin account, as shown in the following figure.

![Modify the password](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17948/155054544611492_en-US.png)

![Modify the password](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17948/155054544611493_en-US.png)

After you change the admin password, in the **admin** drop-down list in the upper-right corner, click **Log Out**. You can then log on again with the new password.

## Integrate Ranger into other services {#section_lnk_tl3_bfb .section}

After completing the preceding steps, you can integrate Ranger into the services in the cluster to control the relevant permissions. For more information, see the following:

-   [Integrate Ranger into HDFS](reseller.en-US/User Guide/Component authorization/Ranger/Integrate Ranger into HDFS.md#)
-   [Integrate Ranger into Hive](reseller.en-US/User Guide/Component authorization/Ranger/Integrate Ranger into Hive.md#)
-   [Integrate Ranger into HBase](reseller.en-US/User Guide/Component authorization/Ranger/Integrate Ranger into HBase.md#)

