# TensorFlow

TensorFlow 1.15.0 of Python 3.0 is a built-in component of EMR Data Science clusters. You can use this component without any additional configurations. On the master node of a Data Science cluster, you can purchase only vCPU resources to compute TensorFlow jobs. On a core node of a Data Science cluster, you can purchase vCPU or vGPU resources to compute TensorFlow jobs.

## View the TensorFlow version

1.  Log on to the master node of your cluster in SSH mode. For more information, see [Connect to the master node of an EMR cluster in SSH mode](/intl.en-US/Cluster Management/Configure clusters/Connect to a cluster/Connect to the master node of an EMR cluster in SSH mode.md).

2.  Run the pip3 list command to view the TensorFlow version.

    ![list](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3650693061/p127294.png)


## Sample code

You can use the sample code in this section to test whether TensorFlow can be used.

1.  Log on to the master node of your cluster in SSH mode. For more information, see [Connect to the master node of an EMR cluster in SSH mode](/intl.en-US/Cluster Management/Configure clusters/Connect to a cluster/Connect to the master node of an EMR cluster in SSH mode.md).

2.  Create a file named file.csv.

    ```
    6.1101,17.592
    5.5277,9.1302
    8.5186,13.662
    7.0032,11.854
    5.8598,6.8233
    8.3829,11.886
    7.4764,4.3483
    ```

3.  Create a test script.

    ```
    import tensorflow as tf
    import os
    import csv
    # Save data as a CSV file.
    file_name_string="file.csv"
    filename_queue = tf.train.string_input_producer([file_name_string])
    # Enter one line at a time.
    reader = tf.TextLineReader()
    key,value = reader.read(filename_queue)
    record_defaults = [[1.0], [1.0]] # The data type determines the type of the data to be read. The data must be presented as a list.
    col1, col2 = tf.decode_csv(value, record_defaults=record_defaults) # All the parsed attributes are tensors of rank 0 (scalars).
    with tf.Session() as sess:
        # Thread coordinator.
        coord = tf.train.Coordinator()
        # Start threads.
        threads = tf.train.start_queue_runners(coord=coord)
        is_second_read=0
        line1_name='%s:1' % file_name_string
        print(line1_name)
        for i in range(0,10):
           #x_ indicates the first data record, y_ indicates the second data record, and line_key indicates the number of the line that is being read.
            x_, y_,line_key = sess.run([col1, col2,key])
           # If the second value of the current line_key loop is equal to the key of the first line (line1_name), the reading is completed, and the loop ends.
            if is_second_read==0 and line_key==line1_name:
                is_second_read=1
            elif is_second_read==1 and line_key==line1_name:
                break
            print (x_,y_,line_key)
        # After the loop ends, close all threads.
        coord.request_stop()
        coord.join(threads)
        sess.close()
    ```

    If the following information is returned, TensorFlow can be used:

    ![test_tensorflow](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/3650693061/p127323.png)


