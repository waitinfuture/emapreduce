# Use EMR Data Science clusters for deep learning {#concept_acg_4gr_bgb .concept}

Data Science cluster is a new model available in E-MapReduce \(EMR\) 3.13.0 and later versions for machine learning and deep learning. You can use GPU or CPU models to perform data training through Data Science clusters. Training data can be stored on HDFS and OSS. EMR supports TensorFlow for distributed training on large amounts of data.

## Create cluster {#section_ysx_ygr_bgb .section}

-   Prerequisites for creating an EMR Data Science cluster:
    -   EMR 3.13.0 or later.
    -   Data Science as the cluster type.
-   Create a cluster

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78541/154752785233944_en-US.png)

    Select a CPU model for Master Instance Type, and select a CPU or GPU model for Core Instance Type.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78541/154752785233945_en-US.png)

    If you select a GPU model, EMR provides Nvidia GPU drivers and the corresponding Cudnn installation.

    After the cluster is created, the corresponding service, driver, and Cudnn are installed. The docker service is installed on all core nodes.


## Run TensorFlow on a Data Science cluster {#section_e3c_f3r_bgb .section}

-   TensorFlow

    TensorFlow is an open-srouce machine learning framework for deep learning of machine learning tasks and training neural models. For more information about TensorFlow, see [TensorFlow](https://www.tensorflow.org/).

-   TensorFlow on YARN

    TensorFlow on YARN developed by the EMR kernel team is a distributed TensorFlow framework based on YARN scheduling. It supports running TensorFlow jobs on YARN and using GPU for training. For information about how to use TensorFlow on YARN, see [TensorFlow instructions](../../../../../intl.en-US/User Guide/Open source components/TensorFlow .md#).

-   Use TensorFlow on YARN to perform deep learning

    TensorFlow on YARN can use high-level APIs for training with more concise codes. This topic takes the Wide and Deep model as an example for training. For information about more models, see [github](https://github.com/tensorflow/models/tree/master/official/wide_deep#tensorflow-linear-model-tutorial). Click [here](https://archive.ics.uci.edu/ml/machine-learning-databases/adult/) to download training data. Data for training is adult.data and adult.test. This example writes training steps in Python according to the stand-alone version.

    1.  Define training data, and then upload training data and validation data to HDFS.

        Put the training data to the /ml/ directory of hdfs:

        ```
        hdfs dfs -put adut.data adult.test /ml/
        ```

    2.  Specify the training data path in the training code:

        ```
        TRAIN_FILES = ['hdfs://emr-header-1.cluster-500157403:9000/ml/adult.data']
        EVAL_FILES = ['hdfs://emr-header-1.cluster-500157403:9000/ml/adult.test']
        
        ```

        HDFS schema is set according to your cluster. If the cluster is not a high availability cluster \(HA cluster\), you only need to check the fs.defaultFS property in core-site.xml. If the cluster is an HA cluster, by default, the HDFS schema is emr-cluster.

    3.  Define feature columns.

        Define the corresponding features according to the wide and deep model:

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

    4.  Define input\_fn.

        You can use input\_fn to obtain training data:

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

    5.  Initiate Estimator.

        The following example uses the pre-defined TensorFlow's wide and deep model to build the Estimator:

        ```
        tf.estimator.DNNLinearCombinedClassifier(
        config=config,
        linear_feature_columns=wide_columns,
        dnn_feature_columns=deep_columns,
        dnn_hidden_units=hidden_units or [100, 70, 50, 25]
        )
        ```

    6.  Train the models:

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

        After the codes are complete, you can submit a task to your Data Science cluster. We recommend that you send the task to the cluster to perform standalone training using a standalone model. After verifying the task without code errors, you can submit a distributed task and specify worker and ps resources for training. An example command for submitting a task is as follows:

        ```
        el_submit -t tensorflow-ps -a wide_and_deep -m local -x True -f ./ -pn 1 -pc 1 -pm 2000 -wn 1 -wc 1 -wg 1 -wm 2000 -c python census_single.
        ```

        The running status of the submitted task can be viewed in the YARN page.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78541/154752785233950_en-US.png)

        Click the **ApplicationMaster** link to view the running status and details of the task.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78541/154752785333951_en-US.png)

        Click **log** to go to ps or worker to view training information.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78541/154752785333952_en-US.png)

        In this example, after the training ends, you can find models in the HDFS path /census where models are generated.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78541/154752785333953_en-US.png)


## Question descriptions {#section_btp_tnr_bgb .section}

If the following errors display, check if there are empty lines in adult.data and adult.test.

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

