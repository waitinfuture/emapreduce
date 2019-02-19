# Error code list {#concept_w1b_fbc_pfb .concept}

## Common error codes {#section_gh3_gbc_pfb .section}

|Error code|Error message|
|----------|-------------|
|4001|The error message returned when the request parameter is invalid, for example, the parameter is missing or the format of the parameter is invalid.|
|4005|The error message returned when you are not authorized to access resources of other users.|
|4006|The error message returned when the cluster is in the Abnormal status and the job cannot be submitted. Check whether the cluster associated with the execution plan has been released.|
|4007|The error message returned when the name of the security group is empty.|
|4009|The error message returned when your account has an overdue payment or is suspended. Check the status of your account.|
|4011|The error message returned when the cluster is in the Abnormal status and cannot be scheduled. Check whether the cluster associated with the execution plan has been released.|
|5012|The error message returned when the number of security groups you can create has exceeded the upper limit. Go to the [security group page](https://ecs.console.aliyun.com/#/securityGroup/region/cn-hangzhou) and delete security groups that are not in use.|
|5038|The error message returned when the job is in a running or pending execution plan and cannot be modified. You can modify the job only after the associated execution plan has been successfully executed. You can clone the job, then modify and use the cloned job.|
|5039|The error message returned when you fail to lock the cluster role. You must have certain permissions to use EMR. For [role authorization](../../../../../reseller.en-US/User Guide/Role authorization.md#), click [here](https://ram.console.aliyun.com/?spm=5176.2020520145.0.0.BRBzdk#/role/authorize?request=%7B%22Requests%22:%20%7B%22request1%22:%20%7B%22RoleName%22:%20%22AliyunEMRDefaultRole%22,%20%22TemplateId%22:%20%22DefaultRole%22%7D%7D,%20%22ReturnUrl%22:%20%22http:%2F%2Femr.console.aliyun.com%2F%22,%20%22Service%22:%20%22EMR%22%7D) to create cluster roles.|
|5050|The error message returned when accessing the database. Try again later.|
|6002|The error message returned when cluster updating failure occurs.|
|8002|The error message returned when you are not authorized to perform the specified operation. Click [RAM](https://ram.console.aliyun.com/#/overview) to apply for authorization.|
|8003|The error message returned when you are not authorized to perform the PassRole operation. Click [RAM](https://ram.console.aliyun.com/#/overview) to apply for authorization.|
|9006|The error message returned when the ID of the cluster does not exist. You need to verify the ID.|
|9007|The error message returned when the password used to log on to the master node is invalid. The password must be 8-30 characters in length and can contain uppercase letters, lowercase letters, and numbers.|

## ECS-related errors {#section_hhm_3bc_pfb .section}

|Error message|Description|
|-------------|-----------|
|The specified InstanceType is not authorized for use.|You have not applied for the specified types of instances that are used to create clusters. You can [apply for high-configuration instances](https://workorder.console.aliyun.com/console.htm#/ticket/add?productId=12&commonQuestionId=113) on the ECS buy page.|
|No zone or cluster resources are available.|No ECS resources are available in this zone.|

