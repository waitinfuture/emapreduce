# EcsInstance {#concept_kfv_mqb_kfb .concept}

|Name|Type|Description|
|----|----|-----------|
|NodeType|Integer|The type of the node. Valid values: master and core.|
|InstanceType|String|The type of the instance. The types include ecs.s3.large \(4-core 8G\), ecs.m1.medium \(4-core 16G\), ecs.m1.xlarge \(8-core 32G\), ecs.c1.large \(8-core 16G\), ecs.c2.large \(16-core 32G\), ecs.c2.xlarge \(16-core 64G\), ecs.n1.large \(4-core 8G\), ecs.n2.large \(4-core 16G\), ecs.n1.xlarge \(8-core 16G\), ecs.n2.xlarge \(8-core 32G\), ecs.n1.3xlarge \(16-core 32G\), and ecs.n2.3xlarge \(16-core 64G\).|
|CpuCore|Integer|The number of cores in the CPU.|
|MemoryCapacity|Integer|The memory capacity.|
|DiskType|String|The type of the disk. Valid values: CLOUD, CLOUD\_EFFICIENCY, and CLOUD\_SSD.|
|DiskCapacity|Integer|The capacity of a single disk. The range is from 40 GiB to 2,000 GiB.|
|BandWidth|String |The bandwidth of the network.|
|Nodes|Array<Node\>|The details of the nodes.|

-   Node

    |Name |Type|Description|
    |-----|----|-----------|
    |ZoneId |String|The ID of the available zone.|
    |InstanceId|String|The ID of the instance.|
    |Status|String|Status|
    |PubIp|String|The public network IP address.|
    |InnerIp|String |The private network IP address.|
    |DiskInfos|Array<DiskInfo\>|The information of the disks.|

-   DiskInfo

    |Name |Type|Required|
    |-----|----|--------|
    |Device |String|The path of the device.|
    |DiskName|String|The name of the disk.|
    |DiskId|String|The ID of the disk.|
    |Type|String|The type of the disk.|
    |Size|String |The capacity of the disk.|


