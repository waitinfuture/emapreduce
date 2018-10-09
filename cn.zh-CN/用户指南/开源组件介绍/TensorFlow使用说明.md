# TensorFlow使用说明 {#concept_lkf_243_kfb .concept}

E-MapReduce 3.13.0以后版本支持[TensorFlow](https://www.tensorflow.org)。用户在软件配置可选服务中添加TensorFlow组件。用户使用E-MapReduce TensorFlow运行高性能计算时，可通过YARN对集群中的CPU、GPU进行调度。

## 准备工作 {#section_fcc_cp3_kfb .section}

-   软件层面，E-MapReduce集群安装TensorFlow和TensorFlow on YARN组件。
-   硬件层面，E-MapReduce支持使用CPU和GPU两种资源进行计算。如您需使用GPU进行计算，可在core或task节点中选择ECS异构计算GPU计算型，如ecs.gn5，ecs.gn6机型。选定GPU机型后，选择您所需的CUDA和CuDNN版本。

## 提交TensorFlow作业 {#section_uqz_cq3_kfb .section}

用户可以登录emr主节点已命令行的方式提交tensorflow作业，如：

```
el_submit [-h] [-t APP_TYPE] [-a APP_NAME] [-m MODE] [-m_arg MODE_ARG]

[-interact INTERACT] [-x EXIT]

[-enable_tensorboard ENABLE_TENSORBOARD]

[-log_tensorboard LOG_TENSORBOARD] [-conf CONF] [-f FILES]

[-pn PS_NUM] [-pc PS_CPU] [-pm PS_MEMORY] [-wn WORKER_NUM]

[-wc WORKER_CPU] [-wg WORKER_GPU] [-wm WORKER_MEMORY]

[-wnpg WNPG] [-ppn PPN] [-c COMMAND [COMMAND ...]]
```

基础参数说明：

-   -t APP\_TYPE 提交的任务类型，支持三种类型的任务类型tensorflow-ps, tensorflow-mpi, standalone，三种类型要配合后面运行模式使用。
    -   tensorflow-ps采用Parameter Server方式通信，该方式为原生TensorFlow PS模式。
    -   tensorflow-mpi采用的是UBER开源的MPI架构horovod进行通信。
    -   standalone模式是用户将任务调度到YARN集群中启动单机任务，类似于单机运行。
-   –a APP\_NAME是指提交TensorFlow提交的作业名称，用户可以根据需要自行命名。
-   –m MODE，是指TensorFlow作业提交的运行时环境，E-MapReduce支持三种类型运行环境local, virtual-env, docker。
    -   local 模式，使用的是emr-worker上面的python运行环境，所以如果要使用一些第三方python包需要手动在所有机器上进行安装。
    -   docker模式， 使用的是emr-worker上面的docker运行时，tensorflow运行在docker container内。
    -   virtual-env 模式，用户上传的python环境，可以在python环境中安装一些不同于worker节点的python库。
-   -m\_arg MODE\_ARG，提交运行模式的补充参数，和MODE配合使用，如果运行时是docker，则设置为docker镜像名称，如果是virtual-env，则指定python环境文件名称，tar.gz打包。
-   –x Exit分布式TensorFlow有些API需要用户手动退出PS，在这种情况下用户可以指定-x选项，当所有worker完成训练并成 功后，PS节点自动退出。
-   -enable\_tensorboard是否在启动训练任务的同时启动TensorBoard。
-   -log\_tensorboard如果训练同时启动TensorBoard，需要指定TensorBoard日志位置，需要时HDFS上的位置。
-   -conf CONF Hadoop conf位置，默认可以不设，使用EMR默认配置。
-   –f FILES运行TensorFlow所有依赖的文件和文件夹，包含执行脚本，如果设置virtual-env执行的virtual-env文件。用户可 以将所有依赖放置到一个文件夹中，脚本会自动将文件夹按照文件夹层次关系上传到HDFS中。
-   -pn TensorFlow启动的参数服务器个数。
-   -pc每个参数服务器申请的CPU核数。
-   -pm每个参数服务器申请的内存大小。
-   -wn TensorFlow启动的Worker节点个数。
-   -wc每个Worker申请的CPU核数。
-   -wg每个Worker申请的GPU核数。
-   -wm每个Worker申请的内存个数。
-   -c COMMAND执行的命令，如pythoncensus.py。

进阶选项，用户需要谨慎使用进阶选项，可能造成任务失败。

-   -wnpg每个GPU核上同时跑的worker数量\(针对tensorflow-ps\)
-   -ppn 每个GPU核上同时跑的worker数量\(针对horovod\) 以上两个选项指的是单卡多进程操作，由于共用一张显卡，需要在程序上进行一定限制，否则会造成显卡OOM

