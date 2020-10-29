# Grant permissions to a RAM user

To make sure that your RAM users can use the E-MapReduce \(EMR\) console, you must access the Resource Access Management \(RAM\) console by using your Alibaba Cloud account and grant the required permissions to the RAM users.

RAM is a resource access control service provided by Alibaba Cloud. For more information, see [What is RAM?](/intl.en-US/Product Introduction/What is RAM?.md). The following examples describe how RAM is used to implement access control:

-   RAM users: If you purchased multiple instances for an EMR cluster, you can create a policy that allows some users who are responsible for O&M, development, or data analysis to use these instances. This avoids the potential accidental disclosure of your AccessKey pair under your account.
-   RAM user groups: You can create multiple user groups and grant different permissions to them. The authorization process is the same as that for RAM users. The user groups can be used to manage multiple RAM users at the same time.

## Policy

Policies are categorized into system policies and custom policies.

-   System policies: the default policies provided by Alibaba Cloud. The following system policies are frequently used in EMR:
    -   AliyunEMRFullAccess: provides RAM users with full access to EMR, including all permissions on all EMR resources.
    -   AliyunEMRDevelopAccess: provides RAM users with the developer permissions of EMR. This policy does not authorize RAM users to perform some operations such as creating and releasing clusters.
    -   AliyunEMRFlowAdmin: provides RAM users with the administrator permissions of EMR. This policy allows RAM users to create projects, and develop and manage jobs. It does not allow RAM users to add members to projects or manage clusters.
-   Custom policies: the user-defined policies. These policies are suitable for users who are familiar with various Alibaba Cloud service APIs and require fine-grained access control. To create a custom policy, see [Policy structure and syntax](/intl.en-US/Policy Management/Policy language/Policy structure and syntax.md).

## Grant permissions to a RAM user

Perform the following steps to grant permissions on EMR to RAM users in the RAM console:

1.  Log on to the [RAM console](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.6.77bd72fe3PD5pf#/policy/detail/system/AliyunEMRRolePolicy/info) by using an Alibaba Cloud account.

2.  In the left-side navigation pane, choose **Identities** \> **Users**.

3.  On the Users page, find the RAM user to which you want to grant permissions and click **Add Permissions** in the Actions column.

4.  In the Add Permissions pane, click the policies you want to attach to the RAM user, and click **OK**.

    For more information about policies, see [Policy](#section_efm_tri_nux).

5.  Click **Complete**.

    After authorization is completed, the granted permissions take effect immediately. The authorized RAM user can log on to the [RAM console](https://ram.console.aliyun.com/?spm=a2c4g.11186623.2.6.77bd72fe3PD5pf#/policy/detail/system/AliyunEMRRolePolicy/info) to check their permissions.


