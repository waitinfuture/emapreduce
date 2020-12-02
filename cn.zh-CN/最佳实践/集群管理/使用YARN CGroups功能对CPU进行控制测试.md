# 使用YARN CGroups功能对CPU进行控制测试

CGroups（Control Groups）是Linux内核提供的一种功能，用来控制、限制与隔离一个进程组群的资源，包括CPU、内存和磁盘输入输出等资源。

## 概述

YARN中集成了CGroups的功能，使得NodeManager可以对Container的CPU的资源使用进行控制，例如可以对单个Container的CPU使用进行控制，也可以对NodeManager管理的总CPU进行控制。

## 开启YARN CGroups功能

**说明：** E-MapReduce集群中的YARN默认没有开启CGroups的功能，需要用户根据需求自行开启。

1.  登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com/)。

2.  进入YARN的**集群服务**页面。
    1.  单击上方的**集群管理**页签。
    2.  在**集群管理**页面，单击相应集群所在行的**集群ID**。
    3.  在左侧导航栏，单击**集群服务** \> **YARN**。
3.  启用CGroups。
    1.  单击右上角的**操作** \> **启用 CGroups**。
    2.  在**执行集群操作**页面，根据需求设置相应参数。
    3.  单击**确定**。
    4.  在**确认**页面，单击**确定**。
4.  重启NodeManager。
    1.  单击右上角的**操作** \> **重启NodeManager**。
    2.  在**执行集群操作**页面，根据需求设置相应参数。
    3.  单击**确定**。
    4.  在**确认**页面，单击**确定**。

## 控制参数

在开启了CGroups功能的前提下，可以通过调节YARN中的参数来控制CPU的资源使用行为：

1.  登录[阿里云 E-MapReduce 控制台](https://emr.console.aliyun.com/)。

2.  进入YARN的**集群服务**页面。
    1.  单击上方的**集群管理**页签。
    2.  在**集群管理**页面，单击相应集群所在行的**集群ID**。
    3.  在左侧导航栏，单击**集群服务** \> **YARN**。
3.  调节参数。
    1.  单击**配置**页签。
    2.  调节如下参数。

        |参数|描述|
        |--|--|
        |**yarn.nodemanager.resource.percentage-physical-cpu-limit**|NodeManager管理的所有Container使用CPU的阈值，默认100%。 **说明：** 任何场景下，NodeManager管理的Container的CPU都不能超过**yarn.nodemanager.resource.percentage-physical-cpu-limit**设置的比例。 |
        |**yarn.nodemanager.linux-container-executor.cgroups.strict-resource-usage**|对Container的CPU使用资源是否严格按照被分配的比例来进行控制。默认是false，即Container可以使用空闲的CPU。|


## 总Container的CPU控制测试

通过调节**yarn.nodemanager.resource.percentage-physical-cpu-limit**参数，以控制NodeManager管理的所有Container的CPU使用率。

下面集群配置为3台4核16GB，其中2台NodeManager，1台ResourceManager，分别设置该值以10、30和50为例，在YARN中运行一个hadoop pi作业，观察NodeManager所在机器的CPU的使用率。

**说明：** 下图中的`%CPU`表示进程占用单个核的比例；`%CPU(s)`表示所有用户进程占总CPU的比例。

-   设置参数为10。

    ![cpu_10](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5638420061/p69258.png)

    如上图所示，`%CPU(s)`接近10%；`%CPU`表示所有的test用户的Container进程加起来（7%+5.3%+5%+4.7%+4.7%+4.3%+4.3%+4%+2%=41.3%=0.413个核，约等于10%\*4core=0.4核，即4个核的10%比例）。

-   设置参数为30。

    ![cpu_30](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/5638420061/p69260.png)

    如上图所示，`%CPU(s)`接近30%；`%CPU`表示所有的test用户的Container进程加起来（19%+18.3%+18.3%+17%+16.7%+16.3%+14.7%+12%=132.3%=1.323个核，约等于30%\*4core=1.2核，即4个核的30%比例）。

-   设置参数为50。

    ![cpu_50](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6638420061/p69261.png)

    如上图所示，`%CPU(s)`接近50%；`%CPU`表示所有的test用户的Container进程加起来（65.1%+60.1%+43.5%+20.3%+3.7%+2%=194.7%=1.947个核，约等于50%\*4core=2核，即4个核的50%比例）。


## Container间的CPU控制测试

NodeManager上面启动多个Container，所有这些Container对CPU资源的占用不超过[总Container的CPU控制测试](#section_o63_1c4_m9w)中设置**yarn.nodemanager.resource.percentage-physical-cpu-limit**的阈值，NodeManager有共享模式（share）和严格模式（strict）两种方式，来管理和控制多个Container之间的CPU使用率，这两种方式通过参数**yarn.nodemanager.linux-container-executor.cgroups.strict-resource-usage**控制。

-   共享模式（share）

    当**yarn.nodemanager.linux-container-executor.cgroups.strict-resource-usage**设置为false时，即为共享模式（默认为false）。在这种模式下，Container除了实际被需要分配的CPU资源外，还可以利用空闲的CPU资源。

    例如，设置**yarn.nodemanager.resource.percentage-physical-cpu-limit**为50，设置**yarn.nodemanager.linux-container-executor.cgroups.strict-resource-usage**为false。NodeManager所在节点是4核，那么该Container申请按比例被分配的cpu资源为（1vcore÷8vcore）\*（4core\*50%）=0.25核，但是如果CPU有空闲，理论上该Container可以占满NodeManager管理的上限4core\*50%=2核。

    ![share](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6638420061/p69304.png)

    上图可以看出，test用户的多个Container进程占用CPU核数的比例相差很大（65%=0.65核；60.1%=0.61核； 3.7%=0.037核等），即单个Container的CPU使用没有被严格限制在（1vcore÷8vcore）\*（4core\*50%）=0.25核。

-   严格模式（strict）

    当**yarn.nodemanager.linux-container-executor.cgroups.strict-resource-usage**设置为true时，即为严格模式（strict）。在这种模式下，Container只能使用被需要分配的CPU资源，即使CPU有空闲也不能使用。

    例如，设置**yarn.nodemanager.resource.percentage-physical-cpu-limit**为50，设置**yarn.nodemanager.linux-container-executor.cgroups.strict-resource-usage**为true。

    ![strict](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6638420061/p69308.png)

    上图可以看出，test用户的多个Container进程占用CPU核数均约在0.25核（26.6%=0.266核；24.9%=0.249核），即该Container实际应该被分配的CPU使用率（1vcore÷8vcore）\*（4core\*50%）=0.25核。


