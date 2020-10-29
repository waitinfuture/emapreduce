# Use the password-free feature of JindoFS SDK

This topic describes how to access JindoFS in password-free mode from an ECS instance \(not an instance in EMR clusters\) when you use JindoFS SDK.

1. You have ECS instances other than those for EMR. 2. The Hadoop ecosystem is used. 3. JindoFS SDK for Java is obtained.

Before you can use JindoFS SDK, you must remove packages related to Jindo from your environment, such as jboot.jar and smartdata-aliyun-jfs-\*.jar. To use Spark, you must also remove the packages in the /opt/apps/spark-current/jars/ directory.

## Step 1: Create an instance RAM role

1.  Log on to the [RAM console](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.6.77bd72fe3PD5pf#/policy/detail/system/AliyunEMRRolePolicy/info) with an Alibaba Cloud account.

2.  In the left-side navigation pane, click **RAM Roles**.

3.  On the RAM Roles page that appears, click **Create RAM Role**. In the pane that appears, select **Alibaba Cloud Service** for Trusted entity type.

4.  Click **Next**.

5.  Enter a role name in the **RAM Role Name** field and select **Elastic Compute Service** from the **Select Trusted Service** drop-down list.

6.  Click **OK**.


## Step 2: Grant permissions to the RAM role

1.  Log on to the [RAM console](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.6.77bd72fe3PD5pf#/policy/detail/system/AliyunEMRRolePolicy/info) with an Alibaba Cloud account.

2.  Optional. If you do not use system permissions, you can create custom permissions. For more information, see the "\(Optional\) Create a custom authorization policy" section in [Implement access control by using RAM](/intl.en-US/Security/Implement access control by using RAM.md).

3.  In the left-side navigation pane, click **RAM Roles**.

4.  Find the created RAM role and click **Input and Attach** in the Actions column.

5.  In the pane that appears, select **System Policy** or **Custom Policy** for Type.

6.  Enter the policy name.

7.  Click **OK**.


## Step 3: Bind the RAM role to an ECS instance

1.  Log on to the [ECS console](https://ecs.console.aliyun.com).

2.  In the left-side navigation pane, choose **Instances & Images** \> **Instances**.

3.  In the top navigation bar, select a region.

4.  Find the ECS instance to which you want to bind the RAM role and choose **More** \> **Instance Settings** \> **Bind/Unbind RAM Role** in the Actions column.

    ![Bind/Unbind RAM Role](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/en-US/2834129951/p53160.png)

5.  In the Bind/Unbind RAM Role dialog box, select the RAM role and click **OK**.


## Step 4: Configure environment variables on ECS

Run one of the following commands to configure environment variables on ECS:

```
export CLASSPATH=/xx/xx/jindofs-2.5.0-sdk.jar
```

```
HADOOP_CLASSPATH=$HADOOP_CLASSPATH:/xx/xx/jindofs-2.5.0-sdk.jar
```

## Step 5: Access JindoFS in password-free mode

1.  Use Shell to access OSS.

    ```
    hdfs dfs -ls/-mkdir/-put/....... oss://<ossPath>
    ```

2.  Access OSS by using the FileSystem interface of Hadoop.

    Example:

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


