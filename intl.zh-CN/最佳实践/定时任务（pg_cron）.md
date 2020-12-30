# 定时任务（pg\_cron）

本文介绍如何通过PostgreSQL提供的pg\_cron插件设置定时任务。

pg\_cron是基于cron的作业调度插件，语法与常规cron相同，但它可以直接从数据库执行PostgreSQL命令，当前版本为1.2。

每一个定时任务分为两部分：

1.  定时任务

    用户具体的任务内容，例如`VACUUM`。

2.  定时计划

    -   规定使用插件的计划，例如每隔1分钟执行一次该任务。
    -   定时计划使用标准的cron语法，其中：

        -   **\***表示**任意时间都运行**。
        -   特定数字表示**仅在这个时间时运行**。
        -   **,**表示**多个时间散列值**。
        -   **-**表示**时间范围**，**/**表示**范围间隔**。
        ```
         ┌───────────── 分钟： 0 ~ 59
         │ ┌────────────── 小时： 0 ~ 23
         │ │ ┌─────────────── 日期： 1 ~ 31
         │ │ │ ┌──────────────── 月份： 1 ~ 12
         │ │ │ │ ┌───────────────── 一周中的某一天 ：0 ~ 6，0表示周日。
         │ │ │ │ │                  
         │ │ │ │ │
         │ │ │ │ │
         * * * * *
        ```

        **说明：**

        -   定时任务执行的时间是GMT时间，请注意将本地时间换算成GMT时间。
        -   可以借助网站 [crontab.guru](http://crontab.guru/)创建、预览定时计划。
    语法示例如下：

    -   每周六3:30 AM（GMT）：

        ```
        30 3 * * 6
        ```

    -   每月1号和30号1:45 AM（GMT）：

        ```
        45 1 1,30 * *
        ```

    -   每周一至周五的3:00 AM （GMT）：

        ```
        00 3 * * 1-5
        ```

    -   从8点（GMT）到20点（GMT），每两小时整点：

        ```
        0 8-20/2 * * *
        ```


## 使用方法

-   创建插件

    ```
    CREATE EXTENSION pg_cron WITH SCHEMA pg_catalog VERSION '1.0';
    ALTER EXTENSION pg_cron UPDATE;
    ```

    **说明：**

    -   请联系运维人员确认数据库配置中shared\_preload\_libraries是否包含pg\_cron，并告知运维人员配置cron.database\_name，配置完成后重启数据库生效。
    -   请使用管理员用户执行上述SQL命令。
    -   创建插件后进行升级，即可将插件版本从1.0升级至1.2。
-   删除插件

    ```
    DROP EXTENSION pg_cron;
    ```

    **说明：** 请使用管理员用户执行上述SQL命令。

-   添加定时任务

    ```
    SELECT cron.schedule('<定时计划>', '<定时任务>');
    ```

    示例如下：

    ```
    -- 周六3:30 AM (GMT) 删除过期数据
    SELECT cron.schedule('30 3 * * 6', $$DELETE FROM events WHERE event_time < now() - interval '1 week'$$);
    
    -- 每天的10:00 AM (GMT) 执行函数test
    SELECT cron.schedule('0 10 * * *', 'select test()');
    
    -- 每分钟执行指定SQL
    SELECT cron.schedule('* * * * *', 'select 1');
    
    -- 每月1号和30号以及每周六和周日的2:30 AM (GMT) 执行磁盘清理
    SELECT cron.schedule('30 2 1,30 * 6,0', 'VACUUM FULL');
    ```

-   删除定时任务

    ```
    SELECT cron.unschedule(<定时任务ID>);
    ```

    示例如下：

    ```
    -- 删除ID为21的定时任务
    SELECT cron.unschedule(21);
    ```

    **说明：** 定时任务ID为创建任务时自动创建，可以通过查看cron.job表的jobid字段获取。

-   查看定时任务列表

    ```
    SELECT * FROM cron.job;
    ```


