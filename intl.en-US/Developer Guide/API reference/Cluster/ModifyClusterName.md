# ModifyClusterName {#concept_n1l_qcb_kfb .concept}

You can call this operation to modify the name of a cluster.

## Request parameters {#section_atg_5cb_kfb .section}

|Field|Type|Required|Default value|Description|
|-----|----|--------|-------------|-----------|
|Id|String|Yes|None|The ID of the cluster.|
|Name|String|Yes|None|The new name of the cluster. The same naming rules for creating the cluster are required. Length constraints: Minimum length of 1 character. Maximum length of 64 characters. Only Chinese characters, letters, numbers, hyphens \(-\), and underscores \(\_\) are allowed.|
|RegionId|String|Yes|None| |

## Response parameters {#section_sby_zcb_kfb .section}

Common response parameters

## Examples {#section_s4t_2db_kfb .section}

-   Sample requests

    ```
    https://emr.aliyuncs.com/?Action=ModifyClusterName
    &Id=C-13A570B821D4BAB3
    &Name=Cluster_Name
    &RegionId=cn-hangzhou
    &Common request parameters
    ```

-   Sample responses

    JSON format

    ```
    {
        "RequestId": "34B08619-2636-49F9-AB4E-CD8D347B1E07"
    }
    ```


