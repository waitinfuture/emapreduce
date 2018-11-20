# ResizeCluster {#concept_klf_nfb_kfb .concept}

You can call this operation to resize a cluster.

## Request parameters {#section_h3c_pfb_kfb .section}

|Field|Type|Required|Default value|Description|
|-----|----|--------|-------------|-----------|
|ClusterId|String|Yes|None|The ID of the cluster.|
|RegionId|String|Yes|None| |
|NewMasterInstances|Integer|Yes|None|The number of master nodes after resizing.|
|NewCoreInstances|Integer|Yes|None|The number of core nodes after resizing.|
|NewTaskInstances|Integer|Yes|None|A reserved word. Set the value to 0.|

## Response parameters {#section_ewt_qfb_kfb .section}

Common response parameters

## Examples {#section_pxm_rfb_kfb .section}

-   Sample requests

    ```
    https://emr.aliyuncs.com/?Action=ResizeCluster
    &NewMasterInstances=1
    &NewCoreInstances=3
    &NewTaskInstances=0
    &ClusterId=500003112
    &amp;RegionId=cn-hangzhou
    &Common request parameters
    ```

-   Sample responses

    JSON format

    ```
    {
        "RequestId": "34B08619-2636-49F9-AB4E-CD8D347B1E07"
    }
    ```


