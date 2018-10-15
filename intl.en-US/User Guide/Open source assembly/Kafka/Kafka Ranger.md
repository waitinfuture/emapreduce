# Kafka Ranger {#concept_blg_2gx_y2b .concept}

Starting from EMR-3.12.0 version, E-MapReduce Kafka allows you to configure permissions with Ranger.

## Integrate Ranger into Kafka {#section_kk2_fxb_z2b .section}

The previous section introduced how to create a cluster with Ranger service in E-MapReduce and some preparation work. This section describes the step-by-step process for integrating Ranger into Kafka.

-   **Enable Kakfa Plugin**
    1.  On **Cluster Management** page, click **Ranger** in the service list to enter the Ranger Management page. Click **Operation** at the upper right corner of the page and select **Enable Kafka PLUGIN**.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/153959417610838_en-US.png)

    2.  You can check the progress by clicking **View Operation History** at the upper right corner of the page.
-   Restart Kafka Broker

    After the preceding task is completed, it is necessary to restart the broker to make it take effect.

    1.  In the cluster management page, click the inverted triangle icon behind **RANGER** in the upper left corner to switch to **Kafka**.
    2.  Click **Actions** at the upper right corner of the page and select **RESTART Broker**.
    3.  You can check the progress by clicking **View Operation History** at the upper right corner of the page.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/153959417710840_en-US.png)

-   **Add Kafka service on Ranger WebUI**

    For information about how to go to Ranger WebUI, see [Ranger Introdcution](https://www.alibabacloud.com/help/doc-detail/66410.htm?spm=a2c63.p38356.a3.3.76e92bfeUUfFKM#concept_gpl_jrc_z2b).

    Add the Kafka service on **Ranger** WebUI:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/153959417710841_en-US.png)

    Configure Kafka Service:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/153959417710842_en-US.png)


## Permissions configuration examples {#section_nkp_hyb_z2b .section}

The preceding section has integrated Ranger into Kafka, which allows you to set relevant permissions.

**Note:** In a standard cluster, Ranger generates the **all - topic** rule by default after the Kafka service is added. This rule means no restriction on permissions \(that is, allow all users to perform all actions\). In this case, Ranger cannot identify permissions through the user.

Use user test as an example to add the Publish permission:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/153959417710843_en-US.png)

After you add a policy by following the preceding steps, the permissions are granted to user test. The test user can perform the write operation on the topic of test.

**Note:** The policy takes effect 1 minute later after it is added.

