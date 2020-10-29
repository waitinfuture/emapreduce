# AutoML

This topic describes how to use AutoML to optimize hyperparameters. Hyperparameters refer to the framework parameters of machine learning models, such as the learning rate of an algorithm and the decay rate of a learning rate.

-   A model training script and an AutoML configuration script are prepared.
-   Python 3.0 and SSH Secure File Transfer Client are installed on your computer.

## Procedure

In this topic, handwriting-recognition model training script digits\_model.py and AutoML configuration script test.py are used. For more information, see [digits\_model.py](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/33813/cn_zh/1596722874090/digits_model.py) and [test.py](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/33813/cn_zh/1596723249705/test.py).

1.  Add the parameters you want to optimize to `parser.add_argument` in the digits\_model.py file.

    ```
    # -*- coding: utf-8 -*-
    """local integration test case, used to test the correctness of an algorithm and recognize handwritten digits by using scikit-learn."""
    from __future__ import absolute_import
    from __future__ import division
    from __future__ import print_function
    from __future__ import unicode_literals
    
    import argparse
    import logging
    import os
    import numpy as np
    
    # Import datasets, classifiers and performance metrics
    from sklearn import datasets
    from sklearn import ensemble
    from sklearn import metrics
    import joblib
    logger = logging.getLogger()
    
    def run_sk_digits(inputs):
        """Recognize handwritten digits by using scikit-learn."""
        # Fake: copy
        if inputs.checkpoint_id >= 0:
            logger.info('copy model from {} to {}'.format(inputs.checkpoint_id,
                                                          inputs.trial_id))
    
        np.random.seed(0)
        # The digits dataset
        digits = datasets.load_digits()
    
        # To apply a classifier on this data, we need to flatten the image, to
        # turn the data in a (samples, feature) matrix:
        n_samples = len(digits.images)
        data = digits.images.reshape((n_samples, -1))
        n_samples = int(n_samples * inputs.ratio)
        data = data[:n_samples]
        digits.target = digits.target[:n_samples]
        offset = n_samples // 2
        x_train, y_train = data[:offset], digits.target[:offset]
        x_test, y_test = data[offset:], digits.target[offset:]
        params = {
            'n_estimators': inputs.n_estimators,
            'max_depth': inputs.max_depth,
            'min_samples_split': inputs.min_samples_split,
            'learning_rate': inputs.learning_rate,
            'loss': 'deviance'
        }
        clf = ensemble.GradientBoostingClassifier(**params)
        assert 0.0 < inputs.used_data_percent <= 1.0
        used_data = int(np.ceil(inputs.used_data_percent * len(x_train)))
        clf.fit(x_train, y_train)
    
        # TODO(weidan): We should allocate dirs (name) for each trial in the TrialManager.
        metric_file = inputs.metric_file
        dump_file = inputs.dump_file
        dirname = os.path.dirname(metric_file)
        if not os.path.exists(dirname):
            os.makedirs(dirname)
        dirname = os.path.dirname(dump_file)
        if not os.path.exists(dirname):
            os.makedirs(dirname)
    
        logger.info('saving to: {}, {}'.format(metric_file, dump_file))
        joblib.dump(clf, dump_file)
        #{"iteration": 0,"metric":"metrics,string","total_processed_sample":10000}
        with open(metric_file, 'w') as f:
            for _, y_pred in enumerate(clf.staged_predict(x_test)):
                result = metrics.classification_report(y_test, y_pred, digits=4)
                #avg / total       0.76      0.76      0.76       899
                result = result.rstrip().split('\n')[-1].split()[-2]
                f.write('%s\n' % result.strip())
    
    
    if __name__ == '__main__':
        parser = argparse.ArgumentParser()
        parser.add_argument('--n_estimators', type=int)
        parser.add_argument('--max_depth', type=int)
        parser.add_argument('--min_samples_split', type=int)
        parser.add_argument('--learning_rate', type=float)
        parser.add_argument('--trial_id', type=str)
        parser.add_argument('--used_data_percent', type=float, default=1.0)
        parser.add_argument('--checkpoint_id', type=int, default=-1)
        parser.add_argument('--dump_file', type=str)
        parser.add_argument('--metric_file', type=str)
        parser.add_argument('--ratio', type=float, default=1.0)
    
        args = parser.parse_args()
    
        logging.basicConfig(level=logging.INFO)
        run_sk_digits(args)
    ```

2.  Configure the following parameters in the test.py file:

    ```
    from pai.automl.hpo import *
    
    n = hyperparam.create(type='Categorical',
                          name='n', candidates=[10, 20, 30, 40, 50, 60, 70, 80, 90])
    s = hyperparam.create(type='Categorical',
                          name='s', candidates=[4, 3, 2])
    d = hyperparam.create(type='Categorical',
                          name='d', candidates=[2, 3, 4, 5])
    lr = hyperparam.create(type='Categorical',
                           name='lr', candidates=[0.01, 0.02, 0.04, 0.08, 0.16, 0.32])
    params = [n, s, d, lr]
    ```

3.  Configure a data output path in the test.py file.

    ```
    local_reader = reader.create(type='local_reader',   # local_reader is used to read files on your computer.
                                 location='./sk_log/{{trial.id}}/metric.log',
                                 parser_pattern='(\\d.\\d+)')
    print(local_reader)
    ```

4.  In the test.py file, configure the parameters you want to optimize and a metric file.

    ```
    cmd = ['python', './digits_model.py',
           '--n_estimators', "{{ trial.param.n }}",
           '--min_samples_split', "{{ trial.param.s }}",
           '--max_depth', "{{ trial.param.d }}",
           '--learning_rate', "{{ trial.param.lr }}",
           '--trial_id', "{{ trial.id }}",
           '--dump_file', "./sk_log/{{ trial.id }}/model.dump",
           '--metric_file', "./sk_log/{{ trial.id }}/metric.log"]
    bashtask = task.create(type='BashTask',
                           cmd=cmd,
                           metric_reader=local_reader)
    tasks = [bashtask]
    ```

5.  In the test.py file, configure the resources that are used to optimize parameters.

    ```
    tuner = AutoTuner(hyperparams=params,
                      task_list=tasks,
                      max_parallel=4,
                      max_trial_num=20,
                      mode='local',
                      user_id='your_cloud_id'
                      )
    ```

6.  Use SSH Secure File Transfer Client to upload the digits\_model.py and test.py files to the same directory.

    In this topic, the files are uploaded to the /usr/etc directory.

7.  Enter the directory.

    ```
    cd /usr/etc
    ```

8.  Run the test.py script.

    ```
    python3 test.py
    ```

    The following result is returned:

    ```
    2020-08-07,14:51:22.163 [INFO] results:
       params              results metadata
            d    lr   n  s               id
    0       2  0.32  50  2  0.9085       17
    1       2  0.32  50  4  0.9085       18
    2       4  0.16  80  2  0.8965        1
    3       4  0.08  70  4  0.8735        3
    4       4  0.16  30  3  0.8723        0
    5       2  0.32  10  2  0.8723       15
    6       4  0.04  80  4  0.8606        2
    7       5  0.01  50  4  0.8152       13
    8       5  0.01  90  2  0.8110       14
    9       5  0.04  10  4  0.8101       22
    10      5  0.01  40  4  0.8056       12
    11      2  0.01  90  2  0.8011       16
    12      2  0.01  90  3  0.8011       19
    13      5  0.01  40  2  0.7998        9
    14      5  0.01  50  2  0.7998       10
    15      2  0.01  60  2  0.7639       11
    16      2  0.01  50  2  0.7553        8
    17      3  0.01  10  2  0.7511        7
    18      2  0.01  10  2  0.7035        4
    19      2  0.01  10  4  0.7035        5
    20      2  0.01  10  3  0.7035        6
    ```


