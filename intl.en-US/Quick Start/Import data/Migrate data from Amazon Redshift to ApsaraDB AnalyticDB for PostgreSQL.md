# Migrate data from Amazon Redshift to ApsaraDB AnalyticDB for PostgreSQL {#concept_cnv_xyk_ggb .concept}

This topic describes how to migrate data from Amazon Redshift to ApsaraDB AnalyticDB for PostgreSQL.

## Overall procedure {#section_z5d_jct_jgb .section}

A typical migration process is as follows:

1.  Prepare resources: Amazon Redshift, Amazon S3, ApsaraDB AnalyticDB for PostgreSQL, and Alibaba Cloud OSS.
2.  Import the data in Redshift to S3.
3.  Use OSSImport to import data files in .csv format from S3 to OSS.
4.  In AnalyticDB for PostgreSQL, create the required objects, including schemas, tables, views, and functions.
5.  Import data from the OSS external table into AnalyticDB for PostgreSQL.

The following figure shows the general workflow:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/83416/155782109935299_en-US.png)

## Preparations on AWS {#section_dvd_jct_jgb .section}

**Prepare information for accessing the S3 service**

-   Access Key ID and Secret Access Key
-   The endpoint of the bucket in S3, for example, **s3.ap-southeast-2.amazonaws.com**
-   The bucket name, for example, **alibaba-hybrid-export**

**Data format requirements for data export**

-   The data file must be in CSV format
-   The size of the file to be exported cannot exceed 50 MB
-   The order of the column values in the file is the same as the column order of the table creation statement
-   Ideally, the number of files to be exported is the same as the number of segments in AnalyticDB for PostgreSQL or a multiple of the number of segments

**Recommended Redshift UNLOAD command option**

The following UNLOAD command is the recommended format for the Redshift UNLOAD option, which is compatible with AnalyticDB for PostgreSQL:

```
unload ('select * from test')
to 's3://xxx-poc/test_export_'
access_key_id '<Your access key id>'
secret_access_key '<Your access key secret>'
DELIMITER AS ','
ADDQUOTES
ESCAPE
NULL AS 'NULL'
MAXFILESIZE 50 mb ;
			
```

Specifically, in an UNLOAD command, the following options are recommended:

```
DELIMITER AS ','
ADDQUOTES
ESCAPE
NULL AS 'NULL'
MAXFILESIZE 50 mb
			
```

**Get the DDL statement of the object in the Redshift database**

Export all DDL statements from AWS Redshift including, but not limited to, schema, table, function, and view.

## Preparations on Alibaba Cloud {#section_ivd_jct_jgb .section}

**Prepare information about the Alibaba Cloud RAM user account**

-   The RAM user account ID
-   The RAM user account password
-   The RAM user account AccessKeyId
-   The RAM user account AccessKeySecret \(paired with the preceding AccessKeyId to form an AccessKey\)

**Prepare a bucket in OSS**

Create a bucket in Alibaba Cloud OSS in the same region as the AWS S3 bucket \(for example, the Sydney \(ap-southeast-2\) region\).

After the OSS bucket is created, the Internet endpoint and VPC endpoint \(that is, the intranet endpoint\) of the bucket can be obtained from the OSS Console.

**Download and install OSSImport**

-   Create an ECS instance in the same area as the OSS bucket, with a network bandwidth of 100Mbps. In the following example, an instance running Windows is created.
-   [Download and install the latest version of OSSImport](https://www.alibabacloud.com/help/doc-detail/56990.htm).
-   After you unzip the OSSImport package, the following folders and files are displayed.

```
ossimport
├── bin
│ └── ossimport2.jar  # The JAR package including master, worker, tracker, and console modules
├── conf
│ ├── local_job.cfg   # Standalone job configuration file
│ └── sys.properties  # Configuration file for the system running
├── console.bat         # Windows command line, which can run distributed call-in tasks
├── console.sh          # Linux command line, which can run distributed call-in tasks
├── import.bat          # The configuration file for one-click import and execution in Windows is the data migration job configured in conf/local_job.cfg, including start, migration, validation, and retry
├── import.sh           # The configuration file of one-click import and execution in Linux is the data migration job configured in conf/local_job.cfg, including start, migration, validation, and retry
├── logs                # Log directory
└── README.md           # Description documentation. We recommend that you carefully read the documentation before using this feature
			
```

## Migrate data files from S3 to OSS using OSSImport {#section_pvd_jct_jgb .section}

**Configure OSSImport**

In the following example, OSSImport is used in the standalone deployment mode.

Edit conf/local\_job.cfg file. In this example, only the parameter configuration that must be modified is provided. For detailed configuration instructions for OSSImport, see [Architecture and configuration](https://www.alibabacloud.com/help/doc-detail/56990.htm).

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

**Start the OSSImport Migration Task**

In the OSSImport stand-alone deployment mode, you can start the migration task by executing import.bat.

**Monitor task status**

During the data migration process, you can see the output in the command execution window. Additionally, you can review the usage of the network bandwidth through the resource manager.

In this example, because the ECS instance and the OSS bucket are deployed in the same region, the network speed between data uploading from the instance to the bucket is not limited. Notably, because data is downloaded from S3 to OSS through the Internet, the speed of data transfer between ECS and OSS is essentially the same as the speed of data transfer between S3 and ECS. In this case, the upload speed is limited by the download speed.

**Failed task retry \(optional\)**

Sub-tasks may fail due to network or other reasons. Failure Retry only retries failed tasks, and will not retry the successful tasks. To retry failed tasks, execute `console.bat retry` in cmd.exe under the instance.

**Check the files migrated to the OSS Bucket \(optional\)**

You can check files through the OSS Console. We also recommend using the **ossbrowser** client tool to view and modify files in the bucket.[Dowload ossbrowser](https://www.alibabacloud.com/help/doc-detail/61872.htm).

**Scrub the csv files \(optional\)**

**Note:** If you want to scrub the data of your csv file, the following commands can be used.

-   Replace `NULL` in the csv files with blank spaces.
-   Replace `\,` with `,` in the csv files.

We recommend you perform data scrubbing operations on locally stored data. Specifically, you need to first download the data file to be scrubbed to ECS through the ossbrowser tool, and then scrub the data. After that, you need to upload the scrubbed data files to another newly created bucket \(so as to be distinguished from the original CSV files\). In later uses, when downloading the original csv files or uploading the scrubbed files, we recommend that ossbrowser use the intranet endpoint of OSS to reduce unnecessary charges to your account.

## DDL conversion from Redshift to AnalyticDB for PostgreSQL {#section_wvd_jct_jgb .section}

This section describes the preparations required before creating a AnalyticDB for PostgreSQL database object. Specifically, it describes how to convert the DDL statements in Redshift syntax format to AnalyticDB for PostgreSQL syntax format. This section also describes the corresponding syntax conventions.

**CREATE SCHEMA**

The following statement is an example that conforms to the PostgreSQL syntax format, which you can save as **create schema.sql**

```
CREATE SCHEMA schema1
  AUTHORIZATION xxxpoc;
GRANT ALL ON SCHEMA schema1 TO xxxpoc;
GRANT ALL ON SCHEMA schema1 TO public;
COMMENT ON SCHEMA model IS 'for xxx migration  poc test';

CREATE SCHEMA oss_external_table
  AUTHORIZATION xxxpoc;
			
```

**CREATE FUNCTION**

Because Redshift provides some SQL functions of which the corresponding functions are not yet supported in HybridDB, you can choose to customize these functions or rewrite them. Specific examples are described as follows.

-   Replace `CONVERT_TIMEZONE(a,b,c)` with following code:

    ```
    timezone(b, timezone(a,c))
    ```

-   Replace GETDATE\(\) with following code:

    ```
    current_timestamp(0):timestamp
    ```

-   Replace and optimize user defined functions \(UDFs\).

    For example, a SQL function of Redshift is as follows:

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

    Replace the preceding function with the following SQL statement:

    ```
    to_char(a - interval '4 hour', 'yyyy-mm-dd')
    ```

-   Other Redshift standard SQL functions.

    In your actual scenario, we recommend that you query the standard SQL function library of PostgreSQL at [Functions and Operators in PostgreSQL8.2](https://www.postgresql.org/docs/8.2/functions.html). In doing so, you can determine which functions you need to manually modify and implement so that they are compatible with AnalyticDB for PostgreSQL. The following is a list of commonly used functions:

-   -   [ISNULL\(\)](https://docs.microsoft.com/en-us/sql/t-sql/functions/isnull-transact-sql?view=sql-server-2017)
-   [DATEADD\(\)](https://docs.aws.amazon.com/redshift/latest/dg/r_DATEADD_function.html)
-   [DATEDIFF\(\)](https://docs.aws.amazon.com/redshift/latest/dg/r_DATEDIFF_function.html)
-   [REGEXP\_COUNT\(\)](https://docs.aws.amazon.com/redshift/latest/dg/REGEXP_COUNT.html)
-   [LEFT\(\)](https://docs.aws.amazon.com/redshift/latest/dg/r_LEFT.html)
-   [RIGHT\(\)](https://docs.aws.amazon.com/redshift/latest/dg/r_LEFT.html)

**CREATE TABLE**

-   Change compression encoding. AnalyticDB for PostgreSQL does not support the full list of [Redshift Compression Encoding](https://docs.aws.amazon.com/redshift/latest/dg/c_Compression_encodings.html). Compression encodings which are not supported are listed as follows:

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
    ENCODE XXX should be removed and replaced with following option in CREATE TABLE statement:

    ```
    with (COMPRESSTYPE={ZLIB|QUICKLZ|RLE_TYPE|NONE})
    ```

-   Change distribution keys. Redshift supports three types of distribution keys. For more information, see [Distribution Styles](https://docs.aws.amazon.com/redshift/latest/dg/c_choosing_dist_sort.html). The following information indicates the rules you need to apply to modify the distribution keys so that the keys are compatible with AnalyticDB for PostgreSQL.
    -   DISTSTYLE EVEN: Replace with `distributed randomly`
    -   DISTKEY: Replace with `distributed by (colname1,...)`
    -   ALL: Remove \(not supported\)

Change SORT key. Replace the COMPOUND or INTERLEAVED options in Redshift sort key clause ``[ COMPOUND | INTERLEAVED ] SORTKEY (column_name [, ...] ) ]``with following clause:

```
with(APPENDONLY=true,ORIENTATION=column)
sortkey (volume);
			
```

**Example 1**

The following statement is a CREATE TABLE statement that conforms to Redshift syntax:

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

After conversion, the CREATE TABLE statement that conforms to the AnalyticDB for PostgreSQL syntax is as follows:

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

**Example 2**

The following statement is a CREATE TABLE statement that conforms to Redshift syntax. It includes the ENCODE and SORTKEY options:

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

After conversion, the CREATE TABLE statement that conforms to the AnalyticDB for PostgreSQL syntax is as follows:

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

**CREATE VIEW**

Similar to the CREATE TABLE statements in the preceding section, if you need to use a CREATE VIEW statement, you need to first convert the statement so that it conforms to the AnalyticDB for PostgreSQL syntax.

## Create and Configure a AnalyticDB for PostgreSQL instance {#section_vwd_jct_jgb .section}

For more information, see:

-   [Create an instance](intl.en-US/Quick Start/Create an instance.md#)
-   [Set up a whitelist](intl.en-US/Quick Start/Set up an instance/Set up a whitelist.md#)
-   [Set up an account](intl.en-US/Quick Start/Set up an instance/Set up an account.md#)

## Create Database Objects {#section_xwd_jct_jgb .section}

Follow the instructions in [Connect to a AnalyticDB for PostgreSQL database](intl.en-US/Quick Start/Connect to a AnalyticDB for PostgreSQL database.md#), and use **psql** or **pgAdmin III 1.6.3** to connect to an instance.

Then, modify the DDL statements in Redshift syntax to DDL statements that conform to the AnalyticDB for PostgreSQL syntax, and then execute these DDL statements to create database objects.

## CREATE EXTERNAL TABLE {#section_ywd_jct_jgb .section}

AnalyticDB for PostgreSQL supports parallel import from OSS and export to OSS through external tables \(which is called the gpossext function\). It can also compress external table files in gzip format to reduce the storage space and the costs. The gpossext function can read or write text and csv files, or text and csv files in gzip format. For more information, see [Parallel import from OSS or export to OSS](intl.en-US/Quick Start/Import data/Parallel import from OSS or export to OSS.md#).

## Import data by using INSERT INTO script {#section_zwd_jct_jgb .section}

After external tables in OSS and database objects in AnalyticDB for PostgreSQL are created, you need to prepare an INSERT script to import data from the external tables to the target tables in AnalyticDB for PostgreSQL. Then, you need to save the INSERT script as insert.sql, and then execute this file.

The format of the INSERT statement is `INSERT INTO <TABLE NAME> SELECT * FROM <OSS EXTERNAL TABLE NAME>;`.

Example:

```
INSERT INTO schema1.table1 SELECT * FROM oss_external_table.table1;
```

After the import is completed, you can use SELECT statements to verify the imported data and compare them with the source data.

## Run a VACUUM script to defragment the database {#section_axd_jct_jgb .section}

After the external tables in OSS are imported into AnalyticDB for PostgreSQL, you need to defragment the database by running `VACUUM` script. Then, you need to save the VACUUM script as vacuum.sql, and then execute this file. For more information about VACUUM, see [VACUUM](https://www.postgresql.org/docs/8.2/sql-vacuum.html).

