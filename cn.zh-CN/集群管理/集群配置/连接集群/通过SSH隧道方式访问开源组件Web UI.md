# 通过SSH隧道方式访问开源组件Web UI

本文介绍如何通过SSH隧道方式访问开源组件的Web UI。

-   已创建集群，详情请参见[创建集群](/cn.zh-CN/集群管理/集群配置/创建集群.md)。
-   确保集群所在的安全组已开放22端口。您可以在创建集群时打开**远程登录**开关，也可以在集群创建好之后手动添加安全组规则，具体操作请参见[添加安全组规则](/cn.zh-CN/安全/安全组/添加安全组规则.md)。

    **说明：** 在安全组规则中手动添加入方向规则：其中授权类型为**IPv4地址段访问**，端口为**22/22**。

-   确保本地服务器与集群主节点网络连通。您可以在创建集群时打开**挂载公网**开关，或者在集群创建好之后在ECS控制台上为主节点挂载公网，为主节点ECS实例分配固定公网IP或EIP，详情请参见[绑定弹性网卡](/cn.zh-CN/网络/弹性网卡/绑定弹性网卡.md)。

在E-MapReduce集群中，为保证集群安全，Hadoop、Spark和Flink等开源组件的Web UI的端口均未对外开放。您可以通过控制台的方式访问Web UI，也可以通过在本地服务器上建立SSH隧道以端口转发的方式来访问Web UI，端口转发方式包括端口动态转发和本地端口转发两种。

如果您需要通过控制台的方式访问Web UI时，请参见[访问链接与端口](/cn.zh-CN/集群管理/集群配置/访问链接与端口.md)。

## 获取主节点的公网IP地址

1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

3.  单击上方的**集群管理**页签。

4.  在**集群管理**页面，单击相应集群所在行的**详情**。

5.  在**集群基础信息**页面的**主机信息**区域，获取主节点的公网IP地址。

    ![IP](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3540659951/p110921.png)


## 获取主节点的主机名

1.  登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com/)。

2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

3.  单击上方的**集群管理**页签。

4.  在**集群管理**页面，单击相应集群所在行的**详情**。

5.  在左侧导航栏，单击**主机列表**。

6.  在**主机列表**页面，查看主节点公网IP地址对应的**主机名**。

    主节点IP地址请参见[获取主节点的公网IP地址](#section_coc_7pb_145)。

    ![header](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3540659951/p111252.png)


## 使用动态端口转发方式

创建从本地服务器开放端口到集群主节点的SSH隧道，并运行侦听该端口的本地SOCKS代理服务器，端口的数据会由SSH隧道转发到集群主节点。

1.  创建SSH隧道。

    -   密钥方式

        ```
        ssh -i [密钥文件路径] -N -D 8157 root@[主节点公网IP地址]
        ```

    -   密码方式

        ```
        ssh -N -D 8157 root@[主节点公网IP地址]
        ```

    相关参数描述如下：

    -   `8157`：本地服务器端口以`8157`为例，实际配置时，您可使用本地服务器未被使用的任意一个端口。
    -   `-D`：表示使用动态端口转发，启动SOCKS代理进程并侦听用户本地端口。
    -   主节点公网IP地址：获取方式请参见[获取主节点的公网IP地址](#section_coc_7pb_145)。
2.  配置浏览器。

    **说明：** 完成隧道创建之后，请保持终端打开状态，此时并不会返回响应。

    完成动态转发配置以后，您可以从以下两种方式中选择一种来进行浏览器配置。

    -   Chrome浏览器命令行方式
        1.  打开命令行窗口，进入本地Google Chrome浏览器客户端的安装目录。

            操作系统不同，Chrome浏览器的默认安装目录不同。

            |操作系统|Chrome默认安装路径|
            |:---|:-----------|
            |Mac OS X|/Applications/Google Chrome.app/Contents/MacOS/Google Chrome|
            |Linux|/usr/bin/google-chrome|
            |Windows|C:\\Program Files \(x86\)\\Google\\Chrome\\Application\\|

        2.  在Chrome浏览器的默认安装目录下，执行以下命令。

            ```
            chrome --proxy-server="socks5://localhost:8157" --host-resolver-rules="MAP * 0.0.0.0 , EXCLUDE localhost" --user-data-dir=/tmp/
            ```

            相关参数描述如下：

            -   `/tmp/`：如果是Windows操作系统，则`/tmp/`可以写成类似/c:/tmppath/的路径。如果是Linux或者Mac OS X操作系统，则可直接写成/tmp/路径。
            -   `8157`：本地服务器端口以`8157`为例，实际配置时，您可使用本地服务器未被使用的任意一个端口。
        3.  在浏览器地址栏输入http://主机名:端口，即可访问相应的Web UI。

            主机名的获取请参见[获取主节点的主机名](#section_541_dvb_3f4)。

            例如：访问Yarn页面时，在浏览器地址栏输入http://emr-header-1:8088。

    -   代理扩展程序方式

        代理扩展程序可以帮助您更加轻松地在浏览器中管理和使用代理，确保网页浏览和集群Web UI访问互不干扰。

        1.  安装Chrome的[SwitchyOmega](https://chrome.google.com/webstore/detail/proxy-switchyomega/padekgcemlokbadohgkifijomclgjgif?hl)插件。
        2.  安装完成以后，单击SwitchyOmega插件，然后在弹出框中选择**选项**进行配置。
        3.  单击**新建情景模式**，输入**情景模式名称**（例如SSH tunnel），情景模式类型选择**PAC 情景模式**。
        4.  在**PAC 脚本**中配置以下内容。

            ```
            function regExpMatch(url, pattern) {    
              try { return new RegExp(pattern).test(url); } catch(ex) { return false; }    
            }
            
            function FindProxyForURL(url, host) {
                // Important: replace 172.31 below with the proper prefix for your VPC subnet
            
                if (shExpMatch(url, "*localhost*")) return "SOCKS5 localhost:8157";
                if (shExpMatch(url, "*emr-header*")) return "SOCKS5 localhost:8157";
                if (shExpMatch(url, "*emr-worker*")) return "SOCKS5 localhost:8157";
            
                return 'DIRECT';
            }
            ```

        5.  完成上述参数配置后，在左侧导航栏中单击**应用选项**。
        6.  打开Chrome浏览器，在Chrome中单击SwitchyOmega插件，切换到之前创建的SSH tunnel情景模式下。
        7.  在浏览器地址栏输入http://主机名:端口。

            即可访问相应的Web UI。组件端口配置请参见[集群端口配置](/cn.zh-CN/集群管理/集群运维/集群端口配置.md)，主机名的获取请参见[获取主节点的主机名](#section_541_dvb_3f4)。

            例如：访问Yarn页面时，在浏览器地址栏输入http://emr-header-1:8088。


## 使用本地端口转发方式

**说明：** 此方式只能查看最外层的页面，无法查看详细的作业信息。

您可以通过SSH本地端口转发（即将主实例端口转发到本地端口），访问当前主节点上运行的网络应用界面，而不使用SOCKS代理。

1.  在本地服务器终端输入以下命令，创建SSH隧道。

    -   密钥方式

        ```
        ssh -i [密钥文件路径] -N -L 8157:[主节点主机名]:8088 root@[主节点公网IP地址]
        ```

    -   密码方式

        ```
        ssh -N -L 8157:[主节点主机名]:8088 root@[主节点公网IP地址]
        ```

    相关参数描述如下：

    -   `-L`：表示使用本地端口转发，您可以指定一个本地端口，用于将数据转发到主节点本地Web服务器上标识的远程端口。
    -   `8088`：主节点上ResourceManager的访问端口，您可以替换为其他组件的端口。

        组件端口配置请参见[集群端口配置](/cn.zh-CN/集群管理/集群运维/集群端口配置.md)，主机名的获取请参见[获取主节点的主机名](#section_541_dvb_3f4)。

    -   `8157`：本地服务器端口以`8157`为例，实际配置时，您可使用本地服务器未被使用的任意一个端口。
2.  保持终端打开状态，打开浏览器中，在浏览器地址栏输入http://localhost:8157/。


