# Use pg\_stat\_activity to analyze and diagnose running PostgreSQL queries

pg\_stat\_activity is a helpful PostgreSQL system view. You can use pg\_stat\_activity to analyze and diagnose running PostgreSQL tasks and troubleshoot problems. The pg\_stat\_activity view shows a row for each server process or connection to the database.

The following table describes the fields of the pg\_stat\_activity view.

|Field|Type|Description|
|-----|----|-----------|
|datid|oid|The object identifier \(OID\) of the database to which the backend is connected.|
|datname|name|The name of the database to which the backend is connected.|
|procpid|integer|The ID of the backend process. Note that only AnalyticDB for PostgreSQL V4.3 supports this field.|
|pid|integer|The ID of the backend process. Note that only AnalyticDB for PostgreSQL V6.0 supports this field.|
|sess\_id|integer|The session ID of the user who logs on to the backend.|
|usesysid|oid|The OID of the user who logs on to the backend.|
|usename|name|The name of the user who logs on to the backend.|
|current\_query|text|Note that only AnalyticDB for PostgreSQL V4.3 supports this field.

The currently executing query in the backend. By default, the query text is truncated to 1,024 characters in length. You can specify the track\_activity\_query\_size parameter to change the maximum number of characters.|
|query|text|Note that only AnalyticDB for PostgreSQL V6.0 supports this field.

The last executed query in the backend. If the state parameter is set to active, the currently executing query is displayed in the backend. If the state parameter is set to a value other than active, the last executed query is displayed in the backend. By default, the query text is truncated to 1,024 characters in length. You can specify the track\_activity\_query\_size parameter to change the maximum number of characters.|
|waiting|boolean|Indicates whether the current SQL query is waiting on a lock. Valid values: True and False.|
|query\_start| |The time when the currently active query was started. If the state field is not set to active, the query\_start field indicates the time when the last query was started.|
|backend\_start| |The time when the current backend process was started.|
|client\_addr|inet|The IP address of the client that is connected to this backend. If this field is empty, the client is connected through a Unix socket on the server or this is an internal process such as autovacuum.|
|client\_port|integer|The TCP port number that the client uses for communication with the backend server. A value of -1 indicates that a Unix socket is used.|
|application\_name|text|The name of the application that is connected to this backend.|
|xact\_start| |The time when the current transaction of this process was started, or null if no transaction is active. If the current query is the first transaction of the process, this field is equivalent to the query\_start field.|
|waiting\_reason|text|The reason why the backend is currently waiting. The backend is currently waiting on a lock or on replication of data among nodes.|
|state|text|Note that only AnalyticDB for PostgreSQL V6.0 supports this field.

The current state of the backend. Valid values: active, idle, idle in transaction, idle in transaction \(aborted\), fastpath function call, and disabled.|
|state\_change|timestampz|Note that only AnalyticDB for PostgreSQL V6.0 supports this field.

The time when the previous state was changed.|

## View connection information

You can execute the following SQL statements to view usernames and the corresponding clients:

```
postgres=# SELECT datname,usename,client_addr,client_port FROM pg_stat_activity ;
datname  |  usename  |  client_addr   | client_port
----------+-----------+----------------+-------------
 postgres | joe       |  xx.xx.xx.xx   |       60621
 postgres | gpmon     |  xx.xx.xx.xx   |       60312
(9 rows)
```

## Query the SQL execution information

You can execute the following SQL statements to view current queries executed by users who connect to the database:

```
 AnalyticDB for PostgreSQL V4.3:
postgres=# SELECT datname,usename,current_query FROM pg_stat_activity ;
 datname  | usename  |                        current_query
----------+----------+--------------------------------------------------------------
 postgres | postgres | SELECT datname,usename,current_query FROM pg_stat_activity ;
 postgres | joe      | <IDLE>
(2 rows)

AnalyticDB for PostgreSQL V6.0:
postgres=# SELECT datname,usename,query FROM pg_stat_activity ;
 datname  | usename  |                        query
----------+----------+--------------------------------------------------------------
 postgres | postgres | SELECT datname,usename,query FROM pg_stat_activity ;
 postgres | joe      |
(2 rows)
```

You can execute the following SQL statements to view only active queries:

```
AnalyticDB for PostgreSQL V4.3:
SELECT datname,usename,current_query
   FROM pg_stat_activity
   WHERE current_query ! = '<IDLE>' ;

AnalyticDB for PostgreSQL V6.0:
SELECT datname,usename,query
   FROM pg_stat_activity
   WHERE state ! = 'idle' ;
```

## View long-running queries

You can execute the following SQL statements to view long-running queries:

```
AnalyticDB for PostgreSQL V4.3:
select current_timestamp - query_start as runtime, datname, usename, current_query
    from pg_stat_activity
    where current_query ! = '<IDLE>'
    order by 1 desc;

AnalyticDB for PostgreSQL V6.0:
select current_timestamp - query_start as runtime, datname, usename, query
    from pg_stat_activity
    where state ! = 'idle'
    order by 1 desc;
```

Sample responses:

```
runtime     |    datname     | usename  |                                current_query
-----------------+----------------+----------+------------------------------------------------------------------------------
 00:00:34.248426 | tpch_1000x_col | postgres | select
                                             :         l_returnflag,
                                             :         l_linestatus,
                                             :         sum(l_quantity) as sum_qty,
                                             :         sum(l_extendedprice) as sum_base_price,
                                             :         sum(l_extendedprice * (1 - l_discount)) as sum_disc_price,
                                             :         sum(l_extendedprice * (1 - l_discount) * (1 + l_tax)) as sum_charge,
                                             :         avg(l_quantity) as avg_qty,
                                             :         avg(l_extendedprice) as avg_price,
                                             :         avg(l_discount) as avg_disc,
                                             :         count(*) as count_order
                                             : from
                                             :         public.lineitem
                                             : where
                                             :         l_shipdate <= date '1998-12-01' - interval '93' day
                                             : group by
                                             :         l_returnflag,
                                             :         l_linestatus
                                             : order by
                                             :         l_returnflag,
                                             :         l_linestatus;
 00:00:00        | postgres       | postgres | select
                                             :        current_timestamp - query_start as runtime,
                                             :        datname,
                                             :        usename,
                                             :        current_query
                                             :     from pg_stat_activity
                                             :     where current_query ! = '<IDLE>'
                                             :     order by 1 desc;
(2 rows)
```

The first returned query runs for a long time. The first query has been running for 34 seconds and continues to run.

## Diagnose and troubleshoot abnormal SQL queries

If an SQL query has been running for an extended period of time without returning any results, you must check whether the query is running or blocked.

```
AnalyticDB for PostgreSQL V4.3:
SELECT datname,usename,current_query
   FROM pg_stat_activity
   WHERE waiting;

AnalyticDB for PostgreSQL V6.0:
SELECT datname,usename,query
   FROM pg_stat_activity
   WHERE waiting;
```

Note that the output shows only SQL queries that are blocked due to lock waits. In most cases, SQL queries are blocked due to lock waits. However, in some other cases, SQL queries are blocked because they are waiting for I/O operations or timers. If the preceding SQL statements have returned responses, it indicates that specific SQL queries are blocked due to lock waits. You can execute the following statements to view details about these SQL queries:

```
SELECT
       w.current_query as waiting_query,
       w.procpid as w_pid,
       w.usename as w_user,
       l.current_query as locking_query,
       l.procpid as l_pid,
       l.usename as l_user,
       t.schemaname || '.' || t.relname as tablename
    from pg_stat_activity w
    join pg_locks l1 on w.procpid = l1.pid and not l1.granted
    join pg_locks l2 on l1.relation = l2.relation and l2.granted
    join pg_stat_activity l on  l2.pid = l.procpid
    join pg_stat_user_tables t on l1.relation = t.relid
    where w.waiting;
```

The output shows blocked SQL queries and their corresponding process IDs. You can review the details of the blocked SQL queries and then cancel or kill the queries to remove the blocks. You can execute the following statement to cancel a running query:

```
SELECT pg_cancel_backend(pid)
```

The pg\_cancel\_backend function will take effect only on a session with a running query. The function will not do anything when the session is idle. In addition, canceling the query may take some time depending on the cleanup or rollback of the transactions. pg\_terminate\_backend can be used to terminate running queries or idle sessions.

```
SELECT pg_terminate_backend(pid);
```

Connections initiated by the specified user are terminated. We recommend that you do not run the pg\_terminate\_backend function on processes identified by pid where active queries exist. You must have superuser permissions to perform the operations in this topic.

