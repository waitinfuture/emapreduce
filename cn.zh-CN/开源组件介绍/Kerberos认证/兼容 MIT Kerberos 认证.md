# 兼容 MIT Kerberos 认证 {#concept_qk1_5pl_z2b .concept}

本文将通过 HDFS 服务介绍兼容 MIT Kerberos 认证流程。

## 兼容 MIT Kerberos 的身份认证方式 {#section_an3_fql_z2b .section}

EMR 集群中 Kerberos 服务端启动在 master 节点，涉及一些管理操作需在 master 节点（emr-header-1）的 root 账号执行。

下面以 test 用户访问 HDFS 服务为例介绍相关流程。

-   Gateway 上执行`hadoop fs -ls /` 
    -   配置 krb5.conf

        ``` {#codeblock_zyx_xmo_tsd}
        Gateway 上面使用root账号
         scp root@emr-header-1:/etc/krb5.conf /etc/
        ```

    -   添加 principal
        -   登录集群 emr-header-1 节点（必须是 header-1，HA 不能在 header-2 上操作），切换到 root 账号。
        -   进入 Kerberos 的 admin 工具。
        -   ``` {#codeblock_x3a_buk_vib}
  sh /usr/lib/has-current/bin/hadmin-local.sh /etc/ecm/has-conf -k /etc/ecm/has-conf/admin.keytab
  HadminLocalTool.local: #直接按回车可以看到一些命令的用法
  HadminLocalTool.local: addprinc #输入命令按回车可以看到具体命令的用法
  HadminLocalTool.local: addprinc -pw 123456 test #添加test的princippal，密码设置为123456
```

    -   导出 keytab 文件

        登录集群 emr-header-1（必须是 header-1，HA 不能在header-2操作），导入 keytab 文件。

        使用 Kerberos 的 admin 工具可以导出 principal 对应的 keytab 文件。

        ``` {#codeblock_lct_ihg_zis}
        HadminLocalTool.local: ktadd -k /root/test.keytab test #导出keytab文件,后续可使用该文件
        ```

    -   kinit 获取 Ticket

        在执行 hdfs 命令的客户端机器上面，如 Gateway。

        -   添加 linux 账号 test

            ``` {#codeblock_kze_pel_jnd}
            useradd test
            ```

        -   安装 MITKerberos 客户端工具。

            可以使用 MITKerberos tools 进行相关操作（如 kinit/klist 等），详细使用方式参见 [MITKerberos](https://web.mit.edu/kerberos/krb5-1.12/doc/user/user_commands/kinit.html) 文档

             `yum install krb5-libs krb5-workstation -y`

        -   切到 test 账号执行 kinit

            ``` {#codeblock_6xr_nsc_cna}
                  su test
                  #如果没有 keytab文件,则执行
                  kinit #直接回车
                  Password for test: 123456 #即可
                  #如有有keytab文件,也可执行
                  kinit -kt test.keytab test    
                  #查看ticket
                  klist
            ```

            **说明：** MITKerberos 工具使用实例

            ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17935/156203404711126_zh-CN.png)

    -   导入环境变量生效，执行：

        ``` {#codeblock_746_etk_i99}
        export HADOOP_CONF_DIR=/etc/has/hadoop-conf
        ```

    -   执行 hdfs 命令

        获取到 Ticket 后，就可以正常执行 hdfs 命令了。

        ``` {#codeblock_bkh_nb5_zdx}
        hadoop fs -ls /
              Found 5 items
             drwxr-xr-x     - hadoop hadoop          0 2017-11-12 14:23 /apps
             drwx------      - hbase  hadoop           0 2017-11-15 19:40 /hbase
             drwxrwx--t+   - hadoop hadoop         0 2017-11-15 17:51 /spark-history
             drwxrwxrwt   - hadoop hadoop          0 2017-11-13 23:25 /tmp
             drwxr-x--t      - hadoop hadoop          0 2017-11-13 16:12 /user
        ```

        **说明：** 跑 yarn 作业，需要提前在集群中所有节点添加对应的 linux 账号（详见下文中\[[EMR集群添加test账号](intl.zh-CN/开源组件介绍/Kerberos认证/RAM 认证.md#ul_myx_vc5_1fb)\]）。

-   java 代码访问 HDFS
    -   使用本地 ticket cache

        **说明：** 需要提前执行 kinit 获取 ticket，且 ticket 过期后程序会访问异常。

        ``` {#codeblock_rjt_oa3_01e}
        public static void main(String[] args) throws IOException {
           Configuration conf = new Configuration();
           //加载hdfs的配置,配置从emr集群上复制一份
           conf.addResource(new Path("/etc/ecm/hadoop-conf/hdfs-site.xml"));
           conf.addResource(new Path("/etc/ecm/hadoop-conf/core-site.xml"));
           //需要在程序所在linux账号下,提前kinit获取ticket
           UserGroupInformation.setConfiguration(conf);
           UserGroupInformation.loginUserFromSubject(null);
           FileSystem fs = FileSystem.get(conf);
           FileStatus[] fsStatus = fs.listStatus(new Path("/"));
           for(int i = 0; i < fsStatus.length; i++){
               System.out.println(fsStatus[i].getPath().toString());
           }
        }
        ```

    -   使用 keytab 文件\(推荐\)

        **说明：** keytab 长期有效，跟本地 ticket 无关。

        ``` {#codeblock_dzz_nkr_thm}
        public static void main(String[] args) throws IOException {
          String keytab = args[0];
          String principal = args[1];
          Configuration conf = new Configuration();
          //加载 hdfs 的配置，配置从 emr 集群上复制一份
          conf.addResource(new Path("/etc/ecm/hadoop-conf/hdfs-site.xml"));
          conf.addResource(new Path("/etc/ecm/hadoop-conf/core-site.xml"));
          //直接使用 keytab 文件,该文件从 emr 集群 master-1 上面执行相关命令获取[文档前面有介绍命令]
          UserGroupInformation.setConfiguration(conf);
          UserGroupInformation.loginUserFromKeytab(principal, keytab);
          FileSystem fs = FileSystem.get(conf);
          FileStatus[] fsStatus = fs.listStatus(new Path("/"));
          for(int i = 0; i < fsStatus.length; i++){
              System.out.println(fsStatus[i].getPath().toString());
          }
          }
        ```

        附 pom 依赖：

        ``` {#codeblock_g4k_d1y_3p4}
        <dependencies>
                    <dependency>
                        <groupId>org.apache.hadoop</groupId>
                        <artifactId>hadoop-common</artifactId>
                        <version>2.7.2</version>
                    </dependency>
                    <dependency>
                        <groupId>org.apache.hadoop</groupId>
                        <artifactId>hadoop-hdfs</artifactId>
                        <version>2.7.2</version>
                    </dependency>
                </dependencies>
        ```


