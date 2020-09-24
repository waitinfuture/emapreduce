# UntagResources

You can call this operation to unbind tags from specified ECS resource list. After a tag is unbound, the tag is automatically deleted if it is not bound to any other resources.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=Emr&api=UntagResources&type=RPC&version=2016-04-08)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|No|UntagResources|The operation that you want to perform. Set the value to UntagResources. |
|RegionId|String|Yes|cn-hangzhou|The ID of the region where your cluster resides. |
|ResourceId.N|RepeatList|Yes|C- 3652B95F65967 deployment environment \*|The ID of the EMR cluster. Valid values of N: 1 to 50. |
|ResourceType|String|Yes|cluster|The type of the resource. Select cluster from the EMR cluster drop-down list. |
|TagKey.N|RepeatList|No|DevNianmin|The tag key of the EMR cluster. Valid values of N: 1 to 20. |
|All|Boolean|No|false|Specifies whether to unbind all tags from the resource. This parameter is valid only when the TagKey.N parameter is not specified in the request. Valid values:

 -   **true**
-   **false**

 Default value: **false**. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|C46FF5A8-C5F0-4024-8262-B16B639225A0|The ID of the request. |

## Examples

Sample requests:

```
http(s)://[Endpoint]/? Action=UntagResources
&RegionId=cn-hangzhou
&ResourceId.1=C-3652B95F65967 environment variables *
&ResourceType=cluster
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RequestId>C46FF5A8-C5F0-4024-8262-B16B639225A0</RequestId>
```

`JSON` format

```
{"RequestId":"C46FF5A8-C5F0-4024-8262-B16B639225A0"}
```

## Error codes

|HTTP status code|Error code|Error message|Description|
|----------------|----------|-------------|-----------|
|500|InternalError|The request processing has failed due to some unknown error.|The error message returned because the request processing has failed due to an internal error. Submit a ticket.|

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/Emr).

