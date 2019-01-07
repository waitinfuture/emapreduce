# TensorFlow {#concept_lkf_243_kfb .concept}

[TensorFlow](https://www.tensorflow.org) is supported by E-MapReduce 3.13.0 and later. You can add TensorFlow from the available services in your software configurations. If you are using TensorFlow in E-MapReduce to perform high-performance computing, you can allocate CPU and GPU resources through YARN.

## Prerequisites {#section_fcc_cp3_kfb .section}

-   On the software side, an E-MapReduce cluster installs TensorFlow and a TensorFlow on YARN \(TOY\) toolkit.
-   On the hardware side, E-MapReduce supports computing using both CPU and GPU resources. If you need to use GPU computing, you can choose ECS instances from compute optimized families with GPU, such as gn5 and gn6, for the core and task nodes in the cluster. Compute optimized families with GPU support heterogeneous computing. After determining the instance type, choose the CUDA toolkit and cuDNN versions as required.

## Submit TensorFlow jobs {#section_uqz_cq3_kfb .section}

You can log on to the master node in the E-MapReduce cluster to submit TensorFlow jobs using the command line. For example:

```
el_submit [-h] [-t APP_TYPE] [-a APP_NAME] [-m MODE] [-m_arg MODE_ARG]

[-interact INTERACT] [-x EXIT]

[-enable_tensorboard ENABLE_TENSORBOARD]

[-log_tensorboard LOG_TENSORBOARD] [-conf CONF] [-f FILES]

[-pn PS_NUM] [-pc PS_CPU] [-pm PS_MEMORY] [-wn WORKER_NUM]

[-wc WORKER_CPU] [-wg WORKER_GPU] [-wm WORKER_MEMORY]

[-wnpg WNPG] [-ppn PPN] [-c COMMAND [COMMAND ...]]
```

The basic parameters are described as follows:

-   -t APP\_TYPE: Specifies the type of task to be submitted. The supported types are tensorflow-ps, tensorflow-mpi, and standalone. They are used in conjunction with the following –m MODE parameter.
    -   tensorflow-ps: Uses a parameter server for the communication of data, which is the PS mode of native TensorFlow.
    -   tensorflow-mpi: Uses Horovod, an open source framework from Uber, which relies on message passing interface \(MPI\) primitives for the communication of data.
    -   standalone: Users assign tasks to one instance in the YARN cluster for execution. This is similar to standalone execution.
-   –a APP\_NAME: Specifies the name of the submitted TensorFlow job. You can name jobs as required.
-   –m MODE: Specifies the runtime environment for submitted TensorFlow jobs. E-MapReduce supports the following environments: local, virtual-env, and docker.
    -   local: Uses Python runtime environments set up in the E-MapReduce worker nodes. If you want to use third-party Python packages, you need to install the packages on all the nodes manually.
    -   docker: Uses the Docker containers installed on the E-MapReduce worker nodes. TensorFlow runs in Docker containers.
    -   virtual-env: Uses isolated Python environments created by users. You can install Python libraries in Python environments. These libraries can be different from those installed in the environments that are set up in the worker nodes.
-   -m\_arg MODE\_ARG: Specifies the supplemental parameter for the –m MODE. If the runtime environment is docker, set the value to the docker image name. If the runtime environment is virtual-env, set the value to the name of Python environment tar.gz file.
-   –x Exit: You need to exit the parameter servers manually for certain distributed TensorFlow APIs. To exit parameter servers automatically when worker servers finish training their models, specify the -x option.
-   -enable\_tensorboard: Specifies whether to enable TensorBoard when TensorFlow starts training models.
-   -log\_tensorboard: Specifies the location of TensorBoard logs in HDFS. If TensorBoard is enabled when TensorFlow starts training models, this parameter is required.
-   -conf CONF: Specifies the location of the Hadoop configuration. Setting the value is optional. The default E-MapReduce configuration is used.
-   –f FILES: Specifies all dependent files and folders for TensorFlow to run, including executable scripts. If virtual-env files that are executed in a virtual environment are specified, you can put all dependencies in one folder. The script then automatically uploads the folders into HDFS according to the folder hierarchy.
-   -pn TensorFlow: Specifies the number of parameter servers to start.
-   -pc: Specifies the number of CPU cores that each parameter server requests.
-   -pm: Specifies the memory size that each parameter server requests.
-   -wn: Specifies the number of worker nodes started by TensorFlow.
-   -wc: Specifies the number of CPU cores that each worker requests.
-   -wg: Specifies the number of GPU cores that each worker requests.
-   -wm: Specifies the memory usage that each worker requests.
-   -c COMMAND: Specifies the command to run. For example, pythoncensus.py.

Advanced options. We recommend that you use advanced options with care, as they may result in job failures.

-   -wnpg: Specifies the number of workers that use a GPU simultaneously \(for tensorflow-ps\).
-   -ppn: Specifies the number of workers that use a GPU simultaneously \(for Horovod\). The preceding options refer to multitasking on a single GPU. Thresholds should be set to avoid GPU running out of memory.

