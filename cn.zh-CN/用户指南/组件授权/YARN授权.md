# YARN授权 {#concept_vz5_4zv_1fb .concept}

YARN的授权根据授权实体，可以分为服务级别的授权、队列级别的授权。

## 服务级别的授权 {#section_efr_j1w_1fb .section}

详见[Hadoop官方文档](https://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/ServiceLevelAuth.html)

-   控制特定用户访问集群服务，如提交作业
-   配置在hadoop-policy.xml
-   服务级别的权限校验在其他权限校验之前\(如HDFS的permission检查/yarn提交作业到队列控制\)

**说明：** 一般设置了HDFS permission检查/yarn队列资源控制，可以不设置服务级别的授权控制，用户可以根据自己需求进行相关配置。

## 队列级别的授权 {#section_mzq_dcw_1fb .section}

YARN可以通过队列对资源进行授权管理，有两种队列调度 Capacity Scheduler和Fair Scheduler。

这里以Capacity Scheduler为例。

-   添加配置

    队列也有两个级别的授权，一个是提交作业到队列的授权，一个是管理队列的授权

    **说明：** 

    -   队列的ACL的控制对象为user/group，设置相关参数时，user和group可同时设置，中间用空格分开，user/group内部可用逗号分开，只有一个空格表示任何人都没有权限。
    -   队列ACL继承: 如果一个user/group可以向某个队列中提交应用程序，则它可以向它的所有子队列中提交应用程序，同理管理队列的ACL也具有继承性。所以如果要防止某个user/group提交作业到某个队列，则需要设置该队列以及该队列的所有父队列的ACL来限制该user/group的提交作业的权限。
    -   yarn.acl.enable

        ACL开关，设置为true

    -   yarn.admin.acl
        -   yarn的管理员设置，如可执行`yarn rmadmin/yarn kill`等命令，该值必须配置，否则后续的队列相关的acl管理员设置无法生效。
        -   如上备注，配置值时可以设置user/group：

            ```
            user1,user2 group1,group2 #user和group用空格隔开
              group1,group2 #只有group情况下，必须在最前面加上空格
            ```

            EMR集群中需将has配置为admin的acl权限

    -   yarn.scheduler.capacity.$\{queue-name\}.acl\_submit\_applications
        -   设置能够向该队列提交的user/group。
        -   其中$\{queue-name\}为队列的名称，可以是多级队列，注意多级情况下的ACL继承机制。如:

            ```
            #queue-name=root
              <property>
                  <name>yarn.scheduler.capacity.root.acl_submit_applications</name>
                  <value> </value> #空格表示任何人都无法往root队列提交作业
              </property>
             #queue-name=root.testqueue
             <property>
               <name>yarn.scheduler.capacity.root.testqueue.acl_submit_applications</name>
                  <value>test testgrp</value> #testqueue只允许test用户/testgrp组提交作业
              </property>
            ```

    -   yarn.scheduler.capacity.$\{queue-name\}.acl\_administer\_queue
        -   设置某些user/group管理队列，比如kill队列中作业等。
        -   queue-name可以是多级，注意多级情况下的ACL继承机制。

            ```
            #queue-name=root
              <property>
                  <name>yarn.scheduler.capacity.root.acl_administer_queue</name>
                  <value> </value>
              </property>
             #queue-name=root.testqueue
             <property>
               <name>yarn.scheduler.capacity.root.testqueue.acl_administer_queue</name>
                  <value>test testgrp</value>
              </property>
            ```

-   重启YARN服务
    -   对于Kerberos安全集群已经默认开启ACL，用户可以根据自己需求配置队列的相关ACL权限控制。
    -   对于非Kerberos安全集群根据上述开启ACL并配置好队列的权限控制，重启YARN服务。
-   配置示例

    -   yarn-site.xml

        ```
        <property>
                <name>yarn.acl.enable</name>
                <value>true</value>
         </property>
        <property>
                <name>yarn.admin.acl</name>
                <value>has</value>
         </property>
        ```

    -   capacity-scheduler.xml
    -   default队列: 禁用default队列，不允许任何用户提交或管理。
    -   q1队列: 只允许test用户提交作业以及管理队列\(如kill\)。
    -   q2队列: 只允许foo用户提交作业以及管理队列。
    ```
    <configuration>
        <property>
            <name>yarn.scheduler.capacity.maximum-applications</name>
            <value>10000</value>
            <description>Maximum number of applications that can be pending and running.</description>
        </property>
        <property>
            <name>yarn.scheduler.capacity.maximum-am-resource-percent</name>
            <value>0.25</value>
            <description>Maximum percent of resources in the cluster which can be used to run application masters i.e.
                controls number of concurrent running applications.
            </description>
        </property>
        <property>
            <name>yarn.scheduler.capacity.resource-calculator</name>
            <value>org.apache.hadoop.yarn.util.resource.DefaultResourceCalculator</value>
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.queues</name>
            <value>default,q1,q2</value>
            <!-- 3个队列-->
            <description>The queues at the this level (root is the root queue).</description>
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.default.capacity</name>
            <value>0</value>
            <description>Default queue target capacity.</description>
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.default.user-limit-factor</name>
            <value>1</value>
            <description>Default queue user limit a percentage from 0.0 to 1.0.</description>
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.default.maximum-capacity</name>
            <value>100</value>
            <description>The maximum capacity of the default queue.</description>
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.default.state</name>
            <value>STOPPED</value>
            <!-- default队列状态设置为STOPPED-->
            <description>The state of the default queue. State can be one of RUNNING or STOPPED.</description>
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.default.acl_submit_applications</name>
            <value> </value>
            <!-- default队列禁止提交作业-->
            <description>The ACL of who can submit jobs to the default queue.</description>
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.default.acl_administer_queue</name>
            <value> </value>
            <!-- 禁止管理default队列-->
            <description>The ACL of who can administer jobs on the default queue.</description>
        </property>
        <property>
            <name>yarn.scheduler.capacity.node-locality-delay</name>
            <value>40</value>
        </property>
        <property>
            <name>yarn.scheduler.capacity.queue-mappings</name>
            <value>u:test:q1,u:foo:q2</value>
            <!-- 队列映射，test用户自动映射到q1队列-->
            <description>A list of mappings that will be used to assign jobs to queues. The syntax for this list is
                [u|g]:[name]:[queue_name][,next mapping]* Typically this list will be used to map users to queues,for
                example, u:%user:%user maps all users to queues with the same name as the user.
            </description>
        </property>
        <property>
            <name>yarn.scheduler.capacity.queue-mappings-override.enable</name>
            <value>true</value>
            <!-- 上述queue-mappings设置的映射，是否覆盖客户端设置的队列参数-->
            <description>If a queue mapping is present, will it override the value specified by the user? This can be used
                by administrators to place jobs in queues that are different than the one specified by the user. The default
                is false.
            </description>
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.acl_submit_applications</name>
            <value> </value>
            <!-- ACL继承性，父队列需控制住权限-->
            <description>
                The ACL of who can submit jobs to the root queue.
            </description>
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.q1.acl_submit_applications</name>
            <value>test</value>
            <!-- q1只允许test用户提交作业-->
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.q2.acl_submit_applications</name>
            <value>foo</value>
            <!-- q2只允许foo用户提交作业-->
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.q1.maximum-capacity</name>
            <value>100</value>
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.q2.maximum-capacity</name>
            <value>100</value>
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.q1.capacity</name>
            <value>50</value>
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.q2.capacity</name>
            <value>50</value>
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.acl_administer_queue</name>
            <value> </value>
            <!-- ACL继承性，父队列需控制住权限-->
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.q1.acl_administer_queue</name>
            <value>test</value>
            <!-- q1队列只允许test用户管理，如kill作业-->
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.q2.acl_administer_queue</name>
            <value>foo</value>
            <!-- q2队列只允许foo用户管理，如kill作业-->
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.q1.state</name>
            <value>RUNNING</value>
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.q2.state</name>
            <value>RUNNING</value>
        </property>
    </configuration>
    ```


