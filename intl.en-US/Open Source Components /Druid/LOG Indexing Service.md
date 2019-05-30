# LOG Indexing Service {#concept_zt2_q5v_hhb .concept}

LOG Indexing Service is a Druid plug-in launched by EMR and is used to consume data from Log Service.

## Background {#section_vrr_wvv_hhb .section}

LOG Indexing Service consumes data in a similar way as Kafka Indexing Service and supports exactly-once semantics. LOG Indexing Service has advantages of both Log Service and Kafka Indexing Service.

-   Provides various convenient methods for collecting data to Log Service.
-   Eliminates the need for Kafka clusters, which shortens the path of the data flow.
-   Supports exactly-once semantics.
-   Retries failed jobs to ensure reliability for data consumption, and allows for cluster restarts and service updates without service interruption.

## Preparations {#section_zk1_dwv_hhb .section}

-   Make sure that you have activated LOG and have configured projects and logstores.
-   Prepare the following configuration items:
    -   The endpoint of LOG. Use the intranet endpoint.
    -   A pair of AccessKey ID and AccessKey Secret to access LOG.

## Use LOG Indexing Service {#section_h5r_nxv_hhb .section}

1.  Prepare the ingestion spec.

    LOG Indexing Service is similar to Kafka Indexing Service. For more information, see [Kafka Indexing Service](reseller.en-US/Open Source Components /Druid/Kafka Indexing Service.md#). The same data is indexed. The ingestion spec for the data source is as follows and is saved as metrics-sls.json.

    ```
    {
        "type": "sls",
        "dataSchema": {
            "dataSource": "metrics-sls",
            "parser": {
                "type": "string",
                "parseSpec": {
                    "timestampSpec": {
                        "column": "time",
                        "format": "auto"
                    },
                    "dimensionsSpec": {
                        "dimensions": ["url", "user"]
                    },
                    "format": "json"
                }
            },
            "granularitySpec": {
                "type": "uniform",
                "segmentGranularity": "hour",
                "queryGranularity": "none"
            },
            "metricsSpec": [{
                    "type": "count",
                    "name": "views"
                },
                {
                    "name": "latencyMs",
                    "type": "doubleSum",
                    "fieldName": "latencyMs"
                }
            ]
        },
        "ioConfig": {
            "project": <your_project>,
            "logstore": <your_logstore>,
            "consumerProperties": {
                "endpoint": "cn-hangzhou-intranet.log.aliyuncs.com", (In this example, the China (Hangzhou) region is used. Use the intranet endpoint.)
                accessKeyId: "<your-access-key-id>",
                "access-key-secret": <your_access_key_secret>,
                "logtail.collection-mode": "simple"/"other"
            },
            "taskCount": 1,
            "replicas": 1,
            "taskDuration": "PT1H"
        },
        "tuningConfig": {
            "type": "sls",
            "maxRowsInMemory": "100000"
        }
    }
    ```

    The ingestion specs of Kafka Indexing Service and LOG Indexing Service are similar. Note the following fields:

    -   type: sls
    -   dataSchema.parser.parseSpec.format: depends on ioConfig.consumerProperties.logtail.collection-mode \(log collection mode of Log Service\). If you select Simple Mode, enter the source file format. If you do not select Simple Mode, enter json.
    -   ioConfig.project: the project of which the logs you want to collect.
    -   ioConfig.logstore: the Logstore of which the logs you want to collect.
    -   ioConfig.consumerProperties.endpoint: the intranet endpoint of LOG. For example, the endpoint for the China \(Hangzhou\) region is cn-hangzhou-intranet.log.aliyuncs.com.
    -   ioConfig.consumerProperties.access-key-id: the AccessKey ID of the account.
    -   ioConfig.consumerProperties.access-key-secret: the AccessKey Secret of the account.
    -   ioConfig.consumerProperties.logtail.collection-mode: the log collection mode of Log Service. If you select Simple Mode, enter simple. Otherwise, enter other.
2.  Run the following command to add a LOG supervisor.

    ```
    curl --negotiate -u:druid -b ~/cookies -c ~/cookies -XPOST -H 'Content-Type: application/json' -d @metrics-sls.json http://emr-header-1.cluster-1234:18090/druid/indexer/v1/supervisor
    ```

    **Note:** The --negotiate, -u, -b, and -c options are required for secure Druid clusters.

3.  Import data to LOG
4.  Perform queries by using Druid.

