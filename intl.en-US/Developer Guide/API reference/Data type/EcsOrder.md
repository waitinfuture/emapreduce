# EcsOrder {#concept_x1c_csb_kfb .concept}

|Name|Type|Description|
|----|----|-----------|
|InstanceType|String|ecs.s2.large \(2-core, 4 GB\), ecs.s3.large \(4-core, 8 GB\), ecs.m1.medium \(4-core, 16 GB\), ecs.m1.xlarge \(8-core, 32 GB\), ecs.c1.large \(8-core, 32 GB\), ecs.c2.large \(12-core, 32 GB\), ecs.c2.xlarge \(16-core, 64 GB\), ecs.n1.medium \(2-core, 4 GB\), ecs.n1.large \(4-core, 8 GB\), ecs.n1.xlarge \(8-core, 16 GB\), ecs.n1.3xlarge \(16-core, 32 GB\), ecs.n1.7xlarge \(32-core, 64 GB\), ecs.n2.large \(4-core, 16 GB\), ecs.n2.xlarge \(8-core, 32 GB\), ecs.n2.3xlarge \(16-core, 64 GB\), ecs.n2.7xlarge \(32-core, 128 GB\), ecs.sn1.large \(4-core, 8 GB\), ecs.sn1.xlarge \(8-core, 16 GB\), ecs.sn1.3xlarge \(16-core, 32 GB\), ecs.sn1.7xlarge \(32-core, 64 GB\), ecs.sn2.large \(4-core, 16 GB\), ecs.sn2.xlarge \(8-core, 32 GB\), ecs.sn2.3xlarge \(16-core, 64 GB\), ecs.sn2.7xlarge \(32-core, 128 GB\)|
|NodeType|String|The type of the node. Valid values: MASTER and CORE.|
|DiskCapacity|Integer|The capacity of a single disk. The range is from 40 GB to 2,000 GB.|
|DiskCount|Integer|The number of disks. The range is from one to four \(an integer\).|
|NodeCount|Integer|Currently, the default number of the master node is one. The minimum number of core nodes is one.|

