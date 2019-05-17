# Responses {#concept_b1h_gl5_cgb .concept}

Sample responses in this topic are formatted for your convenience to view. Line breaks and indentation are not included in actual responses.

Responses adopt the same format. A 2xx HTTP status code indicates that the API call succeeds. A 4xx or 5xx HTTP status code indicates that the API call fails. Formats of a response body include XML and JSON. You can specify the format by passing arguments when sending a request. The default format is XML.

## Successful response examples {#section_sqx_ll5_cgb .section}

JSON format

```
{
	"Data":"true",
	"RequestId":"2670BCFB-925D-4C3E-9994-8D12F7A9F538"
}
```

## Error response examples {#section_etw_ml5_cgb .section}

No result is returned when an API call fails. You can locate the cause of failure according to the error codes that the corresponding operation can return and the following common error codes. When an API call fails, the response comes with a 4xx or 5xx HTTP status code. The message body contains the error code and the error information. The message body also contains the globally unique request ID and the ID of the host that you request access to. If you cannot locate the cause of failure, contact the Alibaba Cloud customer service and provide the host ID and request ID.

JSON format

```
{
	"code":"FLOW_API_FAILED",
	"message":"project not exist",
	"requestId":"11BAFBD8-8509-4177-A26D-407505E73713",
	"successResponse":false
}
```

## Common error codes {#section_vtc_vl5_cgb .section}

