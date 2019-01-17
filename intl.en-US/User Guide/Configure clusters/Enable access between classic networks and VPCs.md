# Enable access between classic networks and VPCs {#concept_e4h_gp3_y2b .concept}

This section describes how to enable inter-access between ECS on classic networks and E-MapReduce clusters on VPC networks.

## ClassicLink {#section_xs1_jp3_y2b .section}

Alibaba Cloud currently provides two types of cloud network: classic and VPC. While some users still use classic networks, E-MapReduce clusters use VPCs.

To grant access between ECS on a classic network and an E-MapReduce cluster on a VPC network, Alibaba Cloud launches [ClassicLink](https://partners-intl.aliyun.com/help/doc-detail/65412.htm). Follow these steps:

1.  Create a vSwitch according to the CIDR block specified in ClassicLink.
2.  To deploy a cluster that you have created, use the vSwitch of the CIDR block.
3.  Connect the corresponding classic network node to the VPC in the ECS console.
4.  Set the security group rules.

