# Configure an account

This topic describes how to create an account and reset the password for an AnalyticDB for PostgreSQL instance.

AnalyticDB for PostgreSQL provides two types of database accounts: privileged accounts and standard accounts.

-   Privileged accounts have all permissions on all databases.
-   Standard accounts have all permissions only on the databases that the accounts are authorized to manage.

    **Note:** Permissions include SELECT, INSERT, UPDATE, DELETE, TRUNCATE, REFERENCES, and TRIGGER.


For more information about permission settings, see [Manage users and permissions](/intl.en-US/Beginner Developer Guide/Manage users and permissions.md).

An account is created for an AnalyticDB for PostgreSQL instance before you use AnalyticDB for PostgreSQL. You cannot use the console to create other accounts. However, you can connect to the instance and execute SQL statements to create other accounts. For more information, see [Execute SQL statements to create accounts](#section_i3t_xe5_sfu).

## Create an initial account

**Note:**

-   After the initial account is created, you cannot delete the account.
-   The initial account is a privileged account.

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdbnext.console.aliyun.com/gpdb/cn-hangzhou/list).

2.  In the upper-left corner, select the region where your instance resides.

3.  Click the name of the instance. The details page of the instance appears.

4.  In the left-side navigation pane, click **Account Management**. The **Account Management** page appears.

5.  Click **Create Account**. The **Create Account** page appears.

6.  Enter the account name and password, and click **OK**.

    -   The account name must be 2 to 16 characters in length, and can contain lowercase letters, digits, and underscores \(\_\). It must start with a letter and end with a letter or digit. For example, *user4example*.

    -   The password must be 8 to 32 characters in length. It must contain at least three of the following character types: uppercase letters, lowercase letters, digits, and special characters.

    -   Confirm the password and make sure that you enter the correct password.


## Execute SQL statements to create accounts

-   Create a privileged account

    ```
    create role admin0  WITH LOGIN ENCRYPTED PASSWORD '111111' rds_superuser;
    ```

-   Create a standard account

    ```
    create role test1 WITH LOGIN ENCRYPTED PASSWORD '111111';
    ```


## Reset a password

If you forget the password of your database account, you can reset the password in the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com).

**Note:** To ensure data security, we recommend that you change your password on a regular basis.

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com).

2.  In the upper-left corner, select the region where your instance resides.

3.  Click the name of the instance. The details page of the instance appears.

4.  In the left-side navigation pane, click **Account Management**. The **Account Management** page appears.

5.  Find the account that you want to manage, and click **Reset Password** in the Actions column. The **Modify Account** pane appears.

6.  After you enter and confirm the new password, click **OK**.

    **Note:** The password must be 8 to 32 characters in length. It must contain at least three of the following character types: uppercase letters, lowercase letters, digits, and special characters. We recommend that you do not use an old password.


## Related API operations

|API|Description|
|---|-----------|
|[CreateAccount](/intl.en-US/API Reference/Account management/CreateAccount.md)|Creates an account for a database.|
|[DescribeAccounts](/intl.en-US/API Reference/Account management/DescribeAccounts.md)|Queries the account information of a database.|
|[ModifyAccountDescription](/intl.en-US/API Reference/Account management/ModifyAccountDescription.md)|Modifies the account name of a database.|
|[ResetAccountPassword](/intl.en-US/API Reference/Account management/ResetAccountPassword.md)|Resets the password of an account.|

