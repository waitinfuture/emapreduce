# Request structure {#concept_cjl_1rt_hfb .concept}

## Endpoint {#section_uhk_lst_hfb .section}

The API service access address is as follows:

|Region|Endpoint|
|------|--------|
|cn-hangzhou: China East 1 \(Hangzhou\)|emr.aliyuncs.com|
|cn-beijing: China North 2 \(Beijing\)|emr.aliyuncs.com|
|cn-shanghai: China East 2 \(Shanghai\)|emr.aliyuncs.com|
|cn-shenzhen: China South 1 \(Shenzhen\)|emr.aliyuncs.com|
|ap-southeast-1: Asia Pacific SE 1 \(Singapore\)|emr.aliyuncs.com|
|us-west-1: US West 1 \(Silicon Valley\)|emr.aliyuncs.com|
|others: other regions|emr.\[ RegionId\].aliyuncs.com|

## Protocols {#section_vhk_lst_hfb .section}

We recommend that you send requests using the HTTPS channel for enhanced security.

## Request methods {#section_whk_lst_hfb .section}

You can send requests using the HTTP GET method. In an HTTP GET request, parameters are included as part of the URL.

## Request parameters {#section_xhk_lst_hfb .section}

Each request must specify the operation to be executed, namely, the action parameter \(for example, CreateDatabase\), and each operation must contain the common request parameters and the specific request parameters of the specified operation.

## Encoding {#section_yhk_lst_hfb .section}

Both requests and responses use UTF-8 for encoding.

