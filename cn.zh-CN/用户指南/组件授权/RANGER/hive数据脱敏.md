# hive数据脱敏 {#concept_ncg_ydg_mgb .concept}

Ranger支持对Hive数据的脱敏处理\(Data Masking\)，它对select的返回结果进行脱敏处理，对用户屏蔽敏感信息。

**说明：** 该功能只针对HiveServer2的场景\(如beeline/jdbc/Hue等途径执行的select\)，对于使用Hive Client\(如hive -e 'select xxxx'\)不支持。

接下来介绍如何在E-MapReduce中使用该功能。

## Hive组件配置Ranger {#section_xwq_vfg_mgb .section}

参见文档: [Hive配置](intl.zh-CN/用户指南/组件授权/RANGER/Hive配置.md#)

## 配置Data Mask Policy {#section_osz_33g_mgb .section}

在Ranger UI的`emr-hive`的service页面可以对用户访问Hive数据进行脱敏处理：

-   支持多种脱敏处理方式，比如显示开始的4个字符/显示最后的4个字符/Hash处理等
-   配置Mask Policy时不支持通配符\(如policy中table/column不能配置为\*\)
-   每个policy只能配置一个列的mask策略，多个列需要配置各自的mask policy

配置Policy流程如下所示：![配置Policy](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/105886/155255260937543_zh-CN.png)![配置Policy](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/105886/155255260937548_zh-CN.png)![配置Policy](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/105886/155255260937550_zh-CN.png)

最后保存即可。

## 测试数据脱敏 {#section_z3g_4mg_mgb .section}

-   场景：

    用户test在select表testdb1.testtbl中列a的数据时，只显示最开始的4个字符。

-   流程：
    1.  配置policy

        在上节的最后一个截图，其实就是配置了该场景的一个policy，可参考上图\(其中脱敏方式选择了show first 4\)。

    2.  脱敏验证

        test用户使用beeline连接HiveServer2,执行`select a from testdb1.testtbl`

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/105886/155255261037553_zh-CN.png)

        如上图所示，test用户执行select命令后，列a显示的数据只有前面4个字符是正常显示，后面字符全部用`x`来脱敏处理。


