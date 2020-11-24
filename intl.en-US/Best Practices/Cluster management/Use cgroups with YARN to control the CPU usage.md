# Use cgroups with YARN to control the CPU usage

Control groups \(cgroups\) are a Linux kernel feature that controls and limits resource usage and isolates resources of a collection of processes. The resources include the CPU, memory, and disk I/O.

## Overview

YARN is integrated with the cgroups feature. This feature allows you to control the CPU usage of each container or all containers managed by the NodeManager.

## Enable the cgroups feature in YARN

**Note:** By default, the cgroups feature is disabled in the YARN component of an E-MapReduce \(EMR\) cluster. You can enable the feature as needed.

1.  Log on to the [Alibaba Cloud EMR console](https://emr.console.aliyun.com/).

2.  Go to the **YARN** page.
    1.  Click the **Cluster Management** tab.
    2.  On the **Cluster Management** page, find your cluster and click its ID.
    3.  In the left-side navigation pane, choose **Cluster Service** \> **YARN**.
3.  Enable the cgroups feature.
    1.  On the YARN page, choose **Actions** \> **Enabled CGroups** in the upper-right corner.
    2.  In the **Cluster Activities** dialog box, configure relevant parameters as required.
    3.  Click **OK**.
    4.  In the **Confirm** message, click **OK**.
4.  Restart NodeManager.
    1.  Choose **Actions** \> **Restart NodeManager** in the upper-right corner.
    2.  In the **Cluster Activities** dialog box, configure relevant parameters as required.
    3.  Click **OK**.
    4.  In the **Confirm** message, click **OK**.

## Modify parameters to control the CPU usage

After you enable the cgroups feature for an EMR cluster, you can modify relevant parameters in the YARN component to control the CPU usage.

1.  Log on to the [Alibaba Cloud EMR console](https://emr.console.aliyun.com/).

2.  Go to the **YARN** page.
    1.  Click the **Cluster Management** tab.
    2.  On the **Cluster Management** page, find your cluster and click its ID.
    3.  In the left-side navigation pane, choose **Cluster Service** \> **YARN**.
3.  Modify relevant parameters.
    1.  On the YARN page, click the **Configure** tab.
    2.  Modify the parameters described in the following table.

        |Parameter|Description|
        |---------|-----------|
        |**yarn.nodemanager.resource.percentage-physical-cpu-limit**|The percentage at which you want to limit the total CPU usage of all containers managed by NodeManager. Default value: 100. **Note:** The total CPU usage of all containers managed by NodeManager does not exceed the upper limit specified by the **yarn.nodemanager.resource.percentage-physical-cpu-limit** parameter. |
        |**yarn.nodemanager.linux-container-executor.cgroups.strict-resource-usage**|Specifies whether to set a hard limit on the CPU usage for each container. If no hard limit is set, containers can use idle CPU resources in addition to the allocated resources. The default value is false, which indicates that containers can use idle CPU resources.|


## Control the total CPU usage of all containers

You can modify the **yarn.nodemanager.resource.percentage-physical-cpu-limit** parameter to control the total CPU usage of all containers managed by NodeManager.

Prepare an EMR cluster that consists of two nodes deployed with NodeManager and one node deployed with ResourceManager. Each node has 4 cores and 16 GiB of memory. Set the yarn.nodemanager.resource.percentage-physical-cpu-limit parameter to control the CPU usage of the nodes deployed with NodeManager. After you set the parameter, run a Hadoop Pi job in the YARN component to check the CPU usage. In this example, the parameter is set to 10, 30, and 50, respectively.

**Note:** In the following figures, `%CPU` indicates the CPU usage of a process on a single core. `%Cpu(s)` indicates the CPU usage of all processes across all cores.

-   Set the parameter to 10.

    ![cpu_10](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6397593061/p69258.png)

    The preceding figure shows that the value of `%Cpu(s)` is 10.2, which is the sum of the `%CPU` values of all container processes of the test user. The total CPU usage is calculated by using the following formula: 7% + 5.3% + 5% + 4.7% + 4.7% + 4.3% + 4.3% + 4% + 2% = 41.3%. This indicates that all processes of the test user consume 0.413 cores, which accounts for about 10% of the four cores on a specific node.

-   Set the parameter to 30.

    ![cpu_30](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6397593061/p69260.png)

    The preceding figure shows that the value of `%Cpu(s)` is 32.5, which is the sum of the `%CPU` values of all container processes of the test user. The total CPU usage is calculated by using the following formula: 19% + 18.3% + 18.3% + 17% + 16.7% + 16.3% + 14.7% + 12% = 132.3%. This indicates that all processes of the test user consume 1.323 cores, which accounts for about 30% of the four cores on a specific node.

-   Set the parameter to 50.

    ![cpu_50](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6397593061/p69261.png)

    The preceding figure shows that the value of `%Cpu(s)` is 48.7, which is the sum of the `%CPU` values of all container processes of the test user. The total CPU usage is calculated by using the following formula: 65.1% + 60.1% + 43.5% + 20.3% + 3.7% + 2% = 194.7%. This indicates that all processes of the test user consume 1.947 cores, which accounts for about 50% of the four cores on a specific node.


## Control the CPU usage of each container

The total CPU usage of all containers managed by NodeManager does not exceed the upper limit specified by the **yarn.nodemanager.resource.percentage-physical-cpu-limit** parameter in the [Control the total CPU usage of all containers](#section_o63_1c4_m9w) section. NodeManager provides the share mode and the strict mode for you to manage and control the CPU usage of each container. You can set the **yarn.nodemanager.linux-container-executor.cgroups.strict-resource-usage** parameter to specify a mode.

-   Use share mode

    To use the share mode, set the **yarn.nodemanager.linux-container-executor.cgroups.strict-resource-usage** parameter to false. This is the default value of the parameter. In share mode, containers can use idle CPU resources in addition to the allocated resources.

    For example, set the **yarn.nodemanager.resource.percentage-physical-cpu-limit** parameter to 50 and the **yarn.nodemanager.linux-container-executor.cgroups.strict-resource-usage** parameter to false. The cluster has two nodes deployed with NodeManager and each node has four cores. All containers managed by NodeManager on a node can consume a maximum of 50% of the four cores, which is equal to two cores. The CPU resources allocated for each container are calculated by using the following formula: \(1 vCore/8 vCores\) × \(4 cores × 50%\) = 0.25 cores. In share mode, each container can consume idle CPU resources, which are a maximum of two cores.

    ![share](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6397593061/p69304.png)

    The preceding figure shows that the CPU resources consumed by the container processes of the test user vary largely, such as 0.65 cores, 0.61 cores, and 0.037 cores. This indicates that some containers consume much more CPU resources than the allocated 0.25 cores.

-   Use strict mode

    To use strict mode, set the **yarn.nodemanager.linux-container-executor.cgroups.strict-resource-usage** parameter to true. In strict mode, containers can consume only the allocated CPU resources, even if idle CPU resources are available.

    For example, set the **yarn.nodemanager.resource.percentage-physical-cpu-limit** parameter to 50 and the **yarn.nodemanager.linux-container-executor.cgroups.strict-resource-usage** parameter to true.

    ![strict](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6397593061/p69308.png)

    The cluster has two nodes deployed with NodeManager and each node has four cores. The CPU resources allocated for each container are calculated by using the following formula: \(1 vCore/8 vCores\) × \(4 cores × 50%\) = 0.25 cores. The preceding figure shows that the CPU resources consumed by the container processes of the test user are around the allocated 0.25 cores, such as 0.266 cores and 0.249 cores.


