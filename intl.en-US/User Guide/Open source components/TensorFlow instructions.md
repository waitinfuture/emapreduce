# TensorFlow instructions {#concept_lkf_243_kfb .concept}

[TensorFlow](https://www.tensorflow.org) is supported by E-MapReduce 3.13.0 and later versions. Users can add the TensorFlow component from available services in software configurations. When using TensorFlow in E-MapReduce to perform high-performance computing, you can allocate CPU and GPU resources through YARN.

## Preparations {#section_fcc_cp3_kfb .section}

-   On the software side, an E-MapReduce cluster installs TensorFlow and a TensorFlow on Yarn \(TOY\) toolkit.
-   On the hardware side, E-MapReduce supports computing using both CPU and GPU resources. If you need to use GPU computing, you can choose ECS instances from compute optimized type families with GPU, such as gn5 and gn6, for the core nodes and task nodes in the cluster. Compute optimized type families with GPU support heterogeneous computing. After determining the instance type, choose the CUDA toolkit and cuDNN versions as required.

## Submit TensorFlow jobs {#section_uqz_cq3_kfb .section}

Users can log on to the master node in the E-MapReduce cluster to submit TensorFlow jobs using the command line. For example:

```
el_submit [-h] [-t APP_TYPE] [-a APP_NAME] [-m MODE] [-m_arg MODE_ARG]

[-interact INTERACT] [-x EXIT]

[-enable_tensorboard ENABLE_TENSORBOARD]

[-log_tensorboard LOG_TENSORBOARD] [-conf CONF] [-f FILES]

[-pn PS_NUM] [-pc PS_CPU] [-pm PS_MEMORY] [-wn WORKER_NUM]

[-wc WORKER_CPU] [-wg WORKER_GPU] [-wm WORKER_MEMORY]

[-wnpg WNPG] [-ppn PPN] [-c COMMAND [COMMAND ...]]
```

Description of basic parameters:

-   The -t APP\_TYPE parameter specifies the type of task to be submitted. The supported types are tensorflow-ps, tensorflow-mpi, and standalone. They are used in conjunction with the following –m MODE parameter.
    -   tensorflow-ps: Uses a parameter server for the communication of data, which is the PS mode of native TensorFlow.
    -   tensorflow-mpi: Uses Horovod, an open source framework from UBER, which relies on message passing interface \(MPI\) primitives, for the communication of data.
    -   standalone: Users assign the tasks to one instance in the YARN cluster for execution. It is similar to a standalone execution.
-   The –a APP\_NAME parameter specifies the name of the submitted TensorFlow job. Users can name jobs as needed.
-   The –m MODE parameter specifies the runtime environment for submitted TensorFlow jobs. E-MapReduce supports the following environments: local, virtual-env, and docker.
    -   local: Uses Python runtime environments set up in the EMR worker nodes. If you want to use third-party Python packages, you need to install the packages on all the nodes manually.
    -   docker: Uses the Docker containers installed on the EMR worker nodes. Tensorflow runs in Docker containers.
    -   virtual-env: Uses isolated Python environments created by users. You can install Python libraries in Python environments. These libraries can be different from those installed in the environments that are set up in the worker nodes.
-   -m\_arg MODE\_ARG: Specifies the supplemental parameter for the –m MODE. If the runtime environment is docker, set the value to the Docker image name. If the runtime environment is virtual-env, set the value to the name of Python environment tar.gz file.
-   –x Exit: Users need to exit the parameter servers manually for some APIs of distributed TensorFlow. To exit parameter servers automatically when worker servers finish training their models, specify the -x option.
-   The -enable\_tensorboard parameter specifies whether to enable TensorBoard when TensorFlow starts training models.
-   The -log\_tensorboard parameter specifies the location of TensorBoard logs on HDFS. If TensorBoard is enabled when TensorFlow starts training models, then this parameter is required.
-   The -conf CONF parameter specifies the location of the Hadoop configuration. It is optional to set the value. The default EMR configuration is used.
-   The –f FILES parameter specifies all dependent files and folders for TensorFlow to run, including executable scripts. If virtual-env files that are executed in a virtual environment are specified, users can put all dependencies in one folder, the script automatically uploads the folders into HDFS according to the folder hierarchy.
-   The -pn TensorFlow parameter specifies the number of parameter servers to start.
-   The -pc parameter specifies the number of CPU cores that each parameter server requests.
-   The -pm parameter specifies the memory size that each parameter server requests.
-   The -wn parameter specifies the number of worker nodes started by TensorFlow.
-   The -wc parameter specifies the number of CPU cores that each worker requests.
-   The -wg parameter specifies the number of GPU cores that each worker requests.
-   The -wm parameter specifies the memory usage that each worker requests.
-   The -c COMMAND parameter specifies the command to run. For example, pythoncensus.py.

Advanced options. Users need to carefully use advanced options, which may cause failure of running jobs.

-   The -wnpg parameter specifies the number of workers that use a GPU simultaneously \(for tensorflow-ps\).
-   The -ppn parameter specifies the number of workers that use a GPU simultaneously \(for horovod \). The preceding options refer to multitasking on a single GPU. Thresholds should be set to avoid GPU running out of memory.

