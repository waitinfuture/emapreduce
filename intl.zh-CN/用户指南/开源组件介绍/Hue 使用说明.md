# Hue 使用说明 {#concept_uh5_qtw_y2b .concept}

目前 E-MapReduce 中支持了 [Hue](http://gethue.com/)，可以通过Apache Knox访问Hue。

## 准备工作 {#section_vy3_stw_y2b .section}

在集群[安全组](intl.zh-CN/用户指南/集群配置/安全组.md#)中[设置安全组规则](../../../../../intl.zh-CN/安全/安全组/添加安全组规则.md#)，打开8888端口。

**说明：** 设置安全组规则时要针对有限的IP范围。禁止在配置的时候对0.0.0.0/0开放规则。

## 访问Hue {#section_ws2_j5w_y2b .section}

EMR的Web界面中提供了快速访问Hue的链接，访问链接查看方式如下：

1.  在集群列表页面，单击集群ID右侧的**管理**。
2.  在页面左侧导航栏中单击**访问链接与端口**。

## 访问密码 {#section_vbs_l5w_y2b .section}

由于Hue默认第一次运行时，如果还没有设置管理员，第一个登录的用户就默认设置为管理员。为了安全起见，EMR会默认生成一个管理员账号与密码，管理员账号是admin，密码通过如下途径查看：

1.  在集群列表页面，单击集群ID右侧的**管理**。
2.  在服务列表中，选择**Hue**。
3.  单击**配置**页签，其中有一个admin\_pwd的参数，这个就是随机密码。

## 忘记密码 {#section_ekc_p5w_y2b .section}

如果用户忘记了自己的Hue账号所对应的密码，可以通过以下方式重新创建一个账号：

1.  在集群列表页面，单击集群ID右侧的**管理**。
2.  在页面左侧导航栏中单击**集群基础信息**。
3.  在**主实例组**部分获取master节点的公网IP。
4.  通过SSH方式登录master节点。
5.  执行以下命令，创建新账号。

    ```
    /opt/apps/hue/build/env/bin/hue createsuperuser
    ```

6.  输入新用户名、电子邮件，然后输入密码，再次输入密码后按回车键。

    如果提示 **Superuser created successfully**，则说明新账号创建成功，稍后用新账号登录Hue即可。


## 添加/修改配置 { .section}

1.  在EMR管理控制台，单击集群ID右侧的**管理**。
2.  在服务列表中，选择**Hue**，然后单击**配置**页签。
3.  点击右上角的**自定义配置**按钮，添加需要添加/修改的key/value值，其中key需要遵循下面规范：

    ```
    $section_path.$real_key
    ```

    **说明：** 

    -   $real\_key 即为需要添加的实际的key，如hive\_server\_host。
    -   $real\_key 前面的 $section\_path 可以通过 hue.ini 文件进行查看，例如：

        hive\_server\_host，通过[hue.ini](https://github.com/cloudera/hue/blob/release-4.1.0/desktop/conf.dist/hue.ini)文件可以看出它属于\[beeswax\]这个section下，则 $section\_path 为 beeswax。

    -   综上，添加的key为 beeswax.hive\_server\_host。
    -   同理，如需修改hue.ini文件中的多级 section \[desktop\] -\> \[\[ldap\]\] -\> \[\[\[ldap\_servers\]\]\] -\> \[\[\[\[users\]\]\]\] -\>user\_name\_attr的值，则需要配置的key为desktop.ldap.ldap\_servers.users.user\_name\_attr。

