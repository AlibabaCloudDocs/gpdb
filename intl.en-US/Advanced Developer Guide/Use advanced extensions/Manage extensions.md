# Manage extensions

AnalyticDB for PostgreSQL is an online, distributed cloud data warehousing service that consists of multiple compute nodes and provides massively parallel processing \(MPP\) data warehousing solutions. AnalyticDB for PostgreSQL is developed by Alibaba Cloud based on open source Greenplum Database with enhanced, in-depth extensions.

## Supported extensions

AnalyticDB for PostgreSQL supports the following extensions:

-   PostGIS: processes geographic data. For more information, see [Use PostGIS](/intl.en-US/Advanced Developer Guide/Use advanced extensions/Use PostGIS.md).

-   MADlib: provides a machine learning function library.

-   fuzzystrmatch: implements fuzzy match of strings.

-   orafunc: provides compatibility with some Oracle functions. For more information, see [Oracle Compatibility Functions](https://gpdb.docs.pivotal.io/43330/utility_guide/orafce_ref.html). *Note: AnalyticDB for PostgreSQL V4.3 supports this extension, whereas AnalyticDB for PostgreSQL V6.0 does not.*

-   orafce: provides compatibility with some Oracle functions. For more information, see [orafce](https://pgxn.org/dist/orafce/3.7.2/). *Note: AnalyticDB for PostgreSQL V6.0 supports this extension, whereas AnalyticDB for PostgreSQL V4.3 does not.*

-   oss\_ext: reads data from Object Storage Service \(OSS\).

-   HyperLogLog: collects statistics. For more information, see [Use HyperLogLog](/intl.en-US/Advanced Developer Guide/Use advanced extensions/Use HyperLogLog.md).

-   PL/Java: compiles user-defined functions \(UDFs\) in PL/Java. For more information, see [Create and use PL/Java UDFs](/intl.en-US/Advanced Developer Guide/Use PL/Java/Create and use PL/Java UDFs.md).

-   pgcrypto: provides cryptography functions that use encryption algorithms to ensure data security. Algorithms include MD5 message-digest \(MD5\), Secure Hash Algorithm 1 \(SHA-1\), SHA-224, SHA-256, SHA-384, SHA-512, Blowfish, Advanced Encryption Standard 128 \(AES-128\), AES-256, Raw Encryption, Pretty Good Privacy \(PGP\) symmetric keys, and PGP public keys. For more information, see [pgcrypto](https://www.postgresql.org/docs/9.4/pgcrypto.html).

-   IntArray: provides integer array-related functions, operators, and indexes.

-   RoaringBitmap: uses the RoaringBitmap efficient compression algorithm for bitmap operations.

-   postgres\_fdw: enables data access across databases. *Note: AnalyticDB for PostgreSQL V6.0 supports this extension, whereas AnalyticDB for PostgreSQL V4.3 does not.*

-   tablefunc: includes various functions that return tables. *Note: AnalyticDB for PostgreSQL V6.0 supports this extension, whereas AnalyticDB for PostgreSQL V4.3 does not.*

-   zhparser: provides full-text search of Chinese language. *Note: AnalyticDB for PostgreSQL V6.0 supports this extension, whereas AnalyticDB for PostgreSQL V4.3 does not.*


## Create an extension

Execute the following statements to create an extension:

```
CREATE EXTENSION <extension name>;
CREATE SCHEMA <schema name>;
CREATE EXTENSION IF NOT EXISTS <extension name> WITH SCHEMA <schema name>;
```

**Note:** Before you create a MADlib extension, you must create a plpythonu extension.

```
CREATE EXTENSION plpythonu;
CREATE EXTENSION madlib;
```

## Delete an extension

Execute the following statements to delete an extension.

**Note:** If an object depends on an extension that you want to delete, you must add the CASCADE keyword to delete the object.

```
DROP EXTENSION <extension name>;
DROP EXTENSION IF EXISTS <extension name> CASCADE;
```

