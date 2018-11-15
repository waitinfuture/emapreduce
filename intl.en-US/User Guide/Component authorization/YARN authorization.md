# YARN authorization {#concept_vz5_4zv_1fb .concept}

YARN authorization can be divided to service-level authorization and queue-level authorization based on the authorization entity.

## Service-level authorization {#section_efr_j1w_1fb .section}

For more information, see [Hadoop official documentation](https://hadoop.apache.org/docs/r2.7.2/hadoop-project-dist/hadoop-common/ServiceLevelAuth.html).

-   Control cluster service access by specific users, such as submitting jobs
-   Configures hadoop-policy.xml
-   Service level permission validation has a higher priority than other permission validation procedures \(such as HDFS permission verification and YARN job submission control\)

**Note:** Generally, if HDFS permission verification and YARN job submission control have been set up, you may choose not to set the service level permission control. You can perform the relevant configurations as needed.

## Queue-level authorization {#section_mzq_dcw_1fb .section}

YARN supports permission control over resources through queues, and it provides two queue scheduling methods, namely Capacity Scheduler and Fair Scheduler. We will take the Capacity Scheduler as an example here.

-   Add a configuration

    A queue also has a two-level authorization: the authorization for job submission \(submitting a job to the queue\) and the authorization for queue management.

    **Note:** 

    -   The ACL control object for a queue is user/group. The users and groups can be set at the same time \(separated by spaces\) when you set up the relevant parameters. You can use a comma to separate different users and groups. Using only one space means that no one has the permission.
    -   ACL inheritance for a queue: If a user/group can submit an application to a queue, then this user/group can submit applications to all sub-queues of this queue. Likewise, the ACL that manages queues can also be inherited. Therefore, if you want to prevent a user/group from submitting jobs to a queue, you must set the ACL for this queue and all its parent queues to restrict the job-submission permission of this user/group.
    -   yarn.acl.enable

        ACL switch, set to true

    -   yarn.admin.acl
        -   YARN administrator setting, which enables/disables executing `yarn rmadmin/yarn kill`, and other commands. This value must be configured, otherwise, the subsequent queue-based ACL administrator settings cannot take effect.
        -   As mentioned in the preceding note, you can set up user/group when setting up the values:

            ```
            user1,user2 group1,group2 #users and groups are separated by a space
              group1,group2 #In case there are only groups, a leading space is required.
            ```

            In an EMR cluster, you must configure the ACL permission for has as admin.

    -   yarn.scheduler.capacity.$\{queue-name\}.acl\_submit\_applications
        -   Set user/group that can submit jobs to this queue
        -   where $\{queue-name\} is the queue name. Multi-level queues are supported. Note that ACL is inherited in multi-level queues, for example:

            ```
            #queue-name=root
              <property>
                  <name>yarn.scheduler.capacity.root.acl_submit_applications</name>
                  <value> </value> # Space means no one can submit jobs to the root queue
              </property>
             #queue-name=root.testqueue
             <property>
               <name>yarn.scheduler.capacity.root.testqueue.acl_submit_applications</name>
                  <value>test testgrp</value> #testqueue only allows the test user and testgrp group to submit jobs
              </property>
            ```

    -   yarn.scheduler.capacity.$\{queue-name\}.acl\_administer\_queue
        -   Set some user/group to manage the queue, such as killing a job in the queue.
        -   Multi-level queue-names are supported. Note that ACL is inherited in multi-level queues.

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

-   Restart the YARN service
    -   The Kerberos secure cluster has enabled ACL by default. You can configure the relevant ACL permissions for queues as needed.
    -   For a non-Kerberos secure cluster, enable ACL and configure the permission control for queues in accordance with the preceding instructions, and then restart the YARN service.
-   Configuration example

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
    -   Default queue: disables the default queue and do not allow any user to submit jobs or manage the queue.
    -   Q1 queue: only allows the test user to submit jobs and manage the queue \(such as killing the jobs\).
    -   Q2 queue: only allows the foo user to submit jobs and manage the queue. Q2 Queues: Only Foo users are allowed to submit jobs and manage queues.
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
            <! --3 queues->
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
            <! -- Status of the default queue is set as STOPPED-->
            <description>The state of the default queue. State can be one of RUNNING or STOPPED.</description>
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.default.acl_submit_applications</name>
            <value> </value>
            <! -- The default queue does not allow job submission-->
            <description>The ACL of who can submit jobs to the default queue.</description>
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.default.acl_administer_queue</name>
            <value> </value>
            <! -- Prevent users/groups to manage the default queue-->
            <description>The ACL of who can administer jobs on the default queue.</description>
        </property>
        <property>
            <name>yarn.scheduler.capacity.node-locality-delay</name>
            <value>40</value>
        </property>
        <property>
            <name>yarn.scheduler.capacity.queue-mappings</name>
            <value>u:test:q1,u:foo:q2</value>
            <! -- Queue mapping, automatically maps the test user to Q1 queue-->
            <description>A list of mappings that will be used to assign jobs to queues. The syntax for this list is
                [u|g]:[name]:[queue_name][,next mapping]* Typically this list will be used to map users to queues,for
                example, u:%user:%user maps all users to queues with the same name as the user.
            </description>
        </property>
        <property>
            <name>yarn.scheduler.capacity.queue-mappings-override.enable</name>
            <value>true</value>
            <! -- Whether or not allow the above queue-mapping to overwrite the queue parameters set up by the client-->
            <description>If a queue mapping is present, will it override the value specified by the user? This can be used
                by administrators to place jobs in queues that are different than the one specified by the user. The default
                is false.
            </description>
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.acl_submit_applications</name>
            <value> </value>
            <! -- ACL inheritance, the parent queue must have the admin permissions-->
            <description>
                The ACL of who can submit jobs to the root queue.
            </description>
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.q1.acl_submit_applications</name>
            <value>test</value>
            <! -- q1 only allows the test user to submit jobs-->
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.q2.acl_submit_applications</name>
            <value>foo</value>
            <! -- q2 only allows the foo user to submit jobs-->
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
            <! -- ACL inheritance, the parent queue must have the admin permissions-->
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.q1.acl_administer_queue</name>
            <value>test</value>
            <! -- q1 only allow the test user to manage the queue, such as killing the jobs-->
        </property>
        <property>
            <name>yarn.scheduler.capacity.root.q2.acl_administer_queue</name>
            <value>foo</value>
            <! -- q2 only allow the foo user to manage the queue, such as killing the jobs-->
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


