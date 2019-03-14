# Hive 数据脱敏 {#concept_ncg_ydg_mgb .concept}

Ranger 支持对 Hive 数据的脱敏处理\(Data Masking\)，它对 select 的返回结果进行脱敏处理，对用户屏蔽敏感信息。

**说明：** 该功能只针对 HiveServer2 的场景\(如 beeline/jdbc/Hue 等途径执行的 select\)，对于使用 Hive Client\(如 hive -e 'select xxxx'\)不支持。

接下来介绍如何在E-MapReduce中使用该功能。

## Hive组件配置Ranger {#section_xwq_vfg_mgb .section}

参见文档: [Hive 配置](intl.zh-CN/用户指南/组件授权/RANGER/Hive配置.md#)

## 配置Data Mask Policy {#section_osz_33g_mgb .section}

在 Ranger UI 的`emr-hive`的 service 页面可以对用户访问 Hive 数据进行脱敏处理：

-   支持多种脱敏处理方式，比如显示开始的4个字符/显示最后的4个字符/Hash处理等
-   配置 Mask Policy 时不支持通配符\(如policy中table/column不能配置为\*\)
-   每个 policy 只能配置一个列的 mask 策略，多个列需要配置各自的 mask policy

配置 Policy 流程如下所示：![配置Policy](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/105886/155255306637543_zh-CN.png)![配置Policy](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/105886/155255306637548_zh-CN.png)![配置Policy](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/105886/155255306637550_zh-CN.png)

最后保存即可。

## 测试数据脱敏 {#section_z3g_4mg_mgb .section}

-   场景：

    用户 test 在 select 表 testdb1.testtbl 中列 a 的数据时，只显示最开始的 4 个字符。

-   流程：
    1.  配置 policy

        在上节的最后一个截图，其实就是配置了该场景的一个 policy，可参考上图\(其中脱敏方式选择了 show first 4\)。

    2.  脱敏验证

        test 用户使用 beeline 连接 HiveServer2，执行`select a from testdb1.testtbl`

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/105886/155255306737553_zh-CN.png)

        如上图所示，test 用户执行 select 命令后，列 a 显示的数据只有前面 4 个字符是正常显示，后面字符全部用`x`来脱敏处理。


