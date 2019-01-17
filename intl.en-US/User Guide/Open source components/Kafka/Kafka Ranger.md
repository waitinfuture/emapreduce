# Kafka Ranger {#concept_blg_2gx_y2b .concept}

With E-MapReduce 3.12.0 and later, Kafka allows you to configure permissions with Ranger.

## Integrate Ranger into Kafka {#section_kk2_fxb_z2b .section}

To integrate Ranger into Kafka, complete the following steps:

-   Enable Kakfa Plugin
    1.  On the **Cluster Management** page, click **Ranger** in the service list to enter the Ranger Management page. Click **Operation** in the upper-right corner and select **Enable Kafka PLUGIN**.

        ![Cluster Management page](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/154770945110838_en-US.png)

    2.  You can check the progress by clicking **View Operation History** in the upper-right corner of the page.

        ![View Operation History](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/154770945110839_en-US.png)

-   Restart Kafka broker****

    After enabling the Kafka plugin, you must restart the broker to make it take effect.

    1.  On the **Cluster Management**page, click the inverted triangle icon behind **RANGER** in the upper-left corner to switch to **Kafka**.
    2.  Click **Actions** in the upper-right corner of the page and select **RESTART Broker**.
    3.  You can check the progress by clicking **View Operation History** in the upper-right corner of the page.

        ![View Operation History](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/154770945110840_en-US.png)

-   Add Kafka service on the Ranger WebUI

    For more information about how to go to the **Ranger**WebUI, see [Ranger Introduction](reseller.en-US/User Guide/Component authorization/Ranger/Introduction to Ranger.md#).

    Add the Kafka service on the WebUI:

    ![Ranger WebUI](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/154770945110841_en-US.png)

    Configure the Kafka service:

    ![Configure the Kafka service](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/154770945210842_en-US.png)


## Configure permissions {#section_nkp_hyb_z2b .section}

After integrating Ranger into Kafka, you can set the relevant permissions.

**Note:** In a standard cluster, Ranger automatically generates the **all - topic** rule after the Kafka service is added. This rule indicates that there are no restrictions on permissions. All users can perform all actions. In this case, Ranger cannot identify permissions through the user.

Here, user\_test is used as an example to add the Publish permission:

![add the Publish permission](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17899/154770945210843_en-US.png)

After you add a policy, the permissions are granted to the test user. This user can then perform the write operation for test.

**Note:** The policy takes effect one minute later after it is added.

