# OSS 参考使用说明 {#concept_ltj_fjg_hfb .concept}

本文介绍在 E-MapReduce 作业配置中如何通过 OSS 配置输入和输出数据源。

## OSS URI {#section_ekl_slg_hfb .section}

在使用 E-MapReduce 时，您将会使用两种 OSS URI，分别是：

-   native URI： oss://\[accessKeyId:accessKeySecret@\]bucket\[.endpoint\]/object/path 

    您在作业中指定输入输出数据源时使用这种 URI，可以类比 hdfs://。您操作 OSS 数据时，可以将 accessKeyId，accessKeySecret 以及 endpoint 配置到 Configuration 中，也可以在 URI 中直接指定 accessKeyId，accessKeySecret 以及 endpoint。

-   ref URI： ossref://bucket/object/path

    只在 E-MapReduce 作业配置时有效，用来指定作业运行需要的资源。例如以下作业配置示例：


我们把 oss 与 ossref 这样的前缀称为 scheme。在使用过程中，需要特别注意 URI 中 scheme 的不同。

**说明：** 

当前所有操作都只支持标准存储类型的 OSS。

-   E-MapReduce 使用 multipart 方式向 OSS 上传大文件。需要注意，当作业异常中断后，OSS 中会残留作业的部分结果数据，需要您手动删掉。这里的行为和使用 HDFS 的方式是一致的。但有一个区别，E-MapReduce 会用到 multipart 方式上传大文件，此时会将文件碎片上传到 OSS 的碎片管理中，所以您不仅要删除 OSS 文件管理中的作业残留文件，还需将 OSS 碎片管理中的文件碎片清理一次，否则会产生数据存储费用。
-   除了上述手动清理，您也可以配置碎片的生命周期，配置完成后过期的文件碎片会被自动清理掉。详情请参考[OSS的文件生命周期管理说明](../../../../intl.zh-CN/控制台用户指南/管理存储空间/设置生命周期规则.md#)。

