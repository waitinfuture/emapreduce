# TensorFlow

Data Science集群内置Python 3的Tensorflow 1.15.0版本，可以直接使用。其中MASTER节点只支持购买CPU资源计算TensorFlow作业，CORE节点支持购买CPU或GPU资源计算TensorFlow作业。

## 查看版本信息

1.  使用SSH方式登录到集群主节点，详情请参见[使用SSH连接主节点](/cn.zh-CN/集群管理/集群配置/连接集群/使用SSH连接主节点.md)。

2.  使用pip3 list命令，查看TensorFlow版本信息。

    ![list](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9349449951/p127294.png)


## 切换TensorFlow版本

1.  使用文件传输工具，上传install\_tf\_header.sh至Data Science集群Master节点的任意目录下。

    切换TensorFlow版本的脚本：[install\_tf\_header.sh](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/183066/cn_zh/1605252130588/install_tf_header.sh)

    **说明：** 本文上传至根目录。

2.  使用SSH方式登录到集群主节点，详情请参见[使用SSH连接主节点](/cn.zh-CN/集群管理/集群配置/连接集群/使用SSH连接主节点.md)。

3.  执行如下命令，切换TensorFlow版本。

    -   命令格式

        ```
        sh install_tf_header.sh x.x.x
        ```

        `x.x.x`为您需要切换的版本号。

    -   示例

        ```
        sh install_tf_header.sh 2.0.3
        ```

4.  使用pip3 list命令，查看TensorFlow版本号。

    ![version_](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2337525061/p182193.png)

    TensorFlow版本已经切换为2.0.3版本。


## 代码示例

您可以使用以下代码测试TensorFlow框架是否可以正常使用。

1.  使用SSH方式登录到集群主节点，详情请参见[使用SSH连接主节点](/cn.zh-CN/集群管理/集群配置/连接集群/使用SSH连接主节点.md)。

2.  创建file.csv文件。

    ```
    6.1101,17.592
    5.5277,9.1302
    8.5186,13.662
    7.0032,11.854
    5.8598,6.8233
    8.3829,11.886
    7.4764,4.3483
    ```

3.  创建测试脚本。

    ```
    import tensorflow as tf
    import os
    import csv
    #保存为csv格式的文件。
    file_name_string="file.csv"
    filename_queue = tf.train.string_input_producer([file_name_string])
    #每次输入一行。
    reader = tf.TextLineReader()
    key,value = reader.read(filename_queue)
    record_defaults = [[1.0], [1.0]] #这里的数据类型决定了读取的数据类型，而且必须是list形式。
    col1, col2 = tf.decode_csv(value, record_defaults=record_defaults) # 解析出的每一个属性都是rank为0的标量。
    with tf.Session() as sess:
        #线程协调器。
        coord = tf.train.Coordinator()
        #启动线程。
        threads = tf.train.start_queue_runners(coord=coord)
        is_second_read=0
        line1_name='%s:1' % file_name_string
        print(line1_name)
        for i in range(0,10):
           #x_第一个数据，y_第二个数据，line_key中保存当前读取的行号。
            x_, y_,line_key = sess.run([col1, col2,key])
           #如果当前line_key循环的第二次的值等于第一行的key(即line1_name)，则说明完成读取，跳出循环。
            if is_second_read==0 and line_key==line1_name:
                is_second_read=1
            elif is_second_read==1 and line_key==line1_name:
                break
            print (x_,y_,line_key)
        #循环结束后，关闭所有线程。
        coord.request_stop()
        coord.join(threads)
        sess.close()
    ```

    当回显信息类似如下所示时，表示可以正常使用。

    ![test_tensorflow](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/9349449951/p127323.png)


