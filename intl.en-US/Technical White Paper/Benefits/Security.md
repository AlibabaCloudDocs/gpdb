# Security

## Network and data links

AnalyticDB for PostgreSQL uses a cloud native distributed system and provides high security. Each instance is created in a virtual private cloud \(VPC\). A VPC is an isolated virtual network that provides higher security and performance than the classic network. You can choose whether to enable a public endpoint based on the scenarios to reduce potential security risks. Only the IP addresses and CIDR blocks that are contained in IP address whitelists are allowed to connect to the instance. You can modify an IP address whitelist in the AnalyticDB for PostgreSQL console.

To improve the security of data links, AnalyticDB for PostgreSQL allows you to enable Secure Sockets Layer \(SSL\) for encryption. The SSL protocol was renamed to Transport Layer Security \(TLS\) after SSL was standardized by the Internet Engineering Task Force \(IETF\). AnalyticDB for PostgreSQL provides SSL Certification Authority \(CA\) certificates for applications. You can even enable mutual authentication certificates to verify the identities of the server and client for higher security. The AnalyticDB for PostgreSQL console provides a secure method to download and update certificates for applications.

## Account and permission management

AnalyticDB for PostgreSQL supports two types of accounts: privileged accounts and standard accounts.

-   Privileged accounts have all permissions on all databases.
-   Standard accounts have all permissions only on the databases that the accounts are authorized to manage.

Before you use AnalyticDB for PostgreSQL, you must create a privileged account that is used to connect to databases in the console. After a privileged account is created, you cannot delete it and you can only reset or change its password. Passwords of accounts are encrypted and stored by using the MD5 hash algorithm for plaintext password security.

**Note:** You cannot create other accounts in the console. However, you can use SQL statements to create and manage other users and user groups after you connect to databases.

By default, a new database contains the public schema and allows all accounts to read data from or write data to the public schema. However, you can use an account to access only the objects that you create. You cannot access the objects that are not owned by the account in the schema. If you want to use an account to access the objects that are not owned by the account in the schema, the account must be granted the required permissions by the owner of the objects. If the appropriate permissions are granted, you can create objects in other schemas. You can manage permissions at the database, schema, and table levels in a fine-grained manner. For example, you can grant read permissions on a table to an account, but you cannot modify the table. In summary, a standard account can access only database objects and system-level default objects whose permissions are granted to the account. To access other objects, the account must be granted the required permissions.

## Data encryption

AnalyticDB for PostgreSQL provides the disk encryption feature. This feature encrypts the data on each disk of your instance based on Elastic Block Storage \(EBS\). This way, your data cannot be decrypted even if it is leaked. After disk encryption is enabled, the static data stored on the disk and the data transmitted between the disk and the AnalyticDB for PostgreSQL instance is encrypted. Snapshots generated from this disk and disks created from these snapshots inherit this encryption attribute. To use the disk encryption feature, you must select enhanced SSD \(ESSD\) or ultra disk as the storage disk type when you create an instance. Disk encryption cannot be disabled after it is enabled.

The keys used for disk encryption are managed by Key Management Service \(KMS\) and activated by users. ActionTrail records each access to keys.

AnalyticDB for PostgreSQL also uses the pgcrypto plug-in to encrypt sensitive data at the table or column level. Data is encrypted by using functions that are provided by the plug-in. Data and keys are transmitted as plaintext. We recommend that you enable the SSL encryption feature for data links to prevent data leaks during data transmissions between AnalyticDB for PostgreSQL and applications. pgcrypto provides cryptography functions that use encryption algorithms to ensure data security. The algorithms include MD5, Secure Hash Algorithm 1 \(SHA-1\), SHA-224, SHA-256, SHA-384, SHA-512, Blowfish, Advanced Encryption Standard 128 \(AES-128\), AES-256, Raw Encryption, Pretty Good Privacy \(PGP\) symmetric keys, and PGP public keys.

## SQL audit

The AnalyticDB for PostgreSQL console provides the SQL audit feature, which allows you to view SQL details and audit SQL statements on a regular basis. SQL audit logs record all data manipulation language \(DML\) and data description language \(DDL\) operations. The SQL audit feature does not affect instance performance.

