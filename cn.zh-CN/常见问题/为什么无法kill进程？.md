# 为什么无法kill进程？ {#concept_gry_h5d_xgb .concept}

在某些情况下，使用`kill PID;`命令杀死进程，命令不起作用。此时，可以使用`select pg_terminate_backend(PID);`命令终止进程。其中PID可以使用`select * from pg_stat_activity;`命令进行查询。但是，有些处于回滚状态的进程是无法kill的，此时只能等进程执行完成或者重启数据库来解决。

