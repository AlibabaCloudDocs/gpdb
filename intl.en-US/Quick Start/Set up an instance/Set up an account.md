# Set up an account {#concept_bhh_2mr_52b .concept}

This document describes how to create an account and reset the password for a AnalyticDB for PostgreSQL instance.

## Create an account { .section}

**Prerequisites**

Before using a AnalyticDB for PostgreSQL instance, you must create an account for the database.

**Note:** 

-   You cannot delete the initial account after it is created.
-   You cannot create other accounts on the console, but you can create them by running SQL statements after logging in to the database.

**Procedure**

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com/).
2.  Select the region where the target instance is located.

3.  Click the ID of the instance to go to the **Basic Information** page of the instance.

4.  Click **Account Management** in the left-side navigation pane.

5.  Click **Create Account**.

6.  Enter the database account and password, and then click **OK**.

    -   Database Account: contains 2 to 16 characters, and consists of lowercase letters, numbers, or underscores \(\_\). It must start with a letter and end with a letter or number. For example, *user4example*.

    -   Password: contains 8 to 32 characters. It must consist of at least three types of the following characters: uppercase letters, lowercase letters, numbers, or special characters.

    -   Confirm Password: Enter the password again.


## Reset account password { .section}

When using AnalyticDB for PostgreSQL, if you forget the password of the database account, you can reset the password in the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com/).

**Note:** We recommend that you change the password on a regular basis for data security considerations.

**Procedure**

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdb.console.aliyun.com/).
2.  Select the region where the target instance is located.

3.  Click **Manage** under the **Action** column of the target instance to go to the **Basic Information** page of the instance.

4.  Click **Account Management** in the left-side navigation pane.

5.  Click **Reset password** under the account to be managed.

6.  Enter and confirm the new password, and then click **OK**.

    **Note:** 

    The password must consist of 8 to 32 characters and contain at least three types of the following characters: uppercase letters, lowercase letters, numbers, or special characters. A password that is previously used is not allowed.


## Related API {#section_uph_3n5_vgb .section}

|API|Description|
|---|-----------|
|[CreateAccount](../../../../intl.en-US/API Reference/Account management/CreateAccount.md#)|Creates an account.|
|[DescribeAccounts](../../../../intl.en-US/API Reference/Account management/DescribeAccounts.md#)|Returns the account information for a database.|
|[ModifyAccountDescription](../../../../intl.en-US/API Reference/Account management/ModifyAccountDescription.md#)|Modifies the description of the database.|
|[ResetAccountPassword](../../../../intl.en-US/API Reference/Account management/ResetAccountPassword.md#)|Resets the password for an account.|

