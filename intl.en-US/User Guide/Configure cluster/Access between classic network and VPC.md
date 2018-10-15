# Access between classic network and VPC {#concept_e4h_gp3_y2b .concept}

Currently, Alibaba Cloud provides two types of cloud networks: classic network and VPC. Many users’ service systems are still using classic networks, while EMR clusters are using VPCs. This section describes how to enable inter-access between ECS on classic networks and EMR clusters on VPC networks.

## ClassicLink {#section_xs1_jp3_y2b .section}

To solve this problem, Alibaba Cloud launches the [ClassicLink Solution](https://www.alibabacloud.com/help/doc-detail/65412.htm?spm=a2c63.p38356.b99.24.6de11bd5fp3ffA).

To solve this problem, Alibaba Cloud launches the ClassLink Solution.

1.  Create a vSwitch according to the CIDR block specified in the ClassLink Solution.
2.  When creating a cluster, use the vSwitch for the CIDR block to deploy the cluster.
3.  Connect the corresponding classic network node to VPC in the ECS console.
4.  Set security group rules.

This is how an inter-access between ECS on a classic network and EMR cluster on a VPC network is realized.

