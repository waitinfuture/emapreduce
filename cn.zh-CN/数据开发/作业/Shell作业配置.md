# Shell作业配置

本文介绍如何配置Shell类型的作业。

已创建好项目，详情请参见[项目管理](/cn.zh-CN/数据开发/项目管理.md)。

## 操作步骤

1.  通过主账号登录[阿里云E-MapReduce控制台](https://emr.console.aliyun.com)。

2.  在顶部菜单栏处，根据实际情况选择地域（Region）和资源组。

3.  单击上方的**数据开发**页签。

4.  在**项目列表**页面，单击待编辑项目所在行的**作业编辑**。

5.  在**作业编辑**区域，在需要操作的文件夹上，右键选择**新建作业**。

6.  输入**作业名称**、**作业描述**，在**作业类型**下拉列表中选择**Shell**作业类型。

    表示创建的作业是一个Bash Shell作业。

7.  单击**确定**。

8.  在**作业内容**中，填写提交该作业需要提供的命令行参数。

    示例如下。

    ```
    DD=`date`;
    echo "hello world, $DD"
    ```

9.  单击**保存**，作业配置即定义完成。


## 问题反馈

如果您在使用阿里云E-MapReduce过程中有任何疑问，欢迎您扫描下面的二维码加入钉钉群进行反馈。

![emr_dingding](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2440659951/p81620.png)

