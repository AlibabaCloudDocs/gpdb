# Use the pg\_cron plug-in to configure scheduled tasks

This topic describes how to use the pg\_cron plug-in of AnalyticDB for PostgreSQL. This plug-in allows you to configure scheduled tasks.

pg\_cron is a cron-based job scheduling plug-in that uses the same syntax as standard cron expressions. pg\_cron allows you to schedule PostgreSQL commands directly from AnalyticDB for PostgreSQL databases. The latest version of pg\_cron is 1.2.

Each scheduled task consists of the following parts:

1.  Task

    The specific task to execute, such as `VACUUM`.

2.  Schedule

    -   The schedule based on which the pg\_cron plug-in is run. For example, the schedule specifies to run the pg\_cron plug-in once every minute.
    -   The schedule follows the same syntax as standard cron expressions. You can use the following operators in the syntax:

        -   An **asterisk \(\*\)** specifies to **run the pg\_cron plug-in at any time**.
        -   A specific number specifies to **run the pg\_cron plug-in only during the time period that is specified by this number**.
        -   **Commas \(,\)** can be used to separate **multiple hash values of time**.
        -   A **hyphen \(-\)** can be used to specify a **time range**. A **forward slash \(/\)** can be used to specify a **step value**.
        ```
         ┌───────────── Minute: 0 to 59
         │ ┌────────────── Hour: 0 to 23
         │ │ ┌─────────────── Date: 1 to 31
         │ │ │ ┌──────────────── Month: 1 to 12
         │ │ │ │ ┌───────────────── Day of the week: 0 to 6 (The value 0 indicates Sunday.)
         │ │ │ │ │                  
         │ │ │ │ │
         │ │ │ │ │
         * * * * *
        ```

        **Note:**

        -   pg\_cron uses Greenwich Mean Time \(GMT\). You must convert the local system time to GMT.
        -   For more information about how to create and preview schedules, visit [crontab.guru](http://crontab.guru/).
    Syntax examples:

    -   Every Saturday at 03:30:00 GMT:

        ```
        30 3 * * 6
        ```

    -   1st and 30th of every month at 01:45:00 GMT:

        ```
        45 1 1,30 * *
        ```

    -   Every week from Monday to Friday at 03:00:00 GMT:

        ```
        00 3 * * 1-5
        ```

    -   Every two hours from 08:00:00 GMT to 20:00:00 GMT:

        ```
        0 8-20/2 * * *
        ```


## Usage

-   Create the pg\_cron plug-in.

    ```
    CREATE EXTENSION pg_cron WITH SCHEMA pg_catalog VERSION '1.0';
    ALTER EXTENSION pg_cron UPDATE;
    ```

    **Note:**

    -   Contact O&M personnel to check whether pg\_cron is added to the list of libraries specified by shared\_preload\_libraries in database configurations. Ask O&M personnel to configure the cron.database\_name parameter. Then, restart the database to make the configurations take effect.
    -   Execute the preceding SQL statements as an administrator.
    -   If you upgrade the pg\_cron plug-in after you create it, the version of pg\_cron is upgraded from 1.0 to 1.2.
-   Delete the pg\_cron plug-in.

    ```
    DROP EXTENSION pg_cron;
    ```

    **Note:** Execute the preceding SQL statement as an administrator.

-   Create a scheduled task.

    ```
    SELECT cron.schedule('<Schedule>', '<Task>');
    ```

    Examples:

    ```
    -- Delete stale data at 03:30:00 GMT every Saturday.
    SELECT cron.schedule('30 3 * * 6', $$DELETE FROM events WHERE event_time < now() - interval '1 week'$$);
    
    -- Perform function test at 10:00:00 GMT every day.
    SELECT cron.schedule('0 10 * * *', 'select test()');
    
    -- Execute the specified SQL statement every minute.
    SELECT cron.schedule('* * * * *', 'select 1');
    
    -- Clear disks at 02:30:00 GMT every Saturday and Sunday and on the 1st and 30th of every month.
    SELECT cron.schedule('30 2 1,30 * 6,0', 'VACUUM FULL');
    ```

-   Delete a scheduled task.

    ```
    SELECT cron.unschedule(<The ID of the scheduled task>);
    ```

    Example:

    ```
    -- Delete a scheduled task whose ID is 21.
    SELECT cron.unschedule(21);
    ```

    **Note:** The ID of a scheduled task is automatically created when you create the scheduled task. You can obtain the ID of the scheduled task by querying the jobid field in the cron.job table.

-   View the list of scheduled tasks.

    ```
    SELECT * FROM cron.job;
    ```


