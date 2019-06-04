# TensorFlow 使用说明 {#concept_lkf_243_kfb .concept}

E-MapReduce 3.13.0 以后版本支持[TensorFlow](https://www.tensorflow.org)。您可以在软件配置可选服务中添加 TensorFlow 组件。当您使用 E-MapReduce TensorFlow 运行高性能计算时，可通过 YARN 对集群中的 CPU、GPU 进行调度。

## 准备工作 {#section_fcc_cp3_kfb .section}

-   软件层面，E-MapReduce 集群安装 TensorFlow 和 TensorFlow on YARN 组件。
-   硬件层面，E-MapReduce 支持使用 CPU 和 GPU 两种资源进行计算。如您需使用 GPU 进行计算，可在 core 或 task 节点中选择 ECS 异构计算 GPU 计算型，如 ecs.gn5，ecs.gn6 机型。选定 GPU 机型后，选择您所需的 CUDA 和 CuDNN 版本。

## 提交 TensorFlow 作业 {#section_uqz_cq3_kfb .section}

用户可以登录 EMR 主节点已命令行的方式提交 Tensorflow 作业，例如：

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

-   -t APP\_TYPE 提交的任务类型，支持三种类型的任务类型 tensorflow-ps， tensorflow-mpi，standalone，三种类型要配合后面运行模式使用。
    -   tensorflow-ps 采用 Parameter Server 方式通信，该方式为原生 TensorFlow PS 模式。
    -   tensorflow-mpi 采用的是 UBER 开源的 MPI 架构 horovod 进行通信。
    -   standalone 模式是用户将任务调度到 YARN 集群中启动单机任务，类似于单机运行。
-   –a APP\_NAME 是指提交 TensorFlow 提交的作业名称，用户可以根据需要自行命名。
-   –m MODE，是指 TensorFlow 作业提交的运行时环境，E-MapReduce 支持三种类型运行环境 local, virtual-env, docker。
    -   local 模式，使用的是 emr-worker 上面的 python 运行环境，所以如果要使用一些第三方 Python 包需要手动在所有机器上进行安装。
    -   docker 模式， 使用的是 emr-worker 上面的 docker 运行时，tensorflow 运行在 docker container 内。
    -   virtual-env 模式，用户上传的 Python 环境，可以在 Python 环境中安装一些不同于 worker 节点的 Python 库。
-   -m\_arg MODE\_ARG，提交运行模式的补充参数，和 MODE 配合使用，如果运行时是 docker，则设置为 docker镜像名称，如果是 virtual-env，则指定 Python 环境文件名称，tar.gz 打包。
-   –x Exit分布式 TensorFlow 有些 API 需要用户手动退出 PS，在这种情况下用户可以指定 -x 选项，当所有 worker 完成训练并成 功后，PS 节点自动退出。
-   -enable\_tensorboard 是否在启动训练任务的同时启动 TensorBoard。
-   -log\_tensorboard 如果训练同时启动 TensorBoard，需要指定 TensorBoard 日志位置，需要时 HDFS 上的位置。
-   -conf CONF Hadoop conf 位置，默认可以不设，使用 EMR 默认配置即可。
-   –f FILES 运行 TensorFlow 所有依赖的文件和文件夹，包含执行脚本，如果设置 virtual-env 执行的 virtual-env 文件，用户可 以将所有依赖放置到一个文件夹中，脚本会自动将文件夹按照文件夹层次关系上传到 HDFS 中。
-   -pn TensorFlow 启动的参数服务器个数。
-   -pc 每个参数服务器申请的CPU核数。
-   -pm 每个参数服务器申请的内存大小。
-   -wn TensorFlow 启动的Worker节点个数。
-   -wc 每个 Worker 申请的 CPU 核数。
-   -wg 每个 Worker 申请的 GPU 核数。
-   -wm 每个 Worker 申请的内存个数。
-   -c COMMAND执行的命令，如 pythoncensus.py。

进阶选项，用户需要谨慎使用进阶选项，可能造成任务失败。

-   -wnpg 每个 GPU 核上同时跑的 Worker 数量（针对 tensorflow-ps）。
-   -ppn 每个 GPU 核上同时跑的 Worker 数量（针对 horovod） 以上两个选项指的是单卡多进程操作，由于共用一张显卡，需要在程序上进行一定限制，否则会造成显卡 OOM。

