# ListJobInstanceWorkers {#concept_cv4_mpb_kfb .concept}

You can call this operation to query the running nodes of a job instance.

## Request parameters {#section_rbf_qpb_kfb .section}

|Field|Type|Required|Default value|Description|
|-----|----|--------|-------------|-----------|
|RegionId|String|Yes|None| |
|JobInstanceId|String|Yes|None|The ID of the job instance.|

## Response parameters {#section_r4g_spb_kfb .section}

|Field|Type|Description|
|-----|----|-----------|
|JobInstanceWorkers|Array<[JobInstanceWorker](EN-US_TP_18036.dita#concept_f2s_mtb_kfb)\>|The information about the running nodes of the job instance.|

## Examples {#section_e2b_5pb_kfb .section}

-   Sample requests

    ```
    https://emr.aliyuncs.com/?Action=ListJobInstanceWorkers
    &JobInstanceId=500006685
    &RegionId=cn-hangzhou
    &Common request parameters
    ```

-   Sample responses

    JSON format

    ```
    {
        "JobInstanceWorkers": {
            "JobInstanceWorkerInfo": [
                {
                    "ApplicationId": "application_1458791367888_0002",
                    "ContainerInfo": "container_1458791367888_0002_02_000001",
                    "InstanceInfo": "10.24.28.118"
                },
                {
                    "ApplicationId": "application_1458791367888_0002",
                    "ContainerInfo": "container_1458791367888_0002_01_000002",
                    "InstanceInfo": "10.24.28.11"
                },
                {
                    "ApplicationId": "application_1458791367888_0002",
                    "ContainerInfo": "container_1458791367888_0002_02_000002",
                    "InstanceInfo": "10.24.28.11"
                },
                {
                    "ApplicationId": "application_1458791367888_0002",
                    "ContainerInfo": "container_1458791367888_0002_01_000001",
                    "InstanceInfo": "10.24.28.118"
                }
            ]
        },
        "RequestId": "0C6F1167-4807-4503-B63F-8EDCB26E0BEE"
    }
    ```


