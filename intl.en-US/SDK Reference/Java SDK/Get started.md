# Get started {#concept_zvw_kmh_kfb .concept}

This section describes how to quickly create clusters, jobs, and execution plans by using Java SDKs.

**Note:** We recommend that you use [OpenAPI Explorer](https://api.aliyun.com) to call and generate SDK sample codes. OpenAPI Explorer allows you to call APIs of cloud services, dynamically generate SDK sample codes, and quickly retrieve interfaces.

## Prerequisites {#section_cx3_lmh_kfb .section}

You can create a Maven project and then add Maven dependencies as follows:

``` {#codeblock_y71_9t5_5a2}
<dependency>
            <groupId>com.aliyun</groupId>
            <artifactId>aliyun-java-sdk-core</artifactId>
            <version>2.3.9</version>
       </dependency>
       <dependency>
            <groupId>com.aliyun</groupId>
            <artifactId>aliyun-java-sdk-emr</artifactId>
            <version>2.2.2</version>
        </dependency>
```

You can also download associated JAR files to your local disk. Take Eclipse as an example. You can download JAR files as follows:

1.  Download the following JAR files:

    [aliyun-java-sdk-core-2.3.9.jar](http://search.maven.org/remotecontent?filepath=com/aliyun/aliyun-java-sdk-core/2.3.9/aliyun-java-sdk-core-2.3.9.jar)

    [aliyun-java-sdk-emr-2.2.2.jar](http://search.maven.org/remotecontent?filepath=com/aliyun/aliyun-java-sdk-emr/2.2.2/aliyun-java-sdk-emr-2.2.2.jar)

2.  Copy the JAR files to your project folder.
3.  In Eclipse, right-click your project name, and then select **Properties** \> **Java Build Path** \> **Add JARs**.
4.  Select all JAR files that you copied in Step 2.

Now, you can use Alibaba Cloud E-MapReduce Open API Java SDK for your Eclipse project.

## Initialize a client {#section_ak3_vmh_kfb .section}

``` {#codeblock_7pe_1ku_zd4}
IClientProfile profile = DefaultProfile.getProfile("cn-hangzhou", "<Your-AccessKeyId>", "<Your-AccessKeySecret>");
    DefaultAcsClient client = new DefaultAcsClient(profile);
```

All operations on the E-MapReduce in SDK can be performed using this client.

## Sample code {#section_m5v_hnh_kfb .section}

-   Clusters
    -   Create a cluster

        ``` {#codeblock_fvy_6cf_coe}
        public static void main(String[] args) {
              IClientProfile profile = DefaultProfile.getProfile("cn-hangzhou", "<Your-AccessKeyId>", "<Your-AccessKeySecret>");
              DefaultAcsClient client = new DefaultAcsClient(profile);
              final CreateClusterRequest request = new CreateClusterRequest();
              request.setName("Your-Cluster-Name");
              // if you did not specify security group id, it will create a new security group with given name
              request.setSecurityGroupId("Your-Security-Group-Id"); // (1)
              request.setAutoRenew(false);
              request.setChargeType("PostPaid"); // PostPaid or PrePaid
              request.setClusterType("HADOOP"); // HADOOP or HBase (2)
              request.setEmrVer("EMR-1.3.0"); // emr image version
              request.setIsOpenPublicIp(true);
              request.setLogEnable(true);
              request.setLogPath("oss://Your-Bucket/Your-Folder");
              request.setMasterPwdEnable(true); // enable master password
              request.setMasterPwd("Aa123456789"); // set master node's password
              request.setZoneId("cn-hangzhou-b"); // set zone
               // The I/O optimization parameters. The available hardware configurations, such as ECS instance types and cloud disk types are determined by the specified ECS instance series.
              // For more information about available configurations, see the Buy Now page of ECS.
              // https://ecs.console.aliyun.com/#/create/postpay/
              request.setIoOptimized(true); // You can specify I/O optimization parameters.
              request.setInstanceGeneration("ecs-2"); // You can specify ecs-2 as an ECS instance series. Valid values: ecs-1 and ecs-2.
              request.setNetType("classic"); // You can specify classic as a network type. Valid values: classic and vpc.
              List<CreateClusterRequest.EcsOrder> ecsOrders = new ArrayList<CreateClusterRequest.EcsOrder>();
              CreateClusterRequest.EcsOrder masterOrder = new CreateClusterRequest.EcsOrder();
              masterOrder.setIndex(1);
              masterOrder.setDiskCapacity(50);
              masterOrder.setDiskCount(2);
              masterOrder.setDiskType("CLOUD_EFFICIENCY"); // specify disk type (2)
              masterOrder.setInstanceType("ecs.n1.large"); // specify ecs instance type
              masterOrder.setNodeCount(1);
              masterOrder.setNodeType("MASTER"); // MASTER or CORE (2)
              ecsOrders.add(masterOrder);
              CreateClusterRequest.EcsOrder coreOrder = new CreateClusterRequest.EcsOrder();
              coreOrder.setIndex(2);
              coreOrder.setDiskCapacity(50);
              coreOrder.setDiskCount(4);
              coreOrder.setDiskType("CLOUD_EFFICIENCY");
              coreOrder.setInstanceType("ecs.n1.large");
              coreOrder.setNodeCount(3);
              coreOrder.setNodeType("CORE");
              ecsOrders.add(coreOrder);
              request.setEcsOrders(ecsOrders);
              try {
                  CreateClusterResponse response = client.getAcsResponse(request);
                  String clusterId = response.getClusterId(); // cluster id
                  // TODO do something with this cluster
              } catch (Exception e) {
                  // TODO do something
              }
          }
        ```

        -   When you create a cluster, you must specify a security group that hosts this cluster. If you did not specify the ID of a security group, then you must specify the name of this security group. You need to create a security group when you create a cluster.
        -   For more information, see [Enumeration](intl.en-US/API Reference/Enumerations.md#).
        -   The preceding code snippet creates a cluster in a classic network. If you want to create a cluster in a [VPC network](../../../../intl.en-US/Cluster Planning and Configurations/Configure clusters/VPC.md#), you need to specify vpc in the request method and specify vpcid and vswitchid as follows:

            ``` {#codeblock_bhn_g5x_fii}
            request.setNetType("vpc"); // You can specify vpc as a network type.
            request.setVpcId("your-vpcId");
            request.setVSwitchId("your-switchId");
            ```

        -   You can specify high availability parameters. For more information about high availability parameters, see the hardware configuration section of [Create a cluster](../../../../intl.en-US/Cluster Planning and Configurations/Configure clusters/Create a cluster.md#).

            ``` {#codeblock_2ii_fos_zmq}
            request.setHighAvailabilityEnable(true);
            ```

        -   You can specify available software components. For more information about available software components, see the software configuration section of [Create a cluster](../../../../intl.en-US/Cluster Planning and Configurations/Configure clusters/Create a cluster.md#).

            ``` {#codeblock_0mx_hrw_7hg}
            List<String> soft = new ArrayList<String>();
            soft.add("presto");
            soft.add("oozie");
            request.setOptionSoftWareLists(soft);
            ```

        -   You can specify a configuration item. For more information, click [here](../../../../intl.en-US/Cluster Planning and Configurations/Third-party software/Software configuration.md#).

            ``` {#codeblock_zr5_4eq_ayr}
            Request. setconfigurations ("Oss: // your-bucket/your-conf.json ");
            ```

        -   You can specify a bootstrap operation. For more information, click [here](../../../../intl.en-US/Cluster Planning and Configurations/Third-party software/Bootstrap actions.md#).

            ``` {#codeblock_tc2_ti0_jg6}
            List<CreateClusterRequest.BootstrapAction> bootstrapActionLists = new ArrayList<CreateClusterRequest.BootstrapAction>();
            CreateClusterRequest.BootstrapAction bootstrapActionList = new CreateClusterRequest.BootstrapAction();
            bootstrapActionList.setName("bootstrapName");
            bootstrapActionList.setPath("oss://emr-agent-pack/bootstrap/run-if.py");
            bootstrapActionList.setArg("instance.isMaster=true mkdir -p /tmp/abc");
            bootstrapActionLists.add(bootstrapActionList);
            request.setBootstrapActions(bootstrapActionLists);
            ```

    -   Query the details of a cluster

        ``` {#codeblock_mvx_5mz_v65}
        public static void main(String[] args) {
              IClientProfile profile = DefaultProfile.getProfile("cn-hangzhou", "<Your-AccessKeyId>", "<Your-AccessKeySecret>");
              DefaultAcsClient client = new DefaultAcsClient(profile);
              final DescribeClusterRequest request = new DescribeClusterRequest();
              request.setId("C-XXXXXXXXXXXXXXXX"); // cluster id
              try {
                  DescribeClusterResponse response = client.getAcsResponse(request);
                  DescribeClusterResponse.ClusterInfo clusterInfo = response.getClusterInfo();
                  // TODO do something with this cluster
              } catch (Exception e) {
                  // TODO do something
              }
          }
        ```

    -   Query the list of clusters

        ``` {#codeblock_gtq_d9w_lxg}
        public static void main(String[] args) {
                  IClientProfile profile = DefaultProfile.getProfile("cn-hangzhou", "<Your-AccessKeyId>", "<Your-AccessKeySecret>");
                  DefaultAcsClient client = new DefaultAcsClient(profile);
                  final ListClustersRequest request = new ListClustersRequest();
                  request.setPageNumber(1);
                  request.setIsDesc(true);
                  request.setPageSize(20);
                  try {
                      ListClustersResponse response = client.getAcsResponse(request);
                      List<ListClustersResponse.ClusterInfo> clusterInfos = response.getClusters();
                      for (ListClustersResponse.ClusterInfo clusterInfo : clusterInfos) {
                          // TODO do something with this cluster
                      }
                  } catch (Exception e) {
                      // TODO do something
                  }
              }
        ```

    -   Release a cluster

        ``` {#codeblock_y88_m9j_jx7}
        public static void main(String[] args) {
                    IClientProfile profile = DefaultProfile.getProfile("cn-hangzhou", "<Your AccessKeyId>", "<Your AccessKeySecret>");
                    DefaultAcsClient client = new DefaultAcsClient(profile);
                    ReleaseClusterRequest request = new ReleaseClusterRequest();
                    request.setId("C-XXXXXXXXXXXXXXXX"); // specify the cluster id you want to release.
                    try {
                        ReleaseClusterResponse response = client.getAcsResponse(request);
                    } catch (Exception e) {
                        // TODO do something
                    }
                }
        ```

-   Jobs
    -   Create a job

        ``` {#codeblock_n1o_0hn_1ci}
        public static void main(String[] args) {
              IClientProfile profile = DefaultProfile.getProfile("cn-hangzhou", "<Your-AccessKeyId>", "<Your-AccessKeySecret>");
              DefaultAcsClient client = new DefaultAcsClient(profile);
              final CreateJobRequest request = new CreateJobRequest();
              request.setName("Your-Job-Name");
              request.setRunParameter("--master yarn-client --driver-memory 4g --executor-memory 4g --executor-cores 2 --num-executors 4 --class com.test.RemoteDebug ossref://Your-Bucket/Resource.jar 1000\"");
              request.setFailAct("CONTINUE"); // STOP or CONTINUE
              request.setType("SPARK"); // SPARK or HADOOP or HIVE or PIG
        ```

        ``` {#codeblock_ssf_i16_7oy}
        try {
                    CreateJobResponse response = client.getAcsResponse(request);
                    String jobId = response.getId();
                    // TODO do something with this job
                } catch (Exception e) {
                    // TODO do something
                }
            }
        ```
        ```

    -   Delete a job

        **Note:** When a job is being used by an execution plan, you cannot delete this job. You must delete this execution plan or modify this execution plan before you delete this job.

        ``` {#codeblock_v3o_ju7_nhe}
        public static void main(String[] args) {
                  IClientProfile profile = DefaultProfile.getProfile("cn-hangzhou", "<Your-AccessKeyId>", "<Your-AccessKeySecret>");
                  DefaultAcsClient client = new DefaultAcsClient(profile);
                  final DeleteJobRequest request = new DeleteJobRequest();
                  request.setId("J-XXXXXXXXXXXXXXXX"); // set  job id
                  try {
                      DeleteJobResponse response = client.getAcsResponse(request);
                  } catch (Exception e) {
                      // TODO do something
                  }
              }
        ```

-   Execution plans
    -   Creating an execution plan

        ``` {#codeblock_1qr_ncm_0pt}
        public static void main(String[] args) {
                  IClientProfile profile = DefaultProfile.getProfile("cn-hangzhou", "<Your-AccessKeyId>", "<Your-AccessKeySecret>");
                  DefaultAcsClient client = new DefaultAcsClient(profile);
                  final CreateExecutionPlanRequest request = new CreateExecutionPlanRequest();
                  request.setName("Your-ExecutionPlan-Name");
                  request.setCreateClusterOnDemand(false);
                  request.setStrategy("RUN_MANUALLY"); // RUN_MANUALLY or SCHEDULE
                  request.setClusterId("C-XXXXXXXXXXXXXXXX"); // specify an existing running cluster
                  List<String> jobIds = new ArrayList<String>();
                  jobIds.add("J-XXXXXXXXXXXXXXXX"); // specify a job
                  request.setJobIdLists(jobIds);
                  try {
                      CreateExecutionPlanResponse response = client.getAcsResponse(request);
                      String executionPlanId = response.getId();
                      // TODO do something with this execution plan
                  } catch (Exception e) {
                      // TODO do something
                  }
              }
        ```

        The preceding code snippet creates an execution plan with the RUN\_MANUALLY type. In addition, an existing cluster is specified for this execution plan.

        If you want to create an execution plan with the SCHEDULE type, you must modify the previous code snippet and add the following code snippet:

        ``` {#codeblock_zgf_zzm_a4m}
        request.setStrategy("SCHEDULE"); // RUN_MANUALLY or SCHEDULE
                  request.setStartTime(new Date().getTime()); // set start time
                  request.setTimeUnit("DAY"); // DAY or HOUR
                  request.setTimeInterval(1); // set time interval
        ```

        If you want to create an execution plan for which a new cluster will be created to run jobs, you must modify the previous code snippet and add the following code snippet:

        ``` {#codeblock_rv3_eoa_ucz}
        request.setCreateClusterOnDemand(true);
                  request.setClusterType("HADOOP");
                  request.setClusterName("Your-Cluster-Name");
                  request.setEmrVer("EMR-1.3.0");
                  request.setSecurityGroupId("Your-Security-Group-Id");
                  request.setIsOpenPublicIp(true);
                   // The I/O optimization parameters. The available hardware configurations such as ECS instance types and cloud disk types are determined by the specified ECS instance series.
                  // For more information about how to select these configurations, see the Buy Now page of Elastic Compute Service.
                  // https://ecs.console.aliyun.com/#/create/postpay/
                  request.setIoOptimized(true); // You can specify true to enable I/O optimization.
                  request.setInstanceGeneration("ecs-2"); // You can specify esc-2 as an ECS instance series. Valid values: ecs-1 and ecs-2.
                  request.setNetType("classic"); // You can specify classic as a network type. Valid values: classic and vpc.
                  request.setLogEnable(true);
                  request.setLogPath("oss://xxx");
                  request.setEcsOrders(); // TODO you can refer to the configurations when you create a cluster. Note that the type of ecsOder is CreateExecutionPlanRequest.EcsOrder. The type of ecsOder here is different from CreateClusterRequest.EcsOrder.
        ```

        You can configure a cluster by specifying the previous parameters. For more information about these parameters, see the section Create a cluster. For an execution plan that requires a new cluster to run jobs, a temporary cluster will be created whenever you execute this execution plan. The temporary cluster is created based on the specified cluster configurations and then released after the execution plan is completed. Unlike the general process of creating a cluster, you must specify a security group ID rather than a security group name when you create a cluster in this case.

        The Create on demand option and the Periodic schedule option are not mutually exclusive. A new cluster is created when an execution plan starts at a scheduled time.

    -   Deleting an execution plan

        ``` {#codeblock_guk_psl_ykl}
        public static void main(String[] args) {
                  IClientProfile profile = DefaultProfile.getProfile("cn-hangzhou", "<Your-AccessKeyId>", "<Your-AccessKeySecret>");
                  DefaultAcsClient client = new DefaultAcsClient(profile);
                  final DeleteExecutionPlanRequest request = new DeleteExecutionPlanRequest();
                  request.setId("WF-XXXXXXXXXXXXXXXX"); // set execution plan id
                  try {
                      DeleteExecutionPlanResponse response = client.getAcsResponse(request);
                  } catch (Exception e) {
                      // TODO do something
                  }
              }
        ```

    -   Execute an execution plan

        **Note:** You cannot execute an execution plan that is running or being scheduled.

        ``` {#codeblock_6r3_xyd_g84}
        public static void main(String[] args) {
                  IClientProfile profile = DefaultProfile.getProfile("cn-hangzhou", "<Your-AccessKeyId>", "<Your-AccessKeySecret>");
                  DefaultAcsClient client = new DefaultAcsClient(profile);
                  RunExecutionPlanRequest request = new RunExecutionPlanRequest();
                  request.setId("WF-XXXXXXXXXXXXXXXX"); // specify the execution plan id which to run
                  try {
                      RunExecutionPlanResponse response = client.getAcsResponse(request);
                      String instanceId = response.getExecutionPlanInstanceId();
                      // TODO do something with this instance
                  } catch (Exception e) {
                      // TODO do something
                  }
              }
        ```

    -   Suspend a scheduled execution plan

        For an execution plan that will be executed periodically , you can suspend this execution plan by calling a method provided by the SDK as follows:

        ``` {#codeblock_y0e_du8_tvd}
        public static void main(String[] args) {
                  IClientProfile clientProfile = DefaultProfile.GetProfile("cn-shanghai", "<your-access-key-id>", "<your-access-key-secret>");
                  DefaultAcsClient client = new DefaultAcsClient(profile);
                  SuspendExecutionPlanSchedulerRequest request = new SuspendExecutionPlanSchedulerRequest();
                  request.setId("WF-XXXXXXXXXXXXXXXX"); // specify the execution plan id you want to suspend
                  try {
                      SuspendExecutionPlanSchedulerResponse response = client.getAcsResponse(request);
                  } catch (Exception e) {
                      // TODO do something
                  }
              }
        ```

    -   Resume an execution plan

        For an execution plan that will be executed periodically , you can resume this execution plan by calling a method provided by the SDK as follows:

        ``` {#codeblock_eo5_owf_vgt}
        public static void main(String[] args) {
                  IClientProfile profile = DefaultProfile.getProfile("cn-hangzhou", "<Your AccessKeyId>", "<Your AccessKeySecret>");
                  DefaultAcsClient client = new DefaultAcsClient(profile);
                  ResumeExecutionPlanSchedulerRequest request = new ResumeExecutionPlanSchedulerRequest();
                  request.setId("WF-XXXXXXXXXXXXXXXX"); // specify the execution plan id you want to suspend
                  try {
                      ResumeExecutionPlanSchedulerResponse response = client.getAcsResponse(request);
                  } catch (Exception e) {
                      // TODO do something
                  }
              }
        ```

    -   Query the running logs of an execution plan

        ``` {#codeblock_gwe_jpq_5hg}
        public static void main(String[] args) {
                  IClientProfile profile = DefaultProfile.getProfile("cn-hangzhou", "<Your AccessKeyId>", "<Your AccessKeySecret>");
                  DefaultAcsClient client = new DefaultAcsClient(profile);
                  ListExecutionPlanInstancesRequest request = new ListExecutionPlanInstancesRequest();
                  // specify execution plan ids
                  List<String> executionPlanIds = new ArrayList<String>();
                  executionPlanIds.add("WF-XXXXXXXXXXXXXXX1");
                  executionPlanIds.add("WF-XXXXXXXXXXXXXXX2");
                  executionPlanIds.add("WF-XXXXXXXXXXXXXXX3");
                  request.setExecutionPlanIdLists(executionPlanIds); // (1)
                  // specify order key (ordered by id)
                  request.setIsDesc(true);
                  // specify page number and page size, default page number is 1 and default page size is 10.
                  request.setPageSize(20);
                  request.setPageNumber(1);
                  // specify if you want to list latest instance for each execution plan id.
                  request.setOnlyLastInstance(true); // (2) default is false
                  try {
                      ListExecutionPlanInstancesResponse response = client.getAcsResponse(request);
                      for (ListExecutionPlanInstancesResponse.ExecutionPlanInstance instance : response.getExecutionPlanInstances()) {
                          // TODO do something with each instance
                      }
                  } catch (Exception e) {
                      // TODO do something
                  }
              }
        ```

        -   You can specify multiple execution plan IDs to query the running logs.
        -   If you query the last running log, only the last running log of a specified execution plan will be returned. You can use this feature to check whether the last execution plan has been executed successfully, or query the last running log of an execution plan.

