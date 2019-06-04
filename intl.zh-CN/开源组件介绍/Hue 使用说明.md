# Hue 使用说明 {#concept_uh5_qtw_y2b .concept}

目前 E-MapReduce 中支持了 [Hue](http://gethue.com/)，可以通过 Apache Knox 访问 Hue。

## 准备工作 {#section_vy3_stw_y2b .section}

在集群[安全组](../../../../intl.zh-CN/集群规划与配置/集群配置/安全组.md#)中[设置安全组规则](../../../../intl.zh-CN/安全/安全组/添加安全组规则.md#)，打开 8888 端口。

**说明：** 设置安全组规则时要针对有限的IP范围。禁止在配置的时候对 0.0.0.0/0 开放规则。

## 访问 Hue {#section_ws2_j5w_y2b .section}

在 E-MapReduce 控制台中提供了快速访问集群中 Hue 服务的链接入口，您可通过以下方式访问 Hue 服务：

1.  在集群列表页面，单击集群 ID 右侧的**管理**。
2.  在页面左侧导航栏中单击**访问链接与端口**。
3.  单击 Hue 服务对应的访问链接。

## 查看访问密码 {#section_vbs_l5w_y2b .section}

Hue 服务默认第一次运行时，如果未设置管理则将第一个登录用户设置为管理员。因此出于安全考虑，E-MapReduce 将默认为 Hue 服务创建一个管理员，账号为 admin。您可以通过以下方式查看该管理员账号的密码：

1.  在集群列表页面，单击集群 ID 右侧的**管理**。
2.  单击左侧导航栏中的**集群服务**，在服务列表中，选择 **Hue**。
3.  单击**配置**页签，找到 admin\_pwd 参数，该参数对应的就是随机密码。

## 创建 Hue 用户账号 {#section_ekc_p5w_y2b .section}

如果用户忘记了自己的 Hue 账号所对应的密码，可以通过以下方式重新创建一个账号：

1.  在集群列表页面，单击集群 ID 右侧的**管理**。
2.  在页面左侧导航栏中单击**集群基础信息**。
3.  在**主实例组**部分获取 Master 节点的公网 IP。
4.  通过 [SSH 登录集群](../../../../intl.zh-CN/集群规划与配置/集群配置/SSH 登录集群.md#)的方式登录 Master 节点。
5.  执行以下命令，创建新账号。

    ```
    /opt/apps/hue/build/env/bin/hue createsuperuser
    ```

6.  输入新用户名、电子邮件，然后输入密码，再次输入密码后按回车键。

    如果提示 **Superuser created successfully**，则说明新账号创建成功，稍后用新账号登录 Hue 即可。


## 添加/修改配置 { .section}

您可以通过自定义配置添加相关配置：

1.  在 E-MapReduce 管理控制台，单击集群 ID 右侧的**管理**。
2.  在服务列表中，选择**Hue**，然后单击**配置**页签。
3.  单击右上角的**自定义配置**按钮，添加配置的 key/value 值，其中 key 需要遵循下面规范：

    ```
    $section_path.$real_key
    ```

    **说明：** 

    -   $real\_key 即为需要添加的实际的key，如 hive\_server\_host。
    -   $real\_key 前面的 $section\_path 可以通过 hue.ini 文件进行查看，例如：

        hive\_server\_host，通过[hue.ini](https://github.com/cloudera/hue/blob/release-4.1.0/desktop/conf.dist/hue.ini)文件可以看出它属于\[beeswax\]这个section下，则 $section\_path 为 beeswax。

    -   综上，添加的 key 为 beeswax.hive\_server\_host。
    -   同理，如需修改 hue.ini文件中的多级 section \[desktop\] -\> \[\[ldap\]\] -\> \[\[\[ldap\_servers\]\]\] -\> \[\[\[\[users\]\]\]\] -\>user\_name\_attr 的值，则需要配置的 key 为desktop.ldap.ldap\_servers.users.user\_name\_attr。

