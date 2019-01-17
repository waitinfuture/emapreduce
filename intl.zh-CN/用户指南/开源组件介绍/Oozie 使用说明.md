# Oozie 使用说明 {#concept_xqx_svw_y2b .concept}

本文将介绍如何在E-MapReduce上使用Oozie。

**说明：** 阿里云 E-MapReduce 在 2.0.0 及之后的版本中提供了对 Oozie 的支持，如果需要在集群中使用 Oozie，请确认集群的版本不低于 2.0.0。

## 准备工作 {#section_wgp_pww_y2b .section}

在集群建立出来之后，需要打通 ssh 隧道，详细步骤请参考[SSH 登录集群](intl.zh-CN/用户指南/SSH 登录集群.md#)。

这里以 MAC 环境为例，使用 Chrome 浏览器实现端口转发（假设集群 master 节点公网 IP 为 **xx.xx.xx.xx**）：

1.  登录到 master 节点。

    ```
    ssh root@xx.xx.xx.xx
    ```

2.  输入密码。
3.  查看本机的 id\_rsa.pub 内容（注意在本机执行，不要在远程的 master 节点上执行）。

    ```
    cat ~/.ssh/id_rsa.pub
    ```

4.  将本机的 id\_rsa.pub 内容写入到远程 master 节点的 ~/.ssh/authorized\_keys 中（在远端 master 节点上执行）。

    ```
    mkdir ~/.ssh/
    vim ~/.ssh/authorized_keys
    ```

5.  然后将[步骤 2](#step2) 中看到的内容粘贴进来，现在应该可以直接使用 `ssh root@xx.xx.xx.xx` 免密登录 master 节点了。
6.  在本机执行以下命令进行端口转发。

    ```
    ssh -i ~/.ssh/id_rsa -ND 8157 root@xx.xx.xx.xx
    ```

7.  启动 Chrome（在本机新开 terminal 执行）。

    ```
    /Applications/Google\ Chrome.app/Contents/MacOS/Google\ Chrome --proxy-server="socks5://localhost:8157" --host-resolver-rules="MAP * 0.0.0.0 , EXCLUDE localhost" --user-data-dir=/tmp
    ```


## 访问 Oozie UI 页面 {#section_ez4_lyw_y2b .section}

在进行端口转发的 Chrome 浏览器中访问：xx.xx.xx.xx:11000/oozie，localhost:11000/oozie 或者内网 ip:11000/oozie。

## 提交workflow作业 {#section_pnw_qyw_y2b .section}

运行 Oozie 需要先安装 Oozie 的sharelib：[https://oozie.apache.org/docs/4.2.0/WorkflowFunctionalSpec.html\#ShareLib](https://oozie.apache.org/docs/4.2.0/WorkflowFunctionalSpec.html#ShareLib)

在 E-MapReduce 集群中，默认给 Oozie 用户安装了 sharelib，即如果使用 Oozie 用户来提交 workflow 作业，则不需要再进行 sharelib 的安装。

由于开启 HA的集群和没有开启 HA 的集群，访问 NameNode 和 ResourceManager 的方式不同，在提交 oozie workflow job 的时候，job.properties 文件中需要指定不同的 NameNode 和 JobTracker （ResourceManager）。具体如下：

-   非 HA 集群

    ```
    nameNode=hdfs://emr-header-1:9000
    jobTracker=emr-header-1:8032
    ```

-   HA 集群

    ```
    nameNode=hdfs://emr-cluster
    jobTracker=rm1,rm2
    ```


下面操作示例中，已经针对是否是 HA 集群配置好了，即样例代码不需要任何修改即可以直接运行。关于 workflow 文件的具体格式，请参考 Oozie 官方文档：[https://oozie.apache.org/docs/4.2.0/](https://oozie.apache.org/docs/4.2.0/%E3%80%82).

-   **在非 HA 集群上提交 workflow 作业**
    1.  登录集群的主 master 节点。

        ```
        ssh root@master公网Ip
        ```

    2.  下载示例代码。

        ```
        [root@emr-header-1 ~]# su oozie
        [oozie@emr-header-1 root]$ cd /tmp
        [oozie@emr-header-1 tmp]$ wget http://emr-sample-projects.oss-cn-hangzhou.aliyuncs.com/oozie-examples/oozie-examples.zip
        [oozie@emr-header-1 tmp]$ unzip oozie-examples.zip
        ```

    3.  将 Oozie workflow 代码同步到 hdfs 上。

        ```
        [oozie@emr-header-1 tmp]$ hadoop fs -copyFromLocal examples/ /user/oozie/examples
        ```

    4.  提交 Oozie workflow 样例作业。

        ```
        [oozie@emr-header-1 tmp]$ $OOZIE_HOME/bin/oozie job -config examples/apps/map-reduce/job.properties -run
        ```

        执行成功之后，会返回一个 jobId，类似：

        ```
        job: 0000000-160627195651086-oozie-oozi-W
        ```

    5.  访问 Oozie UI 页面，可以看到刚刚提交的 Oozie workflow job。
-   **在 HA 集群上提交 workflow 作业**
    1.  登录集群的主 master 节点。

        ```
        ssh root@主master公网Ip
        ```

        可以通过是否能访问 Oozie UI 来判断哪个 master 节点是当前的主 master 节点， Oozie server 服务默认是启动在主 master 节点 `xx.xx.xx.xx:11000/oozie`。

    2.  下载 HA 集群的示例代码。

        ```
        [root@emr-header-1 ~]# su oozie
        [oozie@emr-header-1 root]$ cd /tmp
        [oozie@emr-header-1 tmp]$ wget http://emr-sample-projects.oss-cn-hangzhou.aliyuncs.com/oozie-examples/oozie-examples-ha.zip
        [oozie@emr-header-1 tmp]$ unzip oozie-examples-ha.zip
        ```

    3.  将 Oozie workflow 代码同步到 hdfs 上。

        ```
        [oozie@emr-header-1 tmp]$ hadoop fs -copyFromLocal examples/ /user/oozie/examples
        ```

    4.  提交 Oozie workflow 样例作业。

        ```
        [oozie@emr-header-1 tmp]$ $OOZIE_HOME/bin/oozie job -config examples/apps/map-reduce/job.properties -run
        ```

        执行成功之后，会返回一个 jobId，类似：

        ```
        job: 0000000-160627195651086-oozie-oozi-W
        ```

    5.  访问 Oozie UI 页面，可以看到刚刚提交的 Oozie workflow job。

