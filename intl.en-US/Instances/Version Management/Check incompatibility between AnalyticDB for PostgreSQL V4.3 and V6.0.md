# Check incompatibility between AnalyticDB for PostgreSQL V4.3 and V6.0

AnalyticDB for PostgreSQL V6.0 has configurations that are incompatible with V4.3. To upgrade your AnalyticDB for PostgreSQL instances from V4.3 to V6.0, you must preprocess the incompatible configurations. This topic describes how to run a shell script to check incompatibility between AnalyticDB for PostgreSQL V4.3 and V6.0. A Linux operating system is used in this example. You can refer to the SQL statements in the script for a Windows operating system.

## Precautions

The check items do not include SQL statements of your business, custom stored procedures or functions, or custom views. You can verify the excluded items in AnalyticDB for PostgreSQL V6.0 instances.

## Sample shell script

```
#! /bin/bash

#
# Copyright (c) 2020, Alibaba Group, Inc.
#
# Description:  check unsupported items before upgrade instance.
# Usage:        sh 4x_to_6x_check.sh <PGHOST> <PGPORT> <PGUSER> <PGPASSWORD>
# CheckList:
#          1) Check the version of the AnalyticDB for PostgreSQL instance.
#          2) Check the libraries that are not transferred.
#          3) Check the unsupported distribution keys for tables.
#          4) Check the unsupported field types for tables.
#          5) Check the unsupported extensions
# Notice: If one or more error messages are returned, you must modify the corresponding configurations.
#

if [[ $# -lt 4 ]]; then
    echo "Usage: $0 <PGHOST> <PGPORT> <PGUSER> <PGPASSWORD>"
    exit 1
fi
export PGHOST=$1
export PGPORT=$2
export PGUSER=$3
export PGPASSWORD=$4

db_ver=`psql -d postgres -c "copy (select version()) to stdout"`
db_names=`psql -d postgres -c "copy (select sodddatname from gp_toolkit.gp_size_of_database) to stdout"`
db_names=(${db_names})
db_len=${#db_names[@]}

unsupport6x_ext="('fastann','feature_extractor','varbitx')"
unsupport6x_disted_type="('money','tinterval')"
unsupport6x_type="('unknown')"

# Check the version of the AnalyticDB for PostgreSQL instance.
check_version()
{
  echo ''
  echo $db_ver
  echo ''
  echo '********** check base version...'
  base_time=`date -d "2020-08-31" +%s`
  db_verdate=${db_ver##*compiled on}
  seconds=`date -d "$db_verdate" +%s`
  if [[ $seconds -lt $base_time ]]; then
    echo 'ERROR: please upgrade minor version...'
  else
    echo 'pass......'
  fi
}

# Check the libraries that are not transferred.
check_libraries()
{
  echo ''
  echo '********** check untransferred libraries...'
  count=`psql -d postgres -c "copy (select count(1) from pg_catalog.pg_library) to stdout"`
  if [[ $count -gt 0 ]]; then
    psql -d postgres -c "select name,lanname language from pg_catalog.pg_library;"
    echo "WARN: please transfer libraries manually..."
  else
    echo 'pass......'
  fi
}

# Check the unsupported distribution keys for tables.
check_table_did()
{
  echo ''
  echo '********** check unsupported table distributedId types...'
  count=0
  if [[ $db_ver == *8.2*4.3* ]]; then
    for ((i=0; i<$db_len; ++i)); do
      sql="select count(1) from pg_catalog.pg_class c,pg_catalog.pg_attribute a,pg_catalog.pg_type t,pg_catalog.gp_distribution_policy p where
      a.atttypid=t.oid and a.attrelid=c.oid and p.localoid=c.oid and a.attnum=any(p.attrnums) and a.attnum>0 and t.typname in $unsupport6x_disted_type"
      count1=`psql -d ${db_names[$i]} -c "copy ($sql) to stdout"`
      count=$((count + count1))
      if [[ $count1 -gt 0 ]]; then
        sql="select '${db_names[$i]}' dbname,n.nspname schema,c.relname table_name,a.attname distributed_field,t.typname field_type from
        pg_catalog.pg_namespace n,pg_catalog.pg_class c,pg_catalog.pg_attribute a,pg_catalog.pg_type t,pg_catalog.gp_distribution_policy p
        where a.atttypid=t.oid and n.oid=c.relnamespace and a.attrelid=c.oid and p.localoid=c.oid and a.attnum=any(p.attrnums) and a.attnum>0 and t.typname in $unsupport6x_disted_type order by schema,table_name;"
        psql -d ${db_names[$i]} -c "$sql"
      fi
    done
  fi
  if [[ $count -gt 0 ]]; then
    echo 'ERROR: please alter table distributedId types manually...'
  else
    echo 'pass......'
  fi
}

# Check the unsupported field types for tables.
check_table_ftype()
{
  echo ''
  echo '********** check unsupported table field types...'
  count=0
  if [[ $db_ver == *8.2*4.3* ]]; then
    for ((i=0; i<$db_len; ++i)); do
      sql="select count(1) from pg_catalog.pg_class c,pg_catalog.pg_attribute a,pg_catalog.pg_type t where
      a.atttypid=t.oid and a.attrelid=c.oid and a.attnum>0 and t.typname in $unsupport6x_type"
      count1=`psql -d ${db_names[$i]} -c "copy ($sql) to stdout"`
      count=$((count + count1))
      if [[ $count1 -gt 0 ]]; then
        sql="select '${db_names[$i]}' dbname,n.nspname schema,c.relname table_name,a.attname field_name,t.typname field_type from
        pg_catalog.pg_namespace n,pg_catalog.pg_class c,pg_catalog.pg_attribute a,pg_catalog.pg_type t
        where a.atttypid=t.oid and n.oid=c.relnamespace and a.attrelid=c.oid and a.attnum>0 and t.typname in $unsupport6x_type order by schema,table_name;"
        psql -d ${db_names[$i]} -c "$sql"
      fi
    done
  fi
  if [[ $count -gt 0 ]]; then
    echo 'ERROR: please alter table field types manually...'
  else
    echo 'pass......'
  fi
}

# Check the unsupported extensions.
check_extensions()
{
  echo ''
  echo '********** check unsupported extensions...'
  count=0
  if [[ $db_ver == *8.2*4.3* ]]; then
    for ((i=0; i<$db_len; ++i)); do
      count1=`psql -d ${db_names[$i]} -c "copy (select count(1) from pg_catalog.pg_extension where extname in $unsupport6x_ext) to stdout"`
      count=$((count + count1))
      if [[ $count1 -gt 0 ]]; then
        psql -d ${db_names[$i]} -c "select '${db_names[$i]}' dbname,extname,extversion from pg_catalog.pg_extension where extname in $unsupport6x_ext;"
      fi
    done
  fi
  if [[ $count -gt 0 ]]; then
    echo 'WARN: please drop useless extensions manually...'
    echo 'REF DROP EXTENSION SQL: drop extension <name> '
  else
    echo 'pass......'
  fi
}

check_version
check_libraries
check_table_did
check_table_ftype
check_extensions
```

|Parameter|Description|
|---------|-----------|
|<PGHOST\>|The IP address that is used to connect to the AnalyticDB for PostgreSQL V4.3 instance.|
|<PGPORT\>|The port number that is used to connect to the AnalyticDB for PostgreSQL V4.3 instance.|
|<PGUSER\>|The username that is used to connect to the AnalyticDB for PostgreSQL V4.3 instance.|
|<PGPASSWORD\>|The password for the user.|

## Procedure

1.  Run the following command to install a PostgreSQL client on your Linux device:

    ```
    sudo yum install postgresql
    ```

2.  View the public IP address of the Linux device. Add the public IP address of the Linux device to an IP address whitelist for the AnalyticDB for PostgreSQL V4.3 instance in the [AnalyticDB for PostgreSQL console](https://gpdbnext.console.aliyun.com/). For more information about how to configure an IP address whitelist, see [Configure an IP address whitelist for an AnalyticDB for PostgreSQL instance](/intl.en-US/Quick Start/Configure an IP address whitelist for an AnalyticDB for PostgreSQL instance.md).

3.  Connect to the AnalyticDB for PostgreSQL V4.3 instance by using the Linux device.

    ```
    psql -h <PGHOST> -p <PGPORT> -U <PGUSER>
    ```

4.  Save the edited shell script as a script file such as 4x\_to\_6x.sh. In this example, run the following command to execute the 4x\_to\_6x\_check.sh script for incompatibility check:

    ```
    sh 4x_to_6x_check.sh <PGHOST> <PGPORT> <PGUSER> <PGPASSWORD>
    ```

5.  Modify the incompatible items of the AnalyticDB for PostgreSQL V4.3 instance based on one or more returned error messages. After you modify the items, execute the script again to check whether the check items are compatible with AnalyticDB for PostgreSQL V6.0.


## Sample check results

-   **A "pass" message without "ERROR" indicates that the check items are compatible with AnalyticDB for PostgreSQL V6.0. The following code shows that all check items are compatible with AnalyticDB for PostgreSQL V6.0:**

    ```
    ********** check base version...
    pass......
    
    ********** check untransferred libraries...
    pass......
    
    ********** check unsupported table distributedId types...
    pass......
    
    ********** check unsupported table field types...
    pass......
    
    ********** check unsupported extensions...
    pass......
    ```


-   **If the results contain one or more error messages, you must modify the incompatible configurations. The following code shows that all check items are incompatible with AnalyticDB for PostgreSQL V6.0:**

    ```
    
    PostgreSQL 8.2.15 (Greenplum Database 4.3.99.00 build dev) compiled on May 2 2020 09:35:15
    
    ********** check base version...
    ERROR: please upgrade minor version...
    
    ********** check untransferred libraries...
       name   | language 
    ----------+----------
     select_1 | plpgsql
    (1 row)
    
    WARN: please transfer libraries manually...
    
    ********** check unsupported table distributedId types...
     dbname | schema | table_name | distributed_field | field_type 
    --------+--------+------------+-------------------+------------
     adbpg  | public | test1      | id                | money
    (1 row)
    
    ERROR: please alter table distributedId types manually...
    
    ********** check unsupported table field types...
     dbname | schema | table_name | field_name | field_type 
    --------+--------+------------+------------+------------
     adbpg  | public | test2      | name       | unknown
    (1 row)
    
    ERROR: please alter table field types manually...
    
    ********** check unsupported extensions...
     dbname | extname | extversion 
    --------+---------+------------
     adbpg  | varbitx | 1.0
    (1 row)
    
    WARN: please drop useless extensions manually...
    REF DROP EXTENSION SQL: drop extension <name> 
    ```

    |Error message|Modification method|
    |-------------|-------------------|
    |**ERROR: please upgrade minor version...**|**Upgrade the minor version** of the AnalyticDB for PostgreSQL V4.3 instance in the [AnalyticDB for PostgreSQL console](https://gpdbnext.console.aliyun.com/). For more information, see [Update the kernel of an AnalyticDB for PostgreSQL instance](/intl.en-US/Instances/Version Management/Update the kernel of an AnalyticDB for PostgreSQL instance.md).|
    |**WARN: please transfer libraries manually...**|Migrate the libraries that are used in the AnalyticDB for PostgreSQL V4.3 instance.|
    |**ERROR: please alter table distributedId types manually...**|Modify the incompatible distribution keys in the AnalyticDB for PostgreSQL V4.3 instance.|
    |**ERROR: please alter table field types manually...**|Modify the incompatible field types in the AnalyticDB for PostgreSQL V4.3 instance.|
    |**WARN: please drop useless extensions manually...**|If you need to use the tables or stored procedures that are relevant to the incompatible extensions, modify the configurations of the extensions. If the incompatible extensions are not used in AnalyticDB for PostgreSQL V6.0, delete the extensions.|


