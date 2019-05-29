# Cluster management page {#concept_c41_cbc_pfb .concept}

You can purchase a one vCPU 2 GB ECS instance that runs the Ubuntu system and deploy the instance in a VPC network. You can use this instance as a management client to access the management pages.

The following table lists the endpoints of services in the cluster.

|Software|Service|Endpoint|
|--------|-------|--------|
|Hadoop| | |
| |yarn resourcemanager|k masternode1\_private\_ip:8088,masternode2\_private\_ip:8088|
| |jobhistory|masternode1\_private\_ip:19888|
| |timeline server|masternode1\_private\_ip:8188|
| |hdfs|masternode1\_private\_ip:50070,masternode2\_private\_ip:50070|
|Spark| | |
| |spark ui|masternode1\_private\_ip:4040|
| |history|masternode1\_private\_ip:18080|
|Tez| | |
| |tez-ui|masternode1\_private\_ip:8090/tez-ui2|
|Hue| | |
| |hue|masternode1\_private\_ip:8888|
|Zeppelin| | |
| |zeppelin|masternode1\_private\_ip:8080|
|Hbase| | |
| |hbase|masternode1\_private\_ip:16010|
|Presto| | |
| |presto|masternode1\_private\_ip:9090|
|Oozie| | |
| |oozie|masternode1\_private\_ip:11000|
|Ganglia| | |
| |ganglia|masternode1\_private\_ip:8085/ganglia|

