# EcsInstance {#concept_kfv_mqb_kfb .concept}

|名称|类型|描述|
|--|--|--|
|NodeType|Integer|节点类型，master或者core|
|InstanceType|String|ecs.s3.large（4核8G）, ecs.m1.medium（4核16G）, ecs.m1.xlarge（8核32G）, ecs.c1.large（8核16G）, ecs.c2.large（16核32G）, ecs.c2.xlarge（16核64G）, ecs.n1.large（4核8G）, ecs.n2.large（4核16G）, ecs.n1.xlarge（8核16G）, ecs.n2.xlarge（8核32G）, ecs.n1.3xlarge（16核32G）, ecs.n2.3xlarge（16核64G）|
|CpuCore|Integer|Cpu的核数|
|MemoryCapacity|Integer|内存容量|
|DiskType|String|CLOUD\(普通云盘）,CLOUD\_EFFICIENCY（高效云盘）,CLOUD\_SSD（SSD云盘）|
|DiskCapacity|Integer|磁盘容量，从40G 到 2000G|
|BandWidth|String|网络带宽|
|Nodes|Array<Node\>|节点的详细信息|

-   Node

    |名称|类型|描述|
    |--|--|--|
    |ZoneId|String|可用区Id|
    |InstanceId|String|实例Id|
    |Status|String|状态|
    |PubIp|String|公网Ip|
    |InnerIp|String|内网Ip|
    |DiskInfos|Array<DiskInfo\>|磁盘信息|

-   DiskInfo

    |名称|类型|描述|
    |--|--|--|
    |Device|String|设备路径|
    |DiskName|String|磁盘名称|
    |DiskId|String|磁盘Id|
    |Type|String|类型|
    |Size|String|容量|


