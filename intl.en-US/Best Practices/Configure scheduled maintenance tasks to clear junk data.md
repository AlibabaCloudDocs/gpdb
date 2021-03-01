# Configure scheduled maintenance tasks to clear junk data

When you perform system update operations such as INSERT VALUES, UPDATE, DELETE, and ALTER TABLE ADD COLUMN, junk data is retained in the system table and updated data table. The junk data diminishes the system performance and consumes a large amount of disk space. Therefore, we recommend that you clear such data on a regular basis. This topic describes how to clear junk data for AnalyticDB for PostgreSQL.

## Clear junk data without locking tables

You can clear some junk data without locking their corresponding tables by using the following method:

-   Log on to each database as the database owner, and execute the `VACUUM` statement.

-   Frequency:

    -   When you perform real-time update operations, such as INSERT VALUES, UPDATE, and DELETE, we recommend that you execute the VACUUM statement once a day or at least once a week.
    -   If you only perform batch updates once a day, we recommend that you execute the VACUUM statement once a week or at least once a month.
-   The system does not lock tables on which the VACUUM statement is being executed. You can read data from the tables or write data into them when the VACUUM statement is executed. However, this operation increases CPU utilization and I/O usage, and may affect query performance.

-   You can run the following Linux Shell script file as a scheduled crontab task:


```
#! /bin/bash
export PGHOST=myinst.gpdb.rds.tbsite.net
export PGPORT=3432
export PGUSER=myuser
export PGPASSWORD=mypass
#do not echo command, just get a list of db
dblist=`psql -d postgres -c "copy (select datname from pg_stat_database) to stdout"`
for db in $dblist ; do
    #skip the system databases
    if [[ $db == template0 ]] ||  [[ $db == template1 ]] || [[ $db == postgres ]] || [[ $db == gpdb ]] ; then
        continue
    fi
    echo processing $db
    #vacuum all tables (catalog tables/user tables)
    psql -d $db -e -a -c "VACUUM;"
done
```

## Clear junk data during a maintenance window

You can execute the VACUUM FULL statement to clear all junk data in all tables during a maintenance window by using the following method:

-   Log on to each database as the database owner. The database owner must have full permissions.

    1.  Execute the `REINDEX SYSTEM <database name>` statement.
    2.  Execute the `VACUUM FULL <table name>` and `REINDEX TABLE <table name>` statements on each data table.
    3.  If you create and delete system tables and indexes at a high frequency, we recommend that you execute the VACUUM FULL <table name\> statement to maintain the tables on a regular basis. The system tables include pg\_class, pg\_attribute, and pg\_index. Note: We recommend that you do not access the database when you execute the VACUUM FULL <table name\> statement.
-   The VACUUM FULL statement must be executed at least once a week. If the majority of your data is updated every day, execute the VACUUM FULL statement once a day.

-   The system locks tables on which the VACUUM FULL or REINDEX statement is being executed and these tables cannot be read or written during the execution. This operation increases CPU utilization and I/O usage.

-   You can run the following Linux Shell script file as a scheduled crontab task:


```
#! /bin/bash
export PGHOST=myinst.gpdb.rds.tbsite.net
export PGPORT=3432
export PGUSER=myuser
export PGPASSWORD=mypass
#do not echo command, just get a list of db
dblist=`psql -d postgres -c "copy (select datname from pg_stat_database) to stdout"`
for db in $dblist ; do
    #skip system databases
    if [[ $db == template0 ]] ||  [[ $db == template1 ]] || [[ $db == postgres ]] || [[ $db == gpdb ]] ; then
        continue
    fi
    echo processing db "$db"
    #do a normal vacuum
    psql -d $db -e -a -c "VACUUM;"
    #reindex system tables firstly
    psql -d $db -e -a -c "REINDEX SYSTEM $db;"
    #use a temp file to store the table list, which could be vary large
    cp /dev/null tables.txt
    #query out only the normal user tables, excluding partitions of parent tables
    psql -d $db -c "copy (select '\"'||tables.schemaname||'\".' || '\"'||tables.tablename||'\"' from (select nspname as schemaname, relname as tablename from pg_catalog.pg_class, pg_catalog.pg_namespace, pg_catalog.pg_roles where pg_class.relnamespace = pg_namespace.oid and pg_namespace.nspowner = pg_roles.oid and pg_class.relkind='r' and (pg_namespace.nspname = 'public' or pg_roles.rolsuper = 'false' ) ) as tables(schemaname, tablename) left join pg_catalog.pg_partitions on pg_partitions.partitionschemaname=tables.schemaname and pg_partitions.partitiontablename=tables.tablename where pg_partitions.partitiontablename is null) to stdout;" > tables.txt
    while read line; do
        #some table name may contain the $ sign, so escape it
        line=`echo $line |sed 's/\\\$/\\\\\\\$/g'`
        echo processing table "$line"
        #vacuum full this table, which will lock the table
        psql -d $db -e -a -c "VACUUM FULL $line;"
        #reindex the table to reclaim index space
        psql -d $db -e -a -c "REINDEX TABLE $line;"
    done <tables.txt
done
```

