# 使用JindoFS SDK免密功能

本文介绍使用JindoFS SDK时，E-MapReduce（简称EMR）集群外如何以免密方式访问E-MapReduce JindoFS的文件系统。

适用环境：ECS（EMR环境外）+Hadoop+JavaSDK。

使用JindoFS SDK时，需要把环境中相关Jindo的包从环境中移除，如jboot.jar、smartdata-aliyun-jfs-\*.jar。如果要使用Spark则需要把/opt/apps/spark-current/jars/里面的包也删除，从而可以正常使用。

## 步骤一：创建实例RAM角色

1.  使用云账号登录[RAM的控制台](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.6.77bd72fe3PD5pf#/policy/detail/system/AliyunEMRRolePolicy/info)。

2.  单击左侧导航栏的**RAM角色管理**。

3.  单击**创建 RAM 角色**，选择当前可信实体类型为**阿里云服务**。

4.  单击**下一步**。

5.  输入**角色名称**，从**选择授信服务**列表中，选择**云服务器**。

6.  单击**完成**。


## 步骤二：为RAM角色授予权限

1.  使用云账号登录[RAM的控制台](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.6.77bd72fe3PD5pf#/policy/detail/system/AliyunEMRRolePolicy/info)。

2.  （可选）如果您不使用系统权限，可以参见[账号访问控制](/cn.zh-CN/安全/账号访问控制.md)创建自定义权限策略章节创建一个自定义策略。

3.  单击左侧导航栏的**RAM角色管理**。

4.  单击新创建RAM角色名称所在行的**精确授权**。

5.  选择权限类型为**系统策略**或**自定义策略**。

6.  输入策略名称。

7.  单击**确定**。


## 步骤三：为实例授予RAM角色

1.  登录[ECS管理控制台](https://ecs.console.aliyun.com)。

2.  在左侧导航栏，单击**实例与镜像** \> **实例**。

3.  在顶部状态栏左上角处，选择地域。

4.  找到要操作的ECS实例，选择**更多** \> **实例设置** \> **授予/收回RAM角色**。

    ![授予/收回RAM角色](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0257459951/p53160.png)

5.  在弹窗中，选择创建好的实例RAM角色，单击**确定**完成授予。


## 步骤四：在ECS上设置环境变量

执行如下命令，在ECS上设置环境变量。

```
export CLASSPATH=/xx/xx/jindofs-2.5.0-sdk.jar
```

或者执行如下命令。

```
HADOOP_CLASSPATH=$HADOOP_CLASSPATH:/xx/xx/jindofs-2.5.0-sdk.jar
```

## 步骤五：测试免密方式访问的方法

1.  使用Shell访问OSS。

    ```
    hdfs dfs -ls/-mkdir/-put/....... oss://<ossPath>
    ```

2.  使用Hadoop FileSystem访问OSS。

    JindoFS SDK支持使用Hadoop FileSystem访问OSS，示例代码如下。

    ```
    import org.apache.hadoop.conf.Configuration;
    import org.apache.hadoop.fs.FileSystem;
    import org.apache.hadoop.fs.LocatedFileStatus;
    import org.apache.hadoop.fs.Path;
    import org.apache.hadoop.fs.RemoteIterator;
    
    import java.net.URI;
    
    public class test {
        public static void main(String[] args) throws Exception {
            FileSystem fs = FileSystem.get(new URI("ossPath"), new Configuration());
            RemoteIterator<LocatedFileStatus> iterator = fs.listFiles(new Path("ossPath"), false);
            while (iterator.hasNext()){
                LocatedFileStatus fileStatus = iterator.next();
                Path fullPath = fileStatus.getPath();
                System.out.println(fullPath);
            }
        }
    }
                                
    ```


