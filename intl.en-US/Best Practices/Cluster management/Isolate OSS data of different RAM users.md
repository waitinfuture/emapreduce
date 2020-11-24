# Isolate OSS data of different RAM users

This topic describes how to use Resource Access Management \(RAM\) to isolate Object Storage Service \(OSS\) data of different RAM users.

An Alibaba Cloud account is created.

E-MapReduce \(EMR\) allows you to use RAM to isolate data of different RAM users.

## Procedure

1.  Log on to the [RAM console](https://ram.console.aliyun.com/) by using your Alibaba Cloud account.

2.  Create a RAM user.

    1.  In the left-side navigation pane, choose **Identities** \> **Users**.

    2.  On the Users page, click **Create User**.

        **Note:** You can create multiple RAM users at the same time.

    3.  On the Create User page, specify **Logon Name** and **Display Name**.

    4.  In the **Access Mode** section, select **Console Password Logon** or **Programmatic Access**.

        -   **Console Password Logon**: If you select this check box, you must also complete the basic security settings for logon, including deciding whether to automatically generate a password or customize the logon password, whether the user must reset the password upon the next logon, and whether to enable multi-factor authentication \(MFA\).
        -   **Programmatic Access**: If you select this check box, an AccessKey pair is automatically created for the RAM user. The user can access Alibaba Cloud resources by calling an API operation or by using a development tool.
        **Note:** We recommend that you select only one access mode for the RAM users to ensure the security of your Alibaba Cloud account. This prevents RAM users who have terminated their employment contracts with the company from accessing Alibaba Cloud resources.

    5.  Click **OK**.

3.  Create permission policies.

    RAM provides the default permission policies. RAM also allows you to customize permission policies for flexible authorization. You can create multiple permission policies based on your business requirements.

    1.  In the left-side navigation pane, choose **Permissions** \> **Policies**.

    2.  On the Policies page, click **Create Policy**. The Create Custom Policy page appears.

    3.  Specify **Policy Name**.

    4.  Select **Script** for Configuration Mode.

        For more information about how to configure a permission policy in **Script** mode, see [Policy structure and syntax](/intl.en-US/Policy Management/Policy language/Policy structure and syntax.md). In this example, the script mode is used to create two permission policies. The policies are described in the following table.

        |Test environment \(test-bucket\)|Production environment \(prod-bucket\)|
        |--------------------------------|--------------------------------------|
        |        ```
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

|        ```
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
        ``` |

        After the preceding permission policies are granted to a RAM user, the RAM user is subject to the following limits in the EMR console:

        -   When the RAM user creates a cluster, job, or workflow, all buckets are displayed on the OSS file page. However, the RAM user can access only authorized buckets.
        -   Only the data of authorized buckets can be viewed.
        -   A job can read and write data only from and to an authorized bucket.
    5.  Click **OK**.

4.  Authorize the RAM user.

    If the created RAM user is not authorized, perform the following steps to authorize the RAM user:

    1.  In the left-side navigation pane, choose **Identities** \> **Users**.

    2.  On the Users page, find the RAM user you want to authorize and click **Add Permissions** in the Actions column.

    3.  In the Add Permissions panel, click the policies that you want to attach to the RAM user and click **OK**.

    4.  Click **Complete**.

5.  Grant the console logon permission to the RAM user.

    If you have not granted the console logon permission to the RAM user that you created, perform the following steps to grant the permission to the RAM user:

    1.  In the left-side navigation pane, choose **Identities** \> **Users**.

    2.  On the Users page, find the RAM user to which you want to grant the console logon permission and click the logon name of the RAM user.

    3.  In the **Console Logon Management** section of the page that appears, click **Enable Console Logon**.

    4.  In the Modify Logon Settings panel, select **Enabled** for **Console Password Logon**.

    5.  Click **OK**.

6.  Log on to the EMR console by using the RAM user.

    1.  Log on to the [Alibaba Cloud Management Console](https://signin.alibabacloud.com/login.htm) by using the RAM user.

    2.  Click the More icon in the upper-left corner and choose Products and Services \> **E-MapReduce**.


