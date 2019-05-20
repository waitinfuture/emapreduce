# Common parameters {#concept_frn_4st_hfb .concept}

A common request parameter refers to a request parameter that is used by each API.

## Common request parameters {#section_ibl_yst_hfb .section}

Common request parameters are listed in the following table.

|Name|Type|Required|Description|
|----|----|--------|-----------|
|Format|String|No|The format of the return value. Valid values: JSON and XML. Default value: XML.|
|Version|String|Yes|The version of the API in the date format of YYYY-MM-DD.|
|AccessKeyId|String|Yes|The Access Key ID that is issued by Alibaba Cloud for user access.|
|Signature|String|Yes|The signature result string. For more information, see [Sign signatures](reseller.en-US/API Reference/Description of calls/Sign signatures.md#).|
|SignatureMethod|string|Yes|The signature method. Currently, HMAC-SHA1 is supported.|
|Timestamp|String|Yes|The request timestamp. The format of the date follows the ISO8601 standard and uses UTC time. Format: YYYY-MM-DDThh:mm:ssZ. For example, 2013-08-15T12:00:00Z indicates 20:00:00, Aug 15, 2013, Beijing time.|
|SignatureVersion|String|Yes|The version of the signature algorithm. The current version is 1.0.|
|SignatureNonce|String|Yes|The unique random number. This is used to prevent replay attacks. For different requests, you must use different random numbers.|

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

All requests, successful or failed, are replied with a unique ID \(RequestId\).

-   Examples

    XML format

    ```
    <? xml version="1.0" encoding="UTF-8"? >
    <! --Result root node-->
    <operation name+response>
    <! ----returned request >
    <RequestId>4C467B38-3910-447D-87BC-AC049166F216</RequestId> 
    <! --Returns the result-->
    </operation name+response>
    ```

    JSON format:

    ```
    {
    "RequestId": "4C467B38-3910-447D-87BC-AC049166F216",
    /* Returned results*/
    ```


