# System structure {#concept_kdz_s5t_xgb .concept}

## Architecture {#section_iqr_cf2_z2b .section}

The following figure shows the architecture of Presto:

![Presto system composition](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17915/155255199710899_en-US.png)

Presto has a typical mobile/server architecture comprising a coordinator node and multiple worker nodes. Coordinator is responsible for the following:

-   Receiving and parsing your query requests, generating execution plans, and sending the execution plans to the worker nodes for execution.
-   Monitoring the running status of the worker nodes. Each worker node maintains a heartbeat connection with the coordinator node, reporting the node statuses.
-   Maintaining the metastore data

Worker nodes run the tasks assigned by the coordinator node, read data from external storage systems through connectors, process the data, and send the results to the coordinator node.

