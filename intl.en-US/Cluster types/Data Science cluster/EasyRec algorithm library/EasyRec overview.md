# EasyRec overview

This topic describes recommendation algorithm libraries developed by Alibaba Cloud, including DeepFM, DIN, and MultiTower. An E-MapReduce \(EMR\) Data Science cluster has a built-in EasyRec algorithm library for use.

An EMR Data Science cluster is created. For more information, see [Create a cluster](/intl.en-US/Cluster Management/Configure clusters/Create a cluster.md).

## Input data

Input data must be stored in a CSV file. Sample files:

-   Train: [dwd\_avazu\_ctr\_deepmodel\_train.csv](https://pai-vision-data-hz.oss-cn-zhangjiakou.aliyuncs.com/easy-rec/data/dwd_avazu_ctr_deepmodel_train.csv)
-   Test: [dwd\_avazu\_ctr\_deepmodel\_test.csv](https://pai-vision-data-hz.oss-cn-zhangjiakou.aliyuncs.com/easy-rec/data/dwd_avazu_ctr_deepmodel_test.csv)

CSV files do not require a file header. Input data columns are separated by commas \(,\).

```
1,10,1005,0,85f751fd,c4e18dd6,50e219e0,0e8e4642,b408d42a,09481d60,a99f214a,5deb445a,f4fffcd0,1,0,2098,32,5,238,0,56,0,5
```

## Copy data to HDFS directories

Sample directories:

-   `hadoop fs -mkdir -p hdfs:///user/easy_rec/data/`
-   `hadoop fs -put dwd_avazu_ctr_deepmodel_train.csv hdfs:///user/easy_rec/data/`
-   `hadoop fs -put dwd_avazu_ctr_deepmodel_test.csv hdfs:///user/easy_rec/data/`

## Training files

An example of configuration files is [dwd\_avazu\_ctr\_deepmodel.config](https://pai-vision-data-hz.oss-cn-zhangjiakou.aliyuncs.com/easy-rec/dwd_avazu_ctr_deepmodel.config). For more information, see [Configure files](#section_tmt_m1x_o7w).

Run the `el_submit` command to submit a training task. Syntax:

```
el_submit [-h] [-t APP_TYPE] [-a APP_NAME] [-m MODE] [-m_arg MODE_ARG]

[-interact INTERACT] [-x EXIT]

[-enable_tensorboard ENABLE_TENSORBOARD]

[-log_tensorboard LOG_TENSORBOARD] [-conf CONF] [-f FILES]

[-pn PS_NUM] [-pc PS_CPU] [-pm PS_MEMORY] [-wn WORKER_NUM]

[-wc WORKER_CPU] [-wg WORKER_GPU] [-wm WORKER_MEMORY]

[-wnpg WNPG] [-ppn PPN] [-c COMMAND [COMMAND ...]]
```

**Note:** For more information about parameters in the `el_submit` command, see [Parameters in the el\_submit command](#section_vnz_60g_vx0).

Job submission example:

```
# bash
el_submit -t tensorflow-ps -a easy_rec_train -f dwd_avazu_ctr_deepmodel.config -m local -pn 1 -pc 4 -pm 20000 -wn 3 -wc 6 -wm 20000 -c "python -m easy_rec.python.train_eval --pipeline_config_path dwd_avazu_ctr_deepmodel.config --continue_train"
```

## Evaluate files

Run the `el_submit` command to submit an evaluation task.

```
# bash
el_submit -t standalone -a easy_rec_eval -f dwd_avazu_ctr_deepmodel.config -m local -wn 1 -wc 6 -wm 20000 -c "python -m easy_rec.python.eval --pipeline_config_path dwd_avazu_ctr_deepmodel.config"
```

## Export files

-   Run the `el_submit` command to submit an export task.

    ```
    # bash
    el_submit -t standalone -a easy_rec_export -f dwd_avazu_ctr_deepmodel.config,export.py -m local -wn 1 -wc 1 -wm 5000 -c "python -m easy_rec.python.export --pipeline_config_path dwd_avazu_ctr_deepmodel.config --export_dir hdfs:///user/easy_rec/experiment/export"
    ```

    -   `export_dir`: the export model directory.
    -   `pipeline_config_path`: the path used to store an EasyRec configuration file.
    -   `checkpoint_path`: the path used to store a checkpoint. By default, this parameter is not specified. If you do not specify this parameter, the latest checkpoint in the directory specified by model\_dir is used.
-   View export results.

    ```
    # bash
    hadoop fs -ls hdfs:///user/easy_rec/experiment/export
    ```


## Configure files

-   Data preparation
    -   train\_input\_path: hdfs:///user/easy\_rec/data/dwd\_avazu\_ctr\_deepmodel\_train.csv
    -   eval\_input\_path: hdfs:///user/easy\_rec/data/dwd\_avazu\_ctr\_deepmodel\_test.csv
    -   Model storage path: hdfs:///user/easy\_rec/experiment/
-   Data description

    ```
    # protobuf
    data_config {
      separator: ","
      input_fields: {
        input_name: "label"
        input_type: FLOAT
        default_val:""
      }
      input_fields: {
        input_name: "hour"
        input_type: STRING
        default_val:""
      }
      input_fields: {
        input_name: "c1"
        input_type: STRING
        default_val:""
      }
      ...
      input_fields: {
        input_name: "c20"
        input_type: STRING
        default_val:""
      }
      input_fields: {
        input_name: "c21"
        input_type: STRING
        default_val:""
      }
    
      label_fields: "label"
    
      batch_size: 1024
      num_epochs: 10000
      prefetch_size: 32
      input_type: CSVInput
    }
    ```

-   Configuration of feature-related parameters

    ```
    # protobuf
    feature_configs: {
      input_names: "hour"
      feature_type: IdFeature
      embedding_dim: 16
      hash_bucket_size: 50
    }
    feature_configs: {
      input_names: "c1"
      feature_type: IdFeature
      embedding_dim: 16
      hash_bucket_size: 10
    }
    ...
    feature_configs: {
      input_names: "c20"
      feature_type: IdFeature
      embedding_dim: 16
      hash_bucket_size: 500
    }
    feature_configs: {
      input_names: "c21"
      feature_type: IdFeature
      embedding_dim: 16
      hash_bucket_size: 500
    }
    ```

-   Configuration of training-related parameters

    ```
    # protobuf
    train_config {
      # Print a row of log information every 200 rounds of training.
      log_step_count_steps: 200
      # Optimizer-related parameters
      optimizer_config: {
        adam_optimizer: {
          learning_rate: {
            exponential_decay_learning_rate {
              initial_learning_rate: 0.0001
              decay_steps: 100000
              decay_factor: 0.5
              min_learning_rate: 0.0000001
            }
          }
        }
        use_moving_average: false
      }
      # Use SyncReplicasOptimizer for distributed training in synchronous mode.
      sync_replicas: true
    }
    ```

-   Configuration of evaluation-related parameters

    ```
    # protobuf
    eval_config {
      metrics_set: {
        # The metric is auc.
        auc {}
      }
    }
    ```

-   Configuration of model-related parameters

    ```
    # protobuf
    model_config:{
      model_class: "MultiTower"
      feature_groups: {
        group_name: "item"
        feature_names: "c1"
        feature_names: "banner_pos"
        feature_names: "site_id"
        feature_names: "site_domain"
        feature_names: "site_category"
        feature_names: "app_id"
        feature_names: "app_domain"
        feature_names: "app_category"
        wide_deep:DEEP
      }
      feature_groups: {
        group_name: "user"
        feature_names: "device_id"
        feature_names: "device_ip"
        feature_names: "device_model"
        feature_names: "device_type"
        feature_names: "device_conn_type"
        wide_deep:DEEP
      }
      feature_groups: {
        group_name: "user_item"
        feature_names: "hour"
        feature_names: "c14"
        feature_names: "c15"
        feature_names: "c16"
        feature_names: "c17"
        feature_names: "c18"
        feature_names: "c19"
        feature_names: "c20"
        feature_names: "c21"
        wide_deep:DEEP
      }
    
      multi_tower {
        towers {
          input: "item"
          dnn {
            hidden_units: [384, 320, 256, 192, 128]
          }
        }
    
        towers {
          input: "user"
          dnn {
            hidden_units: [384, 320, 256, 192, 128]
          }
        }
    
        towers {
          input: "user_item"
          dnn {
            hidden_units: [384, 320, 256, 192, 128]
          }
        }
    
        final_dnn {
          hidden_units: [256, 192, 128, 64]
        }
        l2_regularization: 0.0
      }
      embedding_regularization: 0.0
    }
    ```


## Parameters in the el\_submit command

|Parameter|Description|
|---------|-----------|
|`-t APP_TYPE`|The type of the task that you want to submit. Valid values: -   Tensorflow-ps: the PS mode of native TensorFlow. In this mode, a parameter server is used for communication.
-   Tensorflow-mpi: In this mode, Horovod is used for communication.
-   Standalone: You can assign tasks to an instance in the YARN cluster for execution. |
|`–a APP_NAME`|The name of the job.|
|`–m MODE`|The runtime environment. Valid values:-   Local: the Python runtime environment on an emr-worker node. If you want to use third-party Python packages, you must manually install the packages on all nodes.
-   Docker: the Docker runtime environment on an emr-worker node. TensorFlow runs in Docker containers.
-   Virtual-env: the Python environment that you uploaded. In this environment, you can install a Python library that is different from those on the emr-worker nodes. |
|`-m_arg MODE_ARG`|A supplemental parameter.Specify this parameter based on the value of `–m MODE`. If the value is Docker, set this parameter to the name of the Docker image. If the value is Virtual-env, set this parameter to the name of a Python environment package. |
|`–x Exit`|After all emr-worker nodes have completed training, parameter servers automatically exit.|
|`-enable_tensorboard`|Specifies whether to enable TensorBoard when TensorFlow starts a training task.|
|`-log_tensorboard`|The location of TensorBoard logs in HDFS. If TensorBoard is enabled when TensorFlow starts a training task, this parameter is required.|
|`-conf CONF Hadoop conf`|The location of the Hadoop configuration.|
|`–f FILES`|All dependent files and folders, including an executable script, for TensorFlow to run. If Virtual-env files are executed, you can place all dependent files in one folder. The script then automatically uploads the files into HDFS based on the folder hierarchy.|
|`-pn TensorFlow`|The number of parameter servers to start.|
|`-pc`|The number of vCPUs that each parameter server requests.|
|`-pm`|The memory size that each parameter server requests.|
|`-wn TensorFlow`|The number of emr-worker nodes to start.|
|`-wc`|The number of vCPUs that each emr-worker node requests.|
|`-wg`|The number of vGPUs that each emr-worker node requests.|
|`-wm`|The memory resources that each emr-worker node requests.|
|`-c COMMAND`|The command to run, such as `pythoncensus.py`.|
|`-wnpg`|The number of emr-worker nodes that simultaneously use a vGPU. You can specify this parameter when -t APP\_TYPE is set to Tensorflow-ps.|
|`-ppn`|The number of emr-worker nodes that simultaneously use a vGPU. You can specify this parameter when -t APP\_TYPE is set to Tensorflow-mpi.|

## Related operations

-   [Use Excel templates to generate configuration files]()
-   [Use a feature configuration method to generate configuration files]()

