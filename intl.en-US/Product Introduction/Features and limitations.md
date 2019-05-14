# Features and limitations {#concept_hjq_p1r_52b .concept}

This document lists the basic features and limits of AnalyticDB for PostgreSQL.

## Features { .section}

-   Covers the key functions of Greenplum Database. For details, see [Summary of Greenplum Features](http://gpdb.docs.pivotal.io/4380/ref_guide/feature_summary.html).

-   Supports MetaScan and SortKey.

-   Supports the ORCA optimizer.

-   Supports distributed stored procedures in PL/PGSQL and PL/JAVA.

-   Supports multiple extensions, such as PostGIS, MADlib, fuzzystrmatch, orafunc, pgcrypto, and intarray. \(You can use the `CREATE EXTENSION` command to create them.\)

-   Supports using OSS\_EXT extension to read data from or write data to Alibaba Cloud OSS \(Object Storage Service\), and supports the gzip compression to reduce the external table storage cost.

-   Supports the JSON data type and the HyperLogLog type. \(You can use the `CREATE EXTENSION` command to create them.\)

    **Note:** Only support json data type of JSON, and not support jsonb data type of JSON.


## Limits { .section}

-   For limits of the core functions, see [Summary of Greenplum Features](http://gpdb.docs.pivotal.io/4380/ref_guide/feature_summary.html).

-   Permission limits: The initial user of AnalyticDB for PostgreSQL \(the root user\) has the permission for creating databases \(`CREATEDB`\) and users \(`CREATEROLE`\), but does not have the super user permission \(`SUPERUSER`\). That is,

    -   The root user cannot perform operations requiring the super user permission. For example, the root user cannot run file functions such as `pg\_ls\_dir`.

    -   The root user has the permission to view and modify the data of all other non-super users, and terminate \(`Kill`\) the connections of other non-super users.

-   Not support the PL/R extensions.

-   Supports creating the PL/Python extensions, but does not support creating functions by using PL/Python.

-   Not support the gpfdist tool.

-   Not support MapReduce interfaces, gphdfs storage interfaces and local external tables.

-   Not support automatic backup and recovery. AnalyticDB for PostgreSQL keeps two copies of data, and you can back up data by using the pg\_dump tool.


