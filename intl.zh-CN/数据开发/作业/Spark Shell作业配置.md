# Spark Shell作业配置

本文介绍如何配置Spark Shell类型的作业。

已创建好项目，详情请参见[项目管理](/intl.zh-CN/数据开发/项目管理.md)。

## 操作步骤

1.  通过主账号登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com)。

2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

3.  单击上方的**数据开发**页签。

4.  在**项目列表**页面，单击待编辑项目所在行的**作业编辑**。

5.  在**作业编辑**区域，在需要操作的文件夹上，右键选择**新建作业**。

6.  输入**作业名称**和**作业描述**，在**作业类型**下拉列表中选择**Spark Shell**作业类型。

7.  单击**确定**。

8.  在**作业内容**中，输入Spark Shell命令后续的参数。

    示例如下。

    ```
    val count = sc.parallelize(1 to 100).filter { _ =>
      val x = math.random
      val y = math.random
      x*x + y*y < 1
    }.count()println(s"Pi is roughly ${4.0 * count / 100}")
    ```

9.  单击**保存**，作业配置即定义完成。


