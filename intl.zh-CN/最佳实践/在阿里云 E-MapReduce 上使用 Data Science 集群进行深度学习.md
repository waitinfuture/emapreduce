# 在阿里云 E-MapReduce 上使用 Data Science 集群进行深度学习 {#concept_acg_4gr_bgb .concept}

Data Science 集群是阿里云 E-MapReduce 在 3.13.0 版本以后推出的专门用于机器学习，深度学习的新的机型。客户可以通过 Data Science 集群选用 GPU 或者 CPU 机型对数据进行训练，训练的数据可以存储在 HDFS 和 OSS 上，目前支持 TensorFlow 进行分布式训练，方便用户开发基于大数据存储，分布式调度的深度学习应用。

## 集群创建 {#section_ysx_ygr_bgb .section}

-   EMR Data Science 集群创建有以下要求：
    -   EMR 3.13.0 版本及以上
    -   集群类型选取 Data Science 类型
-   创建集群

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78541/154874755233944_zh-CN.png)

    在机型的选择上，Master 节点用户选取 CPU 机型即可，根据需要选择。Core 节点用户可以选取 GPU 机型。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78541/154874755233945_zh-CN.png)

    如果用户选择了 GPU 机型，EMR 会提供 Nvidia GPU 驱动以及对应 Cudnn 安装，用户选择对应的驱动进行安装，方便使用。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78541/154874755233947_zh-CN.png)

    至此，集群创建完毕，对应服务会被安装，驱动以及 Cudnn 将会被安装，同时在所有core 节点上也会安装 docker 服务，用于深度学习训练工具使用。


## 在Data Science集群上运行TensorFlow {#section_e3c_f3r_bgb .section}

-   TensorFlow

    TensorFlow 是谷歌开源的深度学习框架，用于机器学习任务和训练神经网络模型等深度学习。更多的关于 TensorFlow 的信息可以参见[TensorFlow](https://www.tensorflow.org/)官网。

-   TensorFlow on YARN

    TensorFlow on YARN 是 EMR 内核团队开发的基于 YARN 调度的分布式TensorFlow 运行框架，支持在 YARN 上运行 TensorFlow job 并运用 GPU 能力来进行训练。相应的使用说明请参考[TensorFlow on YARN使用说明](../../../../../intl.zh-CN/用户指南/开源组件介绍/TensorFlow使用说明.md#)。

-   使用 TensorFlow on YARN 进行深度学习

    目前 TensorFlow on YARN 支持使用高阶 API 进行训练，代码更加简洁。选取Wide And Deep 来进行训练，模型可以参见[github](https://github.com/tensorflow/models/tree/master/official/wide_deep#tensorflow-linear-model-tutorial)。下载的数据地址[链接](https://archive.ics.uci.edu/ml/machine-learning-databases/adult/)。训练需要的数据为 adult.data和adult.test 。用户按照单机版本通过 python 来写训练步骤：

    1.  首先定义好训练数据，上传训练数据和验证数据到 HDFS 上。

        将训练数据放到 HDFS 的/ml/ 目录下：

        ```
        hdfs dfs -put adut.data adult.test /ml/
        ```

    2.  在训练代码中指定训练数据路径：

        ```
        TRAIN_FILES = ['hdfs://emr-header-1.cluster-500157403:9000/ml/adult.data']
        EVAL_FILES = ['hdfs://emr-header-1.cluster-500157403:9000/ml/adult.test']
        
        ```

        其中 HDFS 的 Schema 需要用户根据自己的集群来设置，如果不是 HA 集群，请查找配置管理中 core-site.xml 中的 fs.defaultFS 属性。如果是 HA 集群，则默认为 emr-cluster。

    3.  定义特征列

        根据 Wide and Deep，分别定义对应特征：

        ```
        """Build a wide and deep model for predicting income category.
        """
        (gender, race, education, marital_status, relationship,
        workclass, occupation, native_country, age,
        education_num, capital_gain, capital_loss, hours_per_week) = INPUT_COLUMNS
        # Continuous columns can be converted to categorical via bucketization
        age_buckets = tf.feature_column.bucketized_column(
        age, boundaries=[18, 25, 30, 35, 40, 45, 50, 55, 60, 65])
        # Wide columns and deep columns.
        wide_columns = [
        # Interactions between different categorical features can also
        # be added as new virtual features.
        tf.feature_column.crossed_column(
        ['education', 'occupation'], hash_bucket_size=int(1e4)),
        tf.feature_column.crossed_column(
        [age_buckets, race, 'occupation'], hash_bucket_size=int(1e6)),
        tf.feature_column.crossed_column(
        ['native_country', 'occupation'], hash_bucket_size=int(1e4)),
        gender,
        native_country,
        education,
        occupation,
        workclass,
        marital_status,
        relationship,
        age_buckets,
        ]
        deep_columns = [
        # Use indicator columns for low dimensional vocabularies
        tf.feature_column.indicator_column(workclass),
        tf.feature_column.indicator_column(education),
        tf.feature_column.indicator_column(marital_status),
        tf.feature_column.indicator_column(gender),
        tf.feature_column.indicator_column(relationship),
        tf.feature_column.indicator_column(race),
        # Use embedding columns for high dimensional vocabularies
        tf.feature_column.embedding_column(
        native_country, dimension=embedding_size),
        tf.feature_column.embedding_column(occupation, dimension=embedding_size),
        age,
        education_num,
        capital_gain,
        capital_loss,
        hours_per_week,
        ]
        ```

    4.  定义 input\_fn

        input\_fn 方法用于用户获取训练数据。

        ```
        def input_fn(filenames,
        num_epochs=None,
        shuffle=True,
        skip_header_lines=0,
        batch_size=200):
        """Generates features and labels for training or evaluation.
        """
        dataset = tf.data.TextLineDataset(filenames).skip(skip_header_lines).map(parse_csv)
        if shuffle:
        dataset = dataset.shuffle(buffer_size=batch_size * 10)
        dataset = dataset.repeat(num_epochs)
        dataset = dataset.batch(batch_size)
        iterator = dataset.make_one_shot_iterator()
        features = iterator.get_next()
        return features, parse_label_column(features.pop(LABEL_COLUMN))
        train_input = lambda: input_fn(
        TRAIN_FILES,
        batch_size=40
        )
        # Don't shuffle evaluation data
        eval_input = lambda: input_fn(
        EVAL_FILES,
        batch_size=40,
        shuffle=False
        )
        ```

    5.  初始化 Estimator

        这里我们使用 TensorFlow 预定义的 Wide And Deep 模型来构建 Estimator。

        ```
        tf.estimator.DNNLinearCombinedClassifier(
        config=config,
        linear_feature_columns=wide_columns,
        dnn_feature_columns=deep_columns,
        dnn_hidden_units=hidden_units or [100, 70, 50, 25]
        )
        ```

    6.  模型训练

        ```
        train_spec = tf.estimator.TrainSpec(train_input,
        max_steps=1000
        )
        exporter = tf.estimator.FinalExporter('census',
        json_serving_input_fn)
        eval_spec = tf.estimator.EvalSpec(eval_input,
        steps=100,
        exporters=[exporter],
        name='census-eval'
        )
        tf.estimator.train_and_evaluate(estimator, train_spec, eval_spec)
        ```

        代码完成后用户可以向集群提交任务，推荐用户使用 standalone 模型先将训练任务发送到集群中进行一个单机训练，验证代码的正确性，当确认任务没有问题后，可以提交分布式版本，指定ps worker的资源进行训练。示例的提交命令如下：

        ```
        el_submit -t tensorflow-ps -a wide_and_deep -m local -x True -f ./ -pn 1 -pc 1 -pm 2000 -wn 1 -wc 1 -wg 1 -wm 2000 -c python census_single.
        ```

        任务提交后可以到 YARN 页面查看任务运行情况

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78541/154874755233950_zh-CN.png)

        单击 **ApplicationMaster** 链接后可以看到任务的运行情况和详细信息

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78541/154874755233951_zh-CN.png)

        单击 **log** 后能够跳转到 ps 或者 worker 上查看训练信息。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78541/154874755233952_zh-CN.png)

        本示例中，模型最后输出到 HDFS 的路径/census中，训练结束后可以在 HDFS 中找到输出的模型。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78541/154874755333953_zh-CN.png)


## 问题说明 {#section_btp_tnr_bgb .section}

如果报以下错误：

```
tensorflow.python.framework.errors_impl.InvalidArgumentError: Expect 15 fields but have 0 in record 0
[[Node: DecodeCSV = DecodeCSV[OUT_TYPE=[DT_INT32, DT_STRING, DT_INT32, DT_STRING, DT_INT32, ..., DT_INT32, DT_INT32, DT_INT32, DT_STRI
NG, DT_STRING], field_delim=",", na_value="", use_quote_delim=true](ExpandDims, DecodeCSV/record_defaults_0, DecodeCSV/record_defaults_1, D
ecodeCSV/record_defaults_2, DecodeCSV/record_defaults_3, DecodeCSV/record_defaults_4, DecodeCSV/record_defaults_5, DecodeCSV/record_default
s_6, DecodeCSV/record_defaults_7, DecodeCSV/record_defaults_8, DecodeCSV/record_defaults_9, DecodeCSV/record_defaults_10, DecodeCSV/record_
defaults_11, DecodeCSV/record_defaults_12, DecodeCSV/record_defaults_13, DecodeCSV/record_defaults_14)]]
[[Node: IteratorGetNext = IteratorGetNext[output_shapes=[[?,1], [?,1], [?,1], [?,1], [?,1], [?,1], [?,1], [?,1], [?,1], [?,1], [?,1],
[?,1], [?,1], [?,1]], output_types=[DT_INT32, DT_INT32, DT_INT32, DT_STRING, DT_INT32, DT_STRING, DT_INT32, DT_STRING, DT_STRING, DT_STRING
, DT_STRING, DT_STRING, DT_STRING, DT_STRING], _device="/job:chief/replica:0/task:0/device:CPU:0"](OneShotIterator)]]
[[Node: global_step/cond/pred_id_S615 = _HostRecv[client_terminated=false, recv_device="/job:ps/replica:0/task:0/device:CPU:0", send_d
evice="/job:chief/replica:0/task:0/device:GPU:0", send_device_incarnation=6104642431418663740, tensor_name="edge_602_global_step/cond/pred_
id", tensor_type=DT_BOOL, _device="/job:ps/replica:0/task:0/device:CPU:0"]()]]
2
```

检查一下 adult.data 和 adult.test 是否有空行存在。

