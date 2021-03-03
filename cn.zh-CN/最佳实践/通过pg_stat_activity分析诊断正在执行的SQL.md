# 通过pg\_stat\_activity分析诊断正在执行的SQL

pg\_stat\_activity是一个非常有用的视图，可以分析排查当前运行的SQL任务以及一些异常问题。pg\_stat\_activity 每行展示的是一个“process” 的相关信息，这里的“process”可以理解为一个用户连接。

pg\_stat\_activity视图的字段描述如下

|字段|类型|描述|
|--|--|--|
|datid|oid|这个后端连接到的数据库的OID|
|datname|name|这个后端连接到的数据库的名称|
|procpid|integer|这个后端的进程 ID （注：4.3 版本支持的字段定义）|
|pid|integer|这个后端的进程 ID （注：6.0 版本支持的字段定义）|
|sess\_id|integer|登录到这个后端的用户的session id|
|usesysid|oid|登录到这个后端的用户的 OID|
|usename|name|登录到这个后端的用户的名称|
|current\_query|text|注：4.3 版本支持的字段定义

这个域显示当前正在执行的查询。 默认情况下，查询文本被截断为1024个字符；可以通过参数 track\_activity\_query\_size更改此值|
|query|text|注：6.0版本支持的字段定义

这个后端最近查询的文本。如果state为active，这个域显示当前正在执行的查询。在所有其他状态下，它显示上一个被执行的查询。 默认情况下，查询文本被截断为1024个字符；可以通过参数 track\_activity\_query\_size更改此值|
|waiting|boolean|True，如果当前SQL在锁等待，否则 false|
|query\_start| |当前活动查询被开始的时间，如果state不是active，这个域为上一个查询被开始的时间|
|backend\_start| |当前后台进程开始的时间|
|client\_addr|inet|连接到这个后端的客户端的 IP 地址。如果这个域为空，它表示客户端通过服务器机器上的一个 Unix 套接字连接或者这是一个内部进程（如自动清理）。|
|client\_port|integer|客户端用以和这个后端通信的 TCP 端口号，如果使用 Unix 套接字则为-1|
|application\_name|text|连接到这个后端的应用的名称|
|xact\_start| |这个进程的当前事务被启动的时间，如果没有活动事务则为空。如果当前查询是它的第一个事务，这一列等于query\_start。|
|waiting\_reason|text|当前执行等待的原因，可能是等锁或者等待节点间数据的复制|
|state|text|注：只有6.0版本支持

这个后端目前的状态，包括 active，idle，idle in transaction，idle in transaction \(aborted\)，fastpath function call，disabled|
|state\_change|timestampz|注：只有6.0版本支持

上次state状态切换的时间|

## 查看连接信息

通过下述SQL就能确认当前的连接用户和对应的连接机器。

```
postgres=# SELECT datname,usename,client_addr,client_port FROM pg_stat_activity ;
datname  |  usename  |  client_addr   | client_port
----------+-----------+----------------+-------------
 postgres | joe       |  xx.xx.xx.xx   |       60621
 postgres | gpmon     |  xx.xx.xx.xx   |       60312
(9 rows)
```

## 查看SQL运行信息

获取当前用户执行SQL信息：

```
 4.3 版本：
postgres=# SELECT datname,usename,current_query FROM pg_stat_activity ;
 datname  | usename  |                        current_query
----------+----------+--------------------------------------------------------------
 postgres | postgres | SELECT datname,usename,current_query FROM pg_stat_activity ;
 postgres | joe      | <IDLE>
(2 rows)

6.0 版本：
postgres=# SELECT datname,usename,query FROM pg_stat_activity ;
 datname  | usename  |                        query
----------+----------+--------------------------------------------------------------
 postgres | postgres | SELECT datname,usename,query FROM pg_stat_activity ;
 postgres | joe      | 
(2 rows)
```

只看当前正在运行的SQL信息：

```
4.3 版本
SELECT datname,usename,current_query
   FROM pg_stat_activity
   WHERE current_query != '<IDLE>' ;

6.0 版本
SELECT datname,usename,query
   FROM pg_stat_activity
   WHERE state != 'idle' ;
```

## 查看耗时较长的查询

查看当前运行中的耗时较长的SQL语句

```
4.3 版本
select current_timestamp - query_start as runtime, datname, usename, current_query
    from pg_stat_activity
    where current_query != '<IDLE>'
    order by 1 desc;

6.0 版本
select current_timestamp - query_start as runtime, datname, usename, query
    from pg_stat_activity
    where state != 'idle'
    order by 1 desc;
```

例如如下返回：

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
                                             :     where current_query != '<IDLE>'
                                             :     order by 1 desc;
(2 rows)
```

可以看到第一个查询耗时较久，已经运行了34s还没有结束。

## 异常SQL诊断及修复

如果一个SQL运行很长时间没有结果，需要检查该SQL还在运行中还是已经被block了：

```
4.3 版本
SELECT datname,usename,current_query
   FROM pg_stat_activity
   WHERE waiting;

6.0 版本
SELECT datname,usename,query
   FROM pg_stat_activity
   WHERE waiting;
```

需要注意的是这个输出只能获取当前因为lock而被block的SQL，因为其他原因被block的SQL这里获取不到。绝大多数情况下SQL都是因为lock而被block，但也会有一些其他情况例如等待i/o、定时器等。如果上述SQL有结果输出说明有SQL被lock阻塞，进一步明确相互block的SQL信息：

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

通过这个SQL的输出信息就能确认相互block的SQL和对应的执行pid。在明确了SQL的阻塞信息后，可以通过cancel或者kill query的方式进行恢复。通过cancel取消一个正在运行的query：

```
SELECT pg_cancel_backend(pid)
```

需要在一个运行query的session中执行，如果session本身就是idle的，执行不起作用。另外取消这个query需要花费一定的时间来做清理和事务的回滚。使用pg\_terminate\_backend来清理idle session，也可以用来终止query：

```
SELECT pg_terminate_backend(pid);
```

该用户的连接会被断开。尽量避免在正在运行query的进程pid上执行。需要注意的是文中提到操作需要用户有superuser的权限。

