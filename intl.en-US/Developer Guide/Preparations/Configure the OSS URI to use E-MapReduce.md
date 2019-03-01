# Configure the OSS URI to use E-MapReduce {#concept_ltj_fjg_hfb .concept}

This topic describes how to configure the OSS URI to use E-MapReduce.

## OSS URI {#section_ekl_slg_hfb .section}

When using E-MapReduce, you can use two types of OSS URIs:

-   native URI： oss://\[accessKeyId:accessKeySecret@\]bucket\[.endpoint\]/object/path

    This URI is used for specifying input and output data sources for a job, which is similar to hdfs://. When you operate OSS data, you can configure the accessKey ID, accessKey Secret, and the endpoint. Alternatively, you can specify the accessKey ID, accessKey Secret, and the endpoint in the URI.

-   ref URI: ossref://bucket/object/path

    It is only valid in the configuration of an E-MapReduce job and is used to specify the resources needed for running the job.


We call prefixes, such as oss and ossref, as schemes. Pay special attention to the scheme difference in URI.

**Note:** 

Currently, operations only support OSS for standard storage types.

-   E-MapReduce uses the multipart mode to upload large files to OSS. Note that if your job is interrupted, some of the result data remains in OSS. You must remove it manually. The procedure here is the same with the use of HDFS. One difference, however, is that E-MapReduce uses the multipart mode to upload large files. The file fragments are uploaded to the OSS fragment management. Therefore, you must delete the remaining job files in OSS file management and clean the file fragments in OSS fragment management. Otherwise, you are charged for data storage.
-   Except for the preceding manual cleanup, you can also configure the lifecycle of the fragments, so that the expired fragments are automatically cleaned. For more information, see [Lifecycle management of OSS files](../../SP_21/DNOSS11848834/EN-US_TP_4748.dita#concept_bmx_p2f_vdb).

