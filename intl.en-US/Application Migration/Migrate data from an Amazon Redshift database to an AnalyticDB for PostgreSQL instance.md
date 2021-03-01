# Migrate data from an Amazon Redshift database to an AnalyticDB for PostgreSQL instance

This topic describes how to migrate data from an Amazon Redshift database to an AnalyticDB for PostgreSQL instance.

## Overall procedure

To migrate data from an Amazon Redshift database to an AnalyticDB for PostgreSQL instance, you must perform the following steps:

1.  Prepare environments and resources for Amazon Redshift, Amazon Simple Storage Service \(Amazon S3\), AnalyticDB for PostgreSQL, and Object Storage Service \(OSS\).
2.  Import data from an Amazon Redshift database to an Amazon S3 bucket.
3.  Import CSV data files from the Amazon S3 bucket to an OSS bucket by using ossimport.
4.  In the destination AnalyticDB for PostgreSQL instance, create objects that correspond to the objects in the source Amazon Redshift database. The objects include schemas, tables, views, and functions.
5.  Import data from the OSS bucket to the AnalyticDB for PostgreSQL instance by using an OSS external table.

The following figure shows the overall procedure.

![2021030103](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/83416/155782109935299_en-US.png)

## Preparations on AWS

Prepare security credentials that are used to access the Amazon S3 bucket

The security credentials include the following items:

-   An Amazon Web Services \(AWS\) access key ID and secret access key.
-   The endpoint of an Amazon S3 bucket. Example: s3.ap-southeast-2.amazonaws.com.
-   The name of the Amazon S3 bucket. Example: alibaba-hybrid-export.

Format conventions for exported data

-   The exported data file must be in the CSV format.
-   The size of the exported data file can be up to 50 MB.
-   The column order in the exported data file must be the same as that in the CREATE TABLE statement.
-   The ideal number of exported data files is the same as or an integer multiple of the number of compute nodes of the destination AnalyticDB for PostgreSQL instance.

Recommended UNLOAD command options to import data from Amazon Redshift to Amazon S3

We recommend that you use the following UNLOAD command options to import data from the Amazon Redshift database to the Amazon S3 bucket:

```
unload ('select * from test')
to 's3://xxx-poc/test_export_'
access_key_id '<Your access key id>'
secret_access_key '<Your access key secret>'
DELIMITER AS ','
ADDQUOTES
ESCAPE
NULL AS 'NULL'
MAXFILESIZE 50 mb;
            
```

In the preceding example, the following options are recommended:

```
DELIMITER AS ','
ADDQUOTES
ESCAPE
NULL AS 'NULL'
MAXFILESIZE 50 mb
            
```

Export DDL statements from the Amazon Redshift database

Export all data definition language \(DDL\) statements from the Amazon Redshift database, which include but not limited to the statements that are used to create schemas, tables, views, and functions.

## Preparations on Alibaba Cloud

Prepare a RAM user

-   The ID of the RAM user.
-   The password of the RAM user.
-   The AccessKey ID of the RAM user.
-   The AccessKey secret of the RAM user.

Create an OSS bucket

Create an OSS bucket in the region where the Amazon S3 bucket is deployed, such as the China \(Beijing\) region. After you create an OSS bucket, you can obtain the endpoint of the bucket in the OSS console. The endpoint of the bucket that is hosted on **an ECS instance in a VPC** is used in this example. VPC-based data migration improves transfer efficiency and security.

Download and install ossimport

1.  Create an ECS instance in the region where the Amazon S3 bucket is deployed. Set the bandwidth to 100 Mbit/s and set the system image to Windows x64 for the ECS instance.
2.  Download and deploy ossimport in standalone mode on an ECS instance. For more information about how to download the latest version of ossimport, see [Architectures and configurations](/intl.en-US/Tools/ossimport/Architectures and configurations.md).
3.  The decompressed software package for ossimport in standalone mode has the following file structure:

```
ossimport
├── bin
│ └── ossimport2.jar  # The JAR package that contains the Master, Worker, Tracker, and Console modules.
├── conf
│ ├── local_job.cfg   # The Job configuration file for the standalone mode.
│ └── sys.properties  # The configuration file that contains system parameters.
├── console.bat         # The Windows command line utility that is used to run tasks step by step.
├── console.sh          # The Linux command line utility that is used to run tasks step by step.
├── import.bat          # The script that automatically imports files based on the conf/local_job.cfg configuration file in Windows. The configuration file contains parameters that specify operations involved in data migration, such as start, migration, verification, and retry.
├── import.sh           # The script that automatically imports files based on the conf/local_job.cfg configuration file in Linux. The configuration file contains parameters that specify operations involved in data migration, such as start, migration, verification, and retry.
├── logs                # The directory that contains logs.
└── README.md           # The file in which ossimport is introduced or described. We recommend that you read this file before you use ossimport.
            
```

## Import data from the Amazon S3 bucket to the OSS bucket by using ossimport

Configure ossimport

ossimport in standalone mode is used in this example. Edit the conf/local\_job.cfg file. Only the parameter configurations that must be modified are provided in this example. For more information, see [Architectures and configurations](/intl.en-US/Tools/ossimport/Architectures and configurations.md).

```
srcType=s3
srcAccessKey="your AWS Access Key ID"
srcSecretKey="your AWS Access Key Secret"
srcDomain=s3.ap-southeast-2.amazonaws.com
srcBucket=alibaba-hybrid-export
srcBucket=
destAccessKey="your Alibaba Cloud Access Key ID"
destSecretKey="your Alibaba Cloud Access Key Secret"
destDomain=http://oss-ap-southeast-2-internal.aliyuncs.com
destBucket=alibaba-hybrid-export-1
destPrefix=
isSkipExistFile=true
            
```

Use ossimport to start the data migration task

Run the import.bat script to start the data migration task by using ossimport in standalone mode.

View the status of the data migration task

During the data migration process, you can view the status of the data migration task in the command line window. You can view the bandwidth usage in Windows Task Manager.

In this example, the ECS instance and the OSS bucket are deployed in the same region. Data is transferred from the ECS instance to the OSS bucket over a VPC, which is not subject to the network speed limit. Data is transferred from the Amazon S3 bucket and the ECS instance over the Internet, which is subject to the network speed limit. The upload speed is limited by the download speed. Therefore, the speed of uploading data from the ECS instance to the OSS bucket is almost the same as the speed of downloading data from the Amazon S3 bucket to the ECS instance.

\(Optional\) Retry a failed task

The data migration task may fail due to factors such as network. In the command line window on the Windows ECS instance, run the console.bat retry command to retry the task. Only failed subtasks are retried. Successful subtasks are not retried.

\(Optional\) Check the files in the OSS bucket

You can check the imported data files in the OSS bucket by using the OSS console. We recommend that you use the ossbrowser tool to view and manage the files in the OSS bucket. For more information about how to download ossbrowser, see [Quick start](/intl.en-US/Tools/ossbrowser/Quick start.md).

\(Optional\) Organize data in the CSV files

**Note:** This step is optional and provided for reference only. You can organize CSV files based on your business needs.

-   Replace `NULL` with spaces in the CSV files.
-   Replace `\,` with `,` in the CSV files.

We recommend that you organize data in a local environment. You can download the files to the ECS instance by using ossbrowser and organize data in the files. Then, upload the organized CSV files to a new OSS bucket. This way, you can distinguish the collated CSV files from the CSV files that are downloaded from the Amazon S3 bucket. When you upload or download CSV files, we recommend that you use the internal endpoint of OSS. This way, the traffic overhead on the internal network is reduced.

## Convert DDL statements from Amazon Redshift to AnalyticDB for PostgreSQL syntax format

Before you create an AnalyticDB for PostgreSQL instance, you must convert the DDL statements that are exported in the preceding step from Amazon Redshift to AnalyticDB for PostgreSQL syntax format. The following section describes the coversion rules.

CREATE SCHEMA

Create a schema based on the AnalyticDB for PostgreSQL syntax and save the schema as create\_schema.sql. The following example provides statements that comply with the AnalyticDB for PostgreSQL syntax:

```
CREATE SCHEMA schema1
  AUTHORIZATION xxxpoc;
GRANT ALL ON SCHEMA schema1 TO xxxpoc;
GRANT ALL ON SCHEMA schema1 TO public;
COMMENT ON SCHEMA model IS 'for xxx migration  poc test';

CREATE SCHEMA oss_external_table
  AUTHORIZATION xxxpoc;
            
```

CREATE FUNCTION

Modify or rewrite the Amazon Redshift SQL functions that are not supported in AnalyticDB for PostgreSQL. The following examples describe how to modify or rewrite some of these functions:

-   Use the following statement to replace the `CONVERT_TIMEZONE(a,b,c)` function:

    ```
    timezone(b, timezone(a,c))
    ```

-   Use the following statement to replace the `GETDATE()` function:

    ```
    current_timestamp(0):timestamp
    ```

-   Replace or optimize user-defined functions \(UDFs\).

    The following example shows an Amazon Redshift SQL function:

    ```
    CREATE OR REPLACE FUNCTION public.f_jdate(dt timestamp without time zone)
    RETURNS character varying AS
    '      from datetime import timedelta, datetime
           if dt.hour < 4:
                  d = timedelta(days=-1)
                  dt = dt + d
           return str(dt.date())'
    LANGUAGE plpythonu IMMUTABLE;
    COMMIT;
                        
    ```

    To improve performance, replace the function with the following AnalyticDB for PostgreSQL function:

    ```
    to_char(a - interval '4 hour', 'yyyy-mm-dd')
    ```

-   Other functions that comply with Amazon Redshift syntax

    You can query the standard SQL function library of PostgreSQL at [Functions and Operators in PostgreSQL8.2](https://www.postgresql.org/docs/8.2/functions.html). Based on the query results, you can modify or rewrite the Amazon Redshift functions that are not supported in AnalyticDB for PostgreSQL. The following list provides some of these functions:

    -   [ISNULL\(\)](https://docs.microsoft.com/en-us/sql/t-sql/functions/isnull-transact-sql?view=sql-server-2017)
    -   [DATEADD\(\)](https://docs.aws.amazon.com/redshift/latest/dg/r_DATEADD_function.html)
    -   [DATEDIFF\(\)](https://docs.aws.amazon.com/redshift/latest/dg/r_DATEDIFF_function.html)
    -   [REGEXP\_COUNT\(\)](https://docs.aws.amazon.com/redshift/latest/dg/REGEXP_COUNT.html)
    -   [LEFT\(\)](https://docs.aws.amazon.com/redshift/latest/dg/r_LEFT.html)
    -   [RIGHT\(\)](https://docs.aws.amazon.com/redshift/latest/dg/r_LEFT.html)

CREATE TABLE

-   Modify compression encodings. Some compression encodings of Amazon Redshift are not supported by AnalyticDB for PostgreSQL. For more information, visit [Compression encodings](https://docs.aws.amazon.com/zh_cn/redshift/latest/dg/c_Compression_encodings.html). The following list provides the unsupported compression encodings:

    -   BYTEDICT
    -   DELTA
    -   DELTA32K
    -   LZO
    -   MOSTLY8
    -   MOSTLY16
    -   MOSTLY32
    -   RAW \(no compression\)
    -   RUNLENGTH
    -   TEXT255
    -   TEXT32K
    -   ZSTD
    Replace `ENCODE XXX` in an Amazon Redshift CREATE TABLE statement with the following clause:

    ```
    with (COMPRESSTYPE={ZLIB|QUICKLZ|RLE_TYPE|NONE})
    ```

-   Modify distribution keys. Amazon Redshift supports three distribution keys. For more information, visit [Distribution styles](https://docs.aws.amazon.com/zh_cn/redshift/latest/dg/c_choosing_dist_sort.html). You must modify distribution keys based on the following rules:
    -   EVEN distribution: Replace DISTSTYLE EVEN with `distributed randomly`.
    -   KEY distribution: Replace DISKEY with `distributed by (colname1,...)`.
    -   ALL distribution: Delete ALL. AnalyticDB for PostgreSQL does not support ALL.
-   Modify sort keys. Replace the COMPOUND or INTERLEAVED option in the `[ COMPOUND | INTERLEAVED ] SORTKEY (column_name [, ...]) ]` sort key clause of Amazon Redshift with the following clause:

    ```
    with(APPENDONLY=true,ORIENTATION=column)
    sortkey (volume);
                        
    ```

    Example 1

    The following example shows an Amazon Redshift CREATE TABLE statement:

    ```
    CREATE TABLE schema1.table1
    (
        filed1 VARCHAR(100) ENCODE lzo,
        filed2 INTEGER DISTKEY,
        filed3 INTEGER,
        filed4 BIGINT ENCODE lzo,
        filed5 INTEGER,
    )
    INTERLEAVED SORTKEY
    (
        filed1,
        filed2
    );
                        
    ```

    Convert the preceding statement to the following CREATE TABLE statement of AnalyticDB for PostgreSQL:

    ```
    CREATE TABLE schema1.table1
    (
        filed1 VARCHAR(100) ,
        filed3 INTEGER,
        filed5 INTEGER
    )
    WITH(APPENDONLY=true,ORIENTATION=column,COMPRESSTYPE=zlib)
    DISTRIBUTED BY (filed2)
    SORTKEY
    (
        filed1,
        filed2
    )
                        
    ```

    Example 2

    The following example shows an Amazon Redshift CREATE TABLE statement, which includes the ENCODE and SORTKEY options:

    ```
    CREATE TABLE schema2.table2
    (
        filed1 VARCHAR(50) ENCODE lzo,
        filed2 VARCHAR(50) ENCODE lzo,
        filed3 VARCHAR(20) ENCODE lzo,
    )
    DISTSTYLE EVEN
    INTERLEAVED SORTKEY
    (
        filed1
    );
                        
    ```

    Convert the preceding statement to the following CREATE TABLE statement of AnalyticDB for PostgreSQL:

    ```
    CREATE TABLE schema2.table2
    (
        filed1 VARCHAR(50),
        filed2 VARCHAR(50),
        filed3 VARCHAR(20),
    )
    WITH(APPENDONLY=true, ORIENTATION=column, COMPRESSTYPE=zlib)
    DISTRIBUTED randomly
    SORTKEY
    (
        filed1
    );
                        
    ```


CREATE VIEW

Convert an Amazon Redshift CREATE VIEW statement to an SQL statement that complies with the AnalyticDB for PostgreSQL syntax. The conversion rules are similar to those of CREATE TABLE statements.

## Create and configure an AnalyticDB for PostgreSQL instance

Create and configure an AnalyticDB for PostgreSQL instance. For more information, see the following topics:

-   [Create an instance](/intl.en-US/Quick Start/Create an instance.md)
-   [Configure an IP address whitelist for an AnalyticDB for PostgreSQL instance](/intl.en-US/Quick Start/Configure an IP address whitelist for an AnalyticDB for PostgreSQL instance.md)
-   [t16843.md\#](/intl.en-US/Quick Start/Configure an account.md)

## Create database objects

Use the psql or pgAdmin III 1.6.3 client tool to log on to the AnalyticDB for PostgreSQL database. For more information, see [Connect to an AnalyticDB for PostgreSQL instance](/intl.en-US/Quick Start/Connect to an AnalyticDB for PostgreSQL instance.md).

Convert the Amazon Redshift DDL statements to the DDL statements that comply with the AnalyticDB for PostgreSQL syntax based on the preceding conversion rules. Then, execute the converted DDL statements to create database objects.

## Create OSS external tables

AnalyticDB for PostgreSQL can import data from or export data to OSS in parallel by using OSS external tables known as the gpossext feature. AnalyticDB for PostgreSQL also supports GZIP compression for OSS external tables to save storage space and costs. For more information about how to create OSS external tables, see [Import or export OSS data by using OSS external tables](/intl.en-US/Data Access/Import or export OSS data by using OSS external tables.md).

## Import data by using the INSERT INTO script

After you create an OSS external table and the destination database objects, you must prepare the `INSERT` script to insert data of the OSS external table into the tables of the destination AnalyticDB for PostgreSQL database. Save the script as the insert.sql file and run the script.

The statement that is used to insert data must be in the following format: `INSERT INTO <TABLE NAME> SELECT * FROM <OSS EXTERNAL TABLE NAME>;`

Example:

```
INSERT INTO schema1.table1 SELECT * FROM oss_external_table.table1;
```

After you import the data, use the SELECT statement to query the imported data. Then, compare and analyze the imported data and the source data to verify data consistency.

## Clear the database by using the VACUUM script

After you import the data from the OSS external table to the AnalyticDB for PostgreSQL instance, you must clear the database by using the `VACUUM` script. Save the VACUUM script as the vacuum.sql file. For more information, visit [VACUUM](https://www.postgresql.org/docs/8.2/sql-vacuum.html).

