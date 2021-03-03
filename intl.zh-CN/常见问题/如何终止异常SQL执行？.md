# 如何终止异常SQL执行？

若需要终止特定的SQL或会话来恢复系统状态，先通过`pg_stat_activity`系统视图查询当前的查询：

```
select current_query,procpid from pg_stat_activity ;

                    current_query                    | procpid
-----------------------------------------------------+---------
 select current_query,procpid from pg_stat_activity; |   32584
 SELECT xxx                                          |   32238
```

**说明：** `current_query`为当前正在进行的查询，`procpid`为后台进程的PID。

终止非当前连接的SELECT：

```
SELECT pg_cancel_backend(pg_stat_activity.procpid)
FROM pg_stat_activity
WHERE procpid <> pg_backend_pid() and current_query like 'SELECT%';--注意SELECT的大小写

 pg_cancel_backend
-------------------
 t
(1 rows)
```

或

```
SELECT pg_terminate_backend(pg_stat_activity.procpid)
FROM pg_stat_activity
WHERE procpid <> pg_backend_pid() and current_query like 'SELECT%'; --注意SELECT的大小写

 pg_terminate_backend
-------------------
 t
(1 rows)
```

**说明：** `cancel` 和`terminate`仅适用于当前用户的查询或权限小于等于当前用户的查询。若提示`"ERROR: must be superuser or rds_superuser to signal other server processes"`，则说明后台有其他用户的连接，但不影响`cancel` 和`terminate`的执行。

