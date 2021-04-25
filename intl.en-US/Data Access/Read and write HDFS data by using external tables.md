# Read and write HDFS data by using external tables

Note: This feature is expected to go online in June.

AnalyticDB for PostgreSQL allows you to read data from and write data to a Hadoop cluster by using external tables and the gphdfs protocol. This topic describes how to read and write Hadoop Distributed File System \(HDFS\) data by using the gphdfs protocol in AnalyticDB for PostgreSQL.

This topic includes the following sections:

-   Create an HDFS test file
-   Create a readable HDFS external table and read data
-   Create a writable HDFS external table and write data
-   Access Hive by using external tables
-   Access HBase by using external tables

## Create an HDFS test file

Log on to an HDFS cluster and run the following commands to create a test file in a test directory:

```
root@namenode:/# hadoop fs -mkdir /test #Create a test folder in the root directory.
root@namenode:/# echo "1 abc" > data.txt #Create a test file.
root@namenode:/# hadoop fs -put local.txt /test #Upload the test file to HDFS.
root@namenode:/# hadoop fs -cat /test/data.txt #View the test file content.
1 abc
```

## Create a readable HDFS external table and read data

In the CREATE EXTERNAL TABLE statement, specify the HDFS cluster endpoint that uses the gphdfs external table protocol, test file path, file format, and delimiter.

```
postgres=# CREATE READABLE EXTERNAL TABLE test (id int, name text)
LOCATION ('gphdfs://namenode_IP:port/test/data.txt')
FORMAT 'text' (delimiter ' ');
```

The following code provides an example on how to read data from an external table:

```
postgres=# select * from test;
 id | name
----+------
  1 | abc
(1 row)
```

## Create a writable HDFS external table and write data

In the CREATE EXTERNAL TABLE statement, declare WRITABLE to specify that the external table is writable.

```
postgres=# create WRITABLE external table test_write (id int, name text)
location('gphdfs://namenode_IP:port/test/data.txt')
FORMAT 'text' (delimiter ' ');
```

Execute the INSERT statement to write data.

```
postgres=# insert into test_write values(2, 'def');
```

Run the following command to view the test file content in the HDFS cluster and check whether the data that is written to the file is valid:

```
root@namenode:/# hadoop fs -cat /test/data.txt #View the test file content.
1 abc
2 def
```

