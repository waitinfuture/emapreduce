# Isolate OSS data of different users {#concept_jrw_vnl_z2b .concept}

This topic describes how to use Resource Access Management \(RAM\) to isolate Object Storage Service \(OSS\) data of different users.

## Prerequisite {#section_xk8_xsb_b6x .section}

An Alibaba Cloud account is created.

## Background {#section_dcf_jpl_z2b .section}

E-MapReduce allows you to use RAM to isolate data of different users.

## Step 1: Log on to the RAM console {#section_wen_dqp_m4b .section}

1.  Log on to the [RAM console](https://ram.console.aliyun.com/) by using an Alibaba Cloud account.

## Step 2: Create a RAM user {#section_scp_1y3_lp2 .section}

1.  In the left-side navigation pane, click **Identities**, and click **Users**.
2.  Click **Create User**.

    **Note:** To create multiple RAM users at a time, click **Add User**.

3.  Specify the **Logon Name** and **Display Name** parameters.
4.  Under **Access Mode**, select **Console Password Logon** or **Programmatic Access**.

    **Note:** We recommend that you select only one access mode for the RAM users to ensure the security of your Alibaba Cloud account. This prevents RAM users who have terminated their employment contracts with the company from accessing Alibaba Cloud resources.

5.  Click **OK**.

## Step 3: Create permission policies {#section_fzt_gus_48m .section}

In addition to providing the default permission policies, RAM allows you to customize permission policies for flexible authorization. You can create multiple permission policies based on your needs.

1.  In the left-side navigation pane, click **Permissions**, and click **Policies**.
2.  On the page that appears, click **Create Policy**.
3.  On the Create Custom Policy page, specify the **Policy Name** and **Note** parameters.
4.  Select **Script** in **Configuration Mode**.

    For more information about how to configure a permission policy in **script** mode, see the [permission policy syntax and structure](../../intl.en-US/User Guide/Policies/Policy language/Policy structure and grammar.md#). In the following example, two permission policies are created in script mode:

    |Test environment \(test-bucket\)|Production environment \(prod-bucket\)|
    |--------------------------------|--------------------------------------|
    |     ``` {#codeblock_h5z_9ni_e3u}
{
"Version": "1",
"Statement": [
{
"Effect": "Allow",
"Action": [
  "oss:ListBuckets"
],
"Resource": [
  "acs:oss:*:*:*"
]
},
{
"Effect": "Allow",
"Action": [
  "oss:Listobjects",
  "oss:GetObject",
  "oss:PutObject",
  "oss:DeleteObject"
],
"Resource": [
  "acs:oss:*:*:test-bucket",
  "acs:oss:*:*:test-bucket/*"
]
}
]
}
    ```

 |     ``` {#codeblock_1g5_77a_u0x}
{
"Version": "1",
"Statement": [
{
"Effect": "Allow",
"Action": [
  "oss:ListBuckets"
],
"Resource": [
  "acs:oss:*:*:*"
]
},
{
"Effect": "Allow",
"Action": [
  "oss:Listobjects",
  "oss:GetObject",
  "oss:PutObject"
],
"Resource": [
  "acs:oss:*:*:prod-bucket",
  "acs:oss:*:*:prod-bucket/*"
]
}
]
}
    ```

 |

    After the preceding permission policies are granted to a RAM user, the RAM user is subject to the following restrictions in the E-MapReduce console:

    -   All buckets are displayed on the OSS selection page for creating clusters, jobs, and execution plans, but only the authorized buckets can be accessed.
    -   Only the contents of the authorized buckets are accessible.
    -   Only the authorized buckets can be read and written. An error is returned if the RAM user performs read or write operation on unauthorized buckets.
5.  Click **OK**.

## Step 4: Grant permissions to the RAM user {#section_p68_c2w_zjf .section}

1.  In the left-side navigation pane, click **Identities**, and click **Users**.
2.  In the **User Logon Name/Display Name** column, find the target RAM user.
3.  Click **Add Permissions**. On the page that appears, the principle is automatically filled in.
4.  In the **Policy Name** column, select the target policies by clicking the corresponding rows.

    **Note:** You can click **X** in the section on the right side of the page to delete the selected policy.

5.  Click **OK**.
6.  Click **Finished**.

## \(Optional\) Step 5: Authorize the RAM user to log on to the Alibaba Cloud console {#section_nhy_pqf_2ad .section}

If the console logon permission is not granted to the RAM user when the RAM user is created, you can grant the permission to the RAM user as follows:

1.  In the left-side navigation pane, click **Identities**, and click **Users**.
2.  In the **User Logon Name/Display Name** column, click the username of the target RAM user.
3.  In the **Console Logon Management** section of the **Authentication** tab, click **Modify Logon Settings**.
4.  Under **Console Password Logon**, select **Enabled**.
5.  Click **OK**.

## Step 6: Log on to the E-MapReduce console as the RAM user {#section_eoq_vld_2k3 .section}

1.  Log on to the [Alibaba Cloud console](https://signin.alibabacloud.com/login.htm) as the RAM user.
2.  After logging on to the console, select **E-MapReduce**.

