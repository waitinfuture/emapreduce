# Credential Provider使用说明

您可以使用Credential Provider配置加密后的AccessKey信息至文件中，避免泄露AccessKey信息。

EMR-3.30.0版本支持JindoOSS Credential Provider功能。您可以通过使用Hadoop Credential Provider将加密后的AccessKey信息存入文件，从而避免配置明文AccessKey，根据不同情况选择合适的JindoOSS Credential Provider。

## 配置JindoOSS Credential Provider

1.  进入SmartData服务。

    1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

    2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

    3.  单击上方的**集群管理**页签。

    4.  在**集群管理**页面，单击相应集群所在行的**详情**。

    5.  在左侧导航栏单击**集群服务** \> **SmartData**。

2.  进入**smartdata-site**服务配置。

    1.  单击**配置**页签。

    2.  在**服务配置**区域，单击**smartdata-site**页签。

3.  添加配置信息。

    1.  在**smartdata-site**页签，单击右上角的**自定义配置**

    2.  在**新增配置项**对话框中，新增如下配置。

        |参数|描述|
        |--|--|
        |**fs.jfs.cache.credentials.provider**|配置com.aliyun.emr.fs.auth.AliyunCredentialsProvider的实现类，多个类时使用英文逗号（, ）隔开，按照先后顺序读取Credential直至读到有效的Credential。例如，`com.aliyun.emr.fs.auth.TemporaryAliyunCredentialsProvider, com.aliyun.emr.fs.auth.SimpleAliyunCredentialsProvider,com.aliyun.emr.fs.auth.EnvironmentVariableCredentialsProvider`。|

        您可以根据情况，选择不同的Credential Provider，支持如下Provider：

        -   [TemporaryAliyunCredentialsProvider](#section_ek7_ghi_o52)
        -   [SimpleAliyunCredentialsProvider](#section_nsb_c15_r6o)
        -   [EnvironmentVariableCredentialsProvider](#section_vik_xxr_hbx)
        -   [InstanceProfileCredentialsProvider](#section_7zw_is0_tn8)

## 使用Hadoop Credential Providers存储AccessKey信息

**说明：** Hadoop Credential Provider详情的使用方法，请参见[CredentialProvider API Guide](https://hadoop.apache.org/docs/current/hadoop-project-dist/hadoop-common/CredentialProviderAPI.html)。

**fs.jfs.cache.oss.accessKeyId**、**fs.jfs.cache.oss.accessKeySecret**和**fs.jfs.cache.oss.securityToken**可以存储至Hadoop Credential Providers。

使用Hadoop提供的命令，存储AccesKey和SecurityToken信息至Credential文件中。命令格式如下。

```
hadoop credential <subcommand> [options]
```

例如，存储AccessKey和Token信息至jceks文件中，jceks代表Java Keystore Provider，除了使用文件权限保护该文件外，您也可以指定密码加密存储信息，如果不指定密码则使用默认字符串加密。

```
hadoop credential create fs.jfs.cache.oss.accessKeyId -value AAA -provider jceks://file/root/oss.jceks
hadoop credential create fs.jfs.cache.oss.accessKeySecret -value BBB -provider jceks://file/root/oss.jceks
hadoop credential create fs.jfs.cache.oss.securityToken -value  CCC -provider jceks://file/root/oss.jceks
```

生成Credenttial文件后，您需要配置下面的参数来指定Provider的类型和位置。

|参数|描述|
|--|--|
|**fs.jfs.cache.oss.security.credential.provider.path**|配置存储AccessKey的Credential文件。例如，jceks://file/$\{user.home\}/oss.jceks为HOME下的oss.jceks文件。 |

## TemporaryAliyunCredentialsProvider

适合使用有时效性的AccessKey和SecurityToken访问OSS的情况。

|参数|参数说明|
|--|----|
|**fs.jfs.cache.credentials.provider**|**com.aliyun.emr.fs.auth.TemporaryAliyunCredentialsProvider**|
|**fs.jfs.cache.oss.accessKeyId**|OSS的AccessKey Id。|
|**fs.jfs.cache.oss.accessKeySecret**|OSS的AccessKey Secret。|
|**fs.jfs.cache.oss.securityToken**|OSS的SecurityToken（临时安全令牌）。|

## SimpleAliyunCredentialsProvider

适合使用长期有效的AccessKey访问OSS的情况。

|参数|参数说明|
|--|----|
|**fs.jfs.cache.credentials.provider**|**com.aliyun.emr.fs.auth.SimpleAliyunCredentialsProvider**|
|**fs.jfs.cache.oss.accessKeyId**|OSS的AccessKey Id。|
|**fs.jfs.cache.oss.accessKeySecret**|OSS的AccessKey Secret。|

## EnvironmentVariableCredentialsProvider

该方式需要在环境变量中配置以下参数。

|参数|参数说明|
|--|----|
|**fs.jfs.cache.credentials.provider**|**com.aliyun.emr.fs.auth.EnvironmentVariableCredentialsProvider**|
|**ALIYUN\_ACCESS\_KEY\_ID**|OSS的AccessKey Id。|
|**ALIYUN\_ACCESS\_KEY\_SECRET**|OSS的AccessKey Secret。|
|**ALIYUN\_SECURITY\_TOKEN**|OSS的SecityToken（临时安全令牌）。**说明：** 仅配置有时效Token时需要。 |

## InstanceProfileCredentialsProvider

该方式无需配置AccessKey，免密方式访问OSS。

|参数|参数说明|
|--|----|
|**fs.jfs.cache.credentials.provider**|**com.aliyun.emr.fs.auth.InstanceProfileCredentialsProvider**|

