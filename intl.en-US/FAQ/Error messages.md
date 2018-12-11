# Error messages {#concept_h5r_fz1_pfb .concept}

## Error message: Pay-As-You-Go instances are not available in this region. {#section_nsk_gz1_pfb .section}

The error message returned when you cannot purchase Pay-As-You-Go ECS instances in the region that you want to create clusters. We recommend that you switch to another region to purchase instances.

## Error message: The request processing has failed due to an unknown error, exception or failure. {#section_osk_gz1_pfb .section}

This is an unknown error that occurs in the ECS management system. EMR is built on Alibaba Cloud Elastic Compute Service \(ECS\) and is also affected by this error. You can try later or submit a ticket to troubleshoot the issues.

## Error message: The Node Controller is temporarily unavailable {#section_psk_gz1_pfb .section}

EMR is built on ECS. The error message returned when the ECS management system has temporary issues. Try creating clusters later.

## Error message: No quota or zone is available. {#section_qsk_gz1_pfb .section}

The error message returned when there is no ECS quota available in the specified zone. You can manually switch to another zone or the system will automatically select a zone for you.

## Error message: The specified InstanceType is not authorized for use. {#section_rsk_gz1_pfb .section}

You need to apply to use Pay-As-You-Go high-configuration instances \(instances with more than eight cores\). Click [Here](https://workorder-intl.console.aliyun.com/#/ticket/createIndex) to apply. You can create high-configuration instances after your application is approved. Make sure that you apply for instances that are supported by EMR, including eight-core 16 GB, eight-core 32 GB, and 16-core 64 GB types.

