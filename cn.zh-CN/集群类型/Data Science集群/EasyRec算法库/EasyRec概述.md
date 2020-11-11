# EasyRec概述

本文为您介绍阿里云自研的推荐算法库，包含DeepFM、DIN和MultiTower等经典主流推荐算法。E-MapReduce的Data Science集群已经内置了EasyRec算法库，您可以直接使用。

已创建E-MapReduce的Data Science集群，并且勾选了EasyRec服务，详情请参见[创建集群](/cn.zh-CN/集群管理/集群配置/创建集群.md)。

## 输入数据

本文输入数据以CSV格式（非CSV格式请转换为CSV格式）的文件为例，示例数据如下：

-   train：[dwd\_avazu\_ctr\_deepmodel\_train.csv](https://pai-vision-data-hz.oss-cn-zhangjiakou.aliyuncs.com/easy-rec/data/dwd_avazu_ctr_deepmodel_train.csv)
-   test：[dwd\_avazu\_ctr\_deepmodel\_test.csv](https://pai-vision-data-hz.oss-cn-zhangjiakou.aliyuncs.com/easy-rec/data/dwd_avazu_ctr_deepmodel_test.csv)

CSV文件不需要有文件头，输入数据列之间使用英文逗号（,）分隔。

```
1,10,1005,0,85f751fd,c4e18dd6,50e219e0,0e8e4642,b408d42a,09481d60,a99f214a,5deb445a,f4fffcd0,1,0,2098,32,5,238,0,56,0,5
```

## 复制数据至HDFS

示例如下：

-   `hadoop fs -mkdir -p hdfs:///user/easy_rec/data/`
-   `hadoop fs -put dwd_avazu_ctr_deepmodel_train.csv hdfs:///user/easy_rec/data/`
-   `hadoop fs -put dwd_avazu_ctr_deepmodel_test.csv hdfs:///user/easy_rec/data/`

## 训练文件

配置文件示例：[dwd\_avazu\_ctr\_deepmodel.config](https://pai-vision-data-hz.oss-cn-zhangjiakou.aliyuncs.com/easy-rec/dwd_avazu_ctr_deepmodel.config)。详细配置请参见[配置文件](#section_tmt_m1x_o7w)。

使用`el_submit`提交训练任务。语法如下。

```
el_submit [-h] [-t APP_TYPE] [-a APP_NAME] [-m MODE] [-m_arg MODE_ARG]

[-interact INTERACT] [-x EXIT]

[-enable_tensorboard ENABLE_TENSORBOARD]

[-log_tensorboard LOG_TENSORBOARD] [-conf CONF] [-f FILES]

[-pn PS_NUM] [-pc PS_CPU] [-pm PS_MEMORY] [-wn WORKER_NUM]

[-wc WORKER_CPU] [-wg WORKER_GPU] [-wm WORKER_MEMORY]

[-wnpg WNPG] [-ppn PPN] [-c COMMAND [COMMAND ...]]
```

**说明：** `el_submit`相关的参数信息，请参见[el\_submit参数](#section_vnz_60g_vx0)。

作业提交示例如下。

```
# bash
el_submit -t tensorflow-ps -a easy_rec_train -f dwd_avazu_ctr_deepmodel.config -m local -pn 1 -pc 4 -pm 20000 -wn 3 -wc 6 -wm 20000 -c "python -m easy_rec.python.train_eval --pipeline_config_path dwd_avazu_ctr_deepmodel.config --continue_train"
```

## 评估文件

使用`el_submit`提交评估任务。

```
# bash
el_submit -t standalone -a easy_rec_eval -f dwd_avazu_ctr_deepmodel.config -m local -wn 1 -wc 6 -wm 20000 -c "python -m easy_rec.python.eval --pipeline_config_path dwd_avazu_ctr_deepmodel.config"
```

## 导出文件

-   使用`el_submit`提交导出任务。

    ```
    # bash
    el_submit -t standalone -a easy_rec_export -f dwd_avazu_ctr_deepmodel.config,export.py -m local -wn 1 -wc 1 -wm 5000 -c "python -m easy_rec.python.export --pipeline_config_path dwd_avazu_ctr_deepmodel.config --export_dir hdfs:///user/easy_rec/experiment/export"
    ```

    -   `export_dir`：导出模型目录。
    -   `pipeline_config_path`：EasyRec配置文件。
    -   `checkpoint_path`：指定checkpoint。默认不指定，不指定时则使用model\_dir下面最新的checkpoint。
-   查看导出结果。

    ```
    # bash
    hadoop fs -ls hdfs:///user/easy_rec/experiment/export
    ```


## 配置文件

-   数据准备
    -   训练表

        ```
        train_input_path: "hdfs://127.0.0.1:9000/user/easy_rec/data/dwd_avazu_ctr_deepmodel_train.csv"
        ```

    -   测试数据

        ```
        eval_input_path: "hdfs://127.0.0.1:9000/user/easy_rec/data/dwd_avazu_ctr_deepmodel_test.csv"
        ```

    -   模型保存路径

        ```
        model_dir: "hdfs://127.0.0.1:9000/user/easy_rec/experiment"
        ```

-   数据相关的描述

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

-   特征相关的参数

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

-   训练相关的参数

    ```
    # protobuf
    train_config {
      # 每200轮打印一行log。
      log_step_count_steps: 200
      # 优化器相关的参数。
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
      # 使用SyncReplicasOptimizer进行分布式训练（同步模式）。
      sync_replicas: true
    }
    ```

-   评估相关的参数

    ```
    # protobuf
    eval_config {
      metrics_set: {
        # metric为auc
        auc {}
      }
    }
    ```

-   模型相关的参数

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


## el\_submit参数

|参数|说明|
|--|--|
|`-t APP_TYPE`|提交的任务类型： -   Tensorflow-ps模式，采用Parameter Server方式通信，该方式为原生TensorFlow PS模式。
-   Tensorflow-mpi模式，采用Horovod进行通信。
-   Standalone模式，您可以调度任务到YARN集群中启动单机任务。 |
|`–a APP_NAME`|作业名称。|
|`–m MODE`|运行环境：-   Local，使用的是emr-worker节点上的Python运行环境。如果您需要使用第三方Python包，请手动在所有节点上安装第三方Python包。
-   Docker，使用的是emr-worker节点上的Docker运行环境，TensorFlow运行在Docker Container内。
-   Virtual-env，您上传的Python环境。您可以在Python环境中安装不同于emr-worker节点的Python库。 |
|`-m_arg MODE_ARG`|补充参数。此参数和`MODE`配合使用，如果运行环境是Docker，则设置为Docker镜像名称。如果运行环境是Virtual-env，则设置为Python环境文件名称。 |
|`–x Exit`|当所有emr-worker节点成功完成训练后，PS节点自动退出。|
|`-enable_tensorboard`|是否在启动训练任务时启动TensorBoard。|
|`-log_tensorboard`|指定HDFS中TensorBoard日志的位置。如果训练时启动了TensorBoard，则需要指定TensorBoard日志位置。|
|`-conf CONF Hadoop conf`|位置。|
|`–f FILES`|运行TensorFlow所有依赖的文件和文件夹，包含执行脚本。如果执行Virtual-env文件，您可以放置所有依赖到一个文件夹中，脚本会自动按照文件夹层次关系上传到HDFS中。|
|`-pn TensorFlow`|启动的参数服务器个数。|
|`-pc`|每个参数服务器申请的CPU核数。|
|`-pm`|每个参数服务器申请的内存大小。|
|`-wn TensorFlow`|启动的emr-worker节点个数。|
|`-wc`|每个emr-worker节点申请的CPU核数。|
|`-wg`|每个emr-worker节点申请的GPU核数。|
|`-wm`|每个emr-worker节点申请的内存个数。|
|`-c COMMAND`|执行的命令。例如`pythoncensus.py`。|
|`-wnpg`|每个GPU核上同时运行的Worker数量（针对tensorflow-ps模式）。|
|`-ppn`|每个GPU核上同时运行的Worker数量（针对horovod模式）。|

## 相关内容

-   [Excel特征配置](/cn.zh-CN/集群类型/Data Science集群/EasyRec算法库/Excel特征配置.md)
-   [FeatureConfig配置](/cn.zh-CN/集群类型/Data Science集群/EasyRec算法库/FeatureConfig配置.md)

