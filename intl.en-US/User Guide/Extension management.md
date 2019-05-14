# Extension management {#concept_bnw_sty_52b .concept}

AnalyticDB for PostgreSQL is developed based on the Greenplum Database and is enhanced with some in-depth extensions by Alibaba Cloud. This document introduces the [extension types](#type), and how to [create](#create) or [delete](#delete) an extension.

## Extension types {#type .section}

AnalyticDB for PostgreSQL supports the following extensions:

-   PostGIS: supports geographic information data.

-   MADlib: supports function library on Machine Learning.

-   fuzzystrmatch: supports fuzzy matching of strings.

-   orafunc: supports some Oracle functions.

-   oss\_ext: supports reading data from OSS.

-   hll: supports using the HyperLogLog algorithm to perform statistical analysis.

-   pljava: supports compiling user-defined functions \(UDF\) in PL/Java.

-   pgcrypto: supports encryption functions.

-   intarray: supports integer array-related functions, operators and indexes.


## Create an extension {#create .section}

Run the following command to create an extension:

```
CREATE EXTENSION <extension name>;
CREATE SCHEMA <schema name>;
CREATE EXTENSION IF NOT EXISTS <extension name> WITH SCHEMA <schema name>;
```

**Note:** 

You need to create the plpythonu extension before creating the MADlib extension, as shown in the following example.

```
CREATE EXTENSION plpythonu;
CREATE EXTENSION madlib;
```

## Delete an extension {#delete .section}

Run the following command to delete an extension:

**Note:** If some other objects are dependent on the extension, you need to add the CASCADE key word to remove all the dependencies first.

```
DROP EXTENSION <extension name>;
DROP EXTENSION IF EXISTS <extension name> CASCADE;
```

