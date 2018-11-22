# EcsOrder {#concept_x1c_csb_kfb .concept}

|名称|类型|描述|
|--|--|--|
|InstanceType|String|ecs.s2.large \(2核4G\)，ecs.s3.large \(4核8G\)，ecs.m1.medium \(4核16G\)，ecs.m1.xlarge \(8核32G\)，ecs.c1.large \(8核16G\)，ecs.c2.large \(16核32G\)，ecs.c2.xlarge \(16核64G\)，ecs.n1.medium \(2核4G\)，ecs.n1.large \(4核8G\)，ecs.n1.xlarge \(8核16G\)，ecs.n1.3xlarge \(16核32G\)，ecs.n1.7xlarge \(32核64G\)，ecs.n2.large \(4核16G\)，ecs.n2.xlarge \(8核32G\)，ecs.n2.3xlarge \(16核64G\)，ecs.n2.7xlarge \(32核128G\)，ecs.sn1.large \(4核8G\)，ecs.sn1.xlarge \(8核16G\)，ecs.sn1.3xlarge \(16核32G\)，ecs.sn1.7xlarge \(32核64G\)，ecs.sn2.large \(4核16G\)，ecs.sn2.xlarge \(8核32G\)，ecs.sn2.3xlarge \(16核64G\)，ecs.sn2.7xlarge \(32核128G\)|
|NodeType|String|节点类型，MASTER或者CORE|
|DiskCapacity|Integer|单个磁盘磁盘容量，从40G 到 2000G|
|DiskCount|Integer|磁盘个数，最小值1，最大值4|
|NodeCount|Integer|master节点目前默认是1个，core节点必须大于等于1个|

