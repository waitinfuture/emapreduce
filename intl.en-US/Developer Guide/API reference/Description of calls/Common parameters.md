# Common parameters {#concept_frn_4st_hfb .concept}

Common parameters

## Common request parameters {#section_ibl_yst_hfb .section}

Common request parameters refer to the request parameters that each API requires.

Parameters

|Name|Type|Required|Description|
|----|----|--------|-----------|
|Format|String|No|The format of the returned value. Valid values: JSON and XML. Default value: XML.|
|Version|String|Yes|The version of the API. Format: YYYY-MM-DD. The current version is 2016-04-08.|
|AccessKeyId|String|Yes|The Key ID provided by Alibaba Cloud for you to access services.|
|Signature|String|Yes|The result string of the signature. For more information about how to calculate a signature, see Sign signatures.|
|SignatureMethod|String|Yse|The mode of the signature. HMAC-SHA1 is supported currently.|
|Timestamp|String|Yes|The timestamp to request. The format of the date follows the ISO8601 standard and uses UTC time. Format: YYYY-MM-DDThh:mm:ssZ. For example, 2013-08-15T12:00:00Z indicates 20:00:00, Aug 15, 2013, Beijing time.|
|SignatureVersion|String|Yes|The algorithm version of the signature. The current version is 1.0.|
|SignatureNonce|String|Yes|The unique random number that is used to prevent network replay attacks. You must use different random numbers for the requests.|

Examples

```
https://emr.aliyuncs.com/
? Format=json
&Version=2016-04-08
&Signature=Pc5WB8gokVn0xfeu%2FZV%2BiNM1dgI%3D 
&SignatureMethod=HMAC-SHA1
&SignatureNonce=15215528852396
&SignatureVersion=1.0
&AccessKeyId=key-test
&OwnerId=12345678
&Timestamp=2014-10-10T12:00:00Z
```

## Common response parameters {#section_bvr_gtt_hfb .section}

Each time you make a call to an API, the system returns a unique identification code \(RequestId\), regardless whether the request is successful.

-   Examples
    -   Successful response examples

        Returned XML results include information about whether the request is successful and specific scenario information. Examples

        ```
        {
          "RequestId": "4C467B38-3910-447D-87BC-AC049166F216",
          /* Returned results
        */
        }
        ```

    -   Error response examples

        No result is returned after the API call fails. You can find the cause of error according to Error codes. When the HTTP request fails, a status code in the 4xx of 5xx format is returned. The response contains specific error codes and error information. The message body also contains a globally unique RequestId and the HostId of the site you request to access. If you cannot find the cause of the error, contact Alibaba Cloud After-Sales Support and provide the HostId and the RequestId for help.

        ```
        {
          "RequestId": "7463B73D-35CC-4D19-A010-6B8D65D242EF",
          "HostId": "emr.aliyuncs.com",
          "Code": "UnsupportedOperation",
          "Message": "The specified action is not supported."
        }
        ```

-   Common error codes

    For more information, see [ECS common error codes](../../SP_2/DNA0011860945/EN-US_TP_9850.dita#EcsApiResponse)


