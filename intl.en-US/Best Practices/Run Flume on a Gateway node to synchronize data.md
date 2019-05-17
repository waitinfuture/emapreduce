# Run Flume on a Gateway node to synchronize data {#concept_fwf_jtm_4gb .concept}

This topic describes how to run Flume on a Gateway node to synchronize data based on Alibaba Cloud E-MapReduce \(EMR\) V3.17.0 and later versions.

## Background {#section_o5c_ntm_4gb .section}

EMR has supported Apache Flume since V3.16.0 and has supported default monitoring since V3.17.0.

-   Basic data flows

    Running Flume on Gateway nodes avoids the impact on EMR Hadoop clusters. Basic data flows that are streamed through Flume agents installed on Gateway nodes are shown in the following figure.

    ![Basic data flows](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120368/155807476838178_en-US.png)


## Prepare the environment {#section_mjt_c5n_4gb .section}

The test is performed using EMR that is deployed in the China East 1 \(Hangzhou\) region. The version of EMR is V3.17.0. The components required for this test are as follows.

-   Flume: 1.8.0

You can use EMR to automatically create a Hadoop cluster. For more information, see [Create a cluster](../../../../reseller.en-US/Quick Start/Step 3 : Create a cluster.md#).

-   Click Create Cluster, click Flume for the cluster type, and select Flume from **Optional Services**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120368/155807476838191_en-US.png)

-   Create a Gateway node and associate it to the Hadoop cluster created in the previous step.

## Procedure {#section_sbj_bzn_4gb .section}

-   Run Flume

    -   The default path of Flume configuration files is /etc/ecm/flume-conf. See [How to use Flume](../../../../reseller.en-US/Open Source Components /Flume/How to use Flume.md#) for modifying the configuration file flume.properties for Flume agents. After the modification, use the following command to run a Flume agent.

        ```
        nohup flume-ng agent -n a1 -f flume.properties &
        ```

    -   You can use the `-c` flag or the `--conf` flag to replace the default configuration file with a custom file. For example:

        ```
        nohup flume-ng agent -n a1 -f flume.properties -c path-to-flume-conf &
        ```

    **Note:** For more information, see [How to use Flume](../../../../reseller.en-US/Open Source Components /Flume/How to use Flume.md#). You need to add the zookeeperQuorum configuration item to the flume.properties configuration file when the Flume agents installed on Gateways use sinks to write data to HBase. For example:

    ```
    a1.sinks.k1.zookeeperQuorum=emr-header-1.cluster-46349:2181
    ```

    The hostname of the ZooKeeper cluster `emr-header-1.cluster-46349` is the value of the hbase.zookeeper.quorum configuration item in the /etc/ecm/hbase-conf/hbase-site.xml file.

-   View monitoring information

    Monitoring data of Flume agents is displayed in the cluster console by default. On the Clusters and Services page, click **FLUME** to jump to the cluster console as shown in the following figure.

    ![The FLUME service page](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/120368/155807476838198_en-US.png)

    **Note:** 

    Monitoring data is classified by the components \(sources, channels, or sinks\) of Flume agents. For example, CHANNEL.channel1 represents monitoring data of the channel1 channel component. Note: Avoid using the same component name when configuring different agents.

    Refer to the official Flume website for viewing monitoring data of Flume agents using Ganglia by creating proper configurations. After doing this, monitoring data of Flume agents will not be displayed in the console.

-   View logs

    By default, the log path of a Flume agent is /mnt/disk1/log/flume/$\{flume-agent-name\}/flume.log. You can modify the /etc/ecm/flume-conf/log4j.properties configuration file to change the log path. However, we recommend that you do not change the default log path.

    **Note:** A log path contains a Flume agent name. Provide a unique name for each agent to avoid logs of different agents to be stored in the same directory.


