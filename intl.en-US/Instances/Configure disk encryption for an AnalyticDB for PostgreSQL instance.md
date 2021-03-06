# Configure disk encryption for an AnalyticDB for PostgreSQL instance

This topic describes how to configure disk encryption for an AnalyticDB for PostgreSQL instance in elastic storage mode. The disk encryption feature encrypts the data on each data disk of your AnalyticDB for PostgreSQL instance by using Elastic Block Storage \(EBS\). This way, your data cannot be cracked even if your data is leaked.

## Introduction

After an encrypted disk is created and attached to an ECS instance, the ECS instance encrypts the following data:

-   Static data on disks.
-   Data transmitted between disks and instances. Data is not encrypted again within the instance operating system.
-   All snapshots created from encrypted disks. Such snapshots are encrypted snapshots.

## Precautions

-   You can enable disk encryption only when your AnalyticDB for PostgreSQL instance is being created. After your instance is created, you cannot enable disk encryption.
-   You must select Enhanced SSD or Ultra Disk storage type for your instance when you create the instance.
-   Disk encryption cannot be disabled after it is enabled.
-   After you enable disk encryption for your instance, both the snapshots generated by the instance and the new instances created from those snapshots are automatically encrypted.
-   Disk encryption does not interrupt your business and you do not need to modify your application.

## Billing

The disk encryption feature is free of charge for AnalyticDB for PostgreSQL instances. You do not need to pay additional fees for the read/write operations you perform on encrypted disks.

For information about key hosting fees and API operation call fees, see [Billing](/intl.en-US/Pricing/Billing.md).

## Procedure

When you create an instance, perform the following operations. For more information, see [Create an instance](/intl.en-US/Quick Start/Create an instance.md).

1.  Set **Instance Resource Type** to Elastic Storage Mode.
2.  Set **Storage Type** to Enhanced SSD or Ultra Disk.
3.  Set **Encryption Type** to Disk Encryption.
4.  Select a key that is used for encryption. If no key is created, you must activate Key Management Service \(KMS\) and create a key.

    **Note:**

    -   Disk encryption supports only keys that are manually created. When you create keys, you must select Disable from the **Rotation Period** drop-down list. For more information, see [Create a CMK](/intl.en-US/Quick Start/Manage and use keys/Create a CMK.md).

    -   If you authorize the instance to access KMS, ActionTrail records the action. For more information, see [Use ActionTrail to record KMS events](/intl.en-US/Access control and audit/Use ActionTrail to record KMS events.md).
5.  Click **Buy Now** to configure disk encryption for the instance.

