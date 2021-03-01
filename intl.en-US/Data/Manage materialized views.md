Manage materialized views 
==============================================

Materialized views are similar to views and allow you to save frequently used or complex queries. Materialized views are different from views in that materialized views are based on physical storage. You cannot write data to materialized views. When a query accesses a materialized view, the system returns the data that is stored in the materialized view. The data in materialized views is not automatically updated and may become obsolete. However, you can retrieve data stored in the materialized views faster than retrieving the same data from underlying tables or using views to retrieve the same data from underlying tables. Therefore, if you accept periodic data updates, materialized views have significant performance advantages.

Create a materialized view 
-----------------------------------------------

Run the `CREATE MATERIALIZED VIEW` command to create a materialized view of a query.



    CREATE MATERIALIZED VIEW my_materialized_view as 
        SELECT * FROM people WHERE age > 40
        DISTRIBUTED BY (id);
    
    SELECT * from my_materialized_view ORDER BY age;
         id     |    name    |    city    | age
    ------------+------------+------------+-----
     004        | zhaoyi     | zhenzhou   |  44
     005        | xuliui     | jiaxing    |  54
     006        | maodi      | shanghai   |  55
    (3 rows)



The query defined in a materialized view is executed and used to populate the view at the time the command is issued. A materialized view has many of the same properties as a table, except that object identifiers (OIDs) are not automatically generated. The DISTRIBUTED BY clause is optional when you create a materialized view. If the DISTRIBUTED BY clause is not specified, the first column of the table is used as the distribution key.


**Note** If a materialized view query contains an **ORDER BY** or **SORT** clause, the data may not be ordered or sorted.

Refresh or disable a materialized view 
-----------------------------------------------------------

Run the `REFRESH MATERIALIZED VIEW` command to update data in a materialized view.



    INSERT INTO people VALUES('007','sunshen','shenzhen',60);
    
    SELECT * from my_materialized_view ORDER BY age;
         id     |    name    |    city    | age
    ------------+------------+------------+-----
     004        | zhaoyi     | zhenzhou   |  44
     005        | xuliui     | jiaxing    |  54
     006        | maodi      | shanghai   |  55
    (3 rows) 
    
    REFRESH MATERIALIZED VIEW my_materialized_view;
    
    SELECT * from my_materialized_view ORDER BY age;
         id     |    name    |    city    | age 
    ------------+------------+------------+-----
     004        | zhaoyi     | zhenzhou   |  44 
     005        | xuliui     | jiaxing    |  54
     006        | maodi      | shanghai   |  55
     007        | sunshen    | shenzhen   |  60  
    (4 rows)



If you include the `WITH NO DATA` clause in the REFRESH MATERIALIZED VIEW command, the materialized view is not populated with data and cannot be scanned after you run the command. If a query attempts to access a materialized view that cannot be scanned, an error is returned.




    REFRESH MATERIALIZED VIEW my_materialized_view With NO DATA;
    
    SELECT * from my_materialized_view ORDER BY age;
    ERROR:  materialized view "my_materialized_view" has not been populated
    HINT:  Use the REFRESH MATERIALIZED VIEW command.
    
    REFRESH MATERIALIZED VIEW my_materialized_view;
    
    SELECT * from my_materialized_view ORDER BY age;
         id     |    name    |    city    | age 
    ------------+------------+------------+-----
     004        | zhaoyi     | zhenzhou   |  44 
     005        | xuliui     | jiaxing    |  54
     006        | maodi      | shanghai   |  55
     007        | sunshen    | shenzhen   |  60  
    (4 rows)



Remove a materialized view 
-----------------------------------------------

Run the `DROP MATERIALIZED VIEW` command to remove a materialized view.



    CREATE MATERIALIZED VIEW depend_materialized_view as 
        SELECT * FROM my_materialized_view WHERE age > 50 
        DISTRIBUTED BY (id);
    
    DROP MATERIALIZED VIEW depend_materialized_view;



You can use `DROP MATERIALIZED VIEW ... CASCADE` to remove objects that depend on the materialized view. For example, if the materialized view that you want to remove has dependent views, the materialized views are also removed.



**Notice** You must specify the **CASCADE** option in the DROP MATERIALIZED VIEW command when you remove a materialized view that has dependent views. Otherwise, an error is returned.

    CREATE MATERIALIZED VIEW depend_materialized_view as 
        SELECT * FROM my_materialized_view WHERE age > 50 
        DISTRIBUTED BY (id);
    
    DROP MATERIALIZED VIEW my_materialized_view;
    ERROR:  cannot drop materialized view my_materialized_view because other objects depend on it
    DETAIL:  materialized view depend_materialized_view depends on materialized view my_materialized_view
    HINT:  Use DROP ... CASCADE to drop the dependent objects too.
    
    DROP MATERIALIZED VIEW my_materialized_view CASCADE;



Scenarios 
------------------------------

* Materialized views apply to queries that are not time-sensitive.

* Materialized views apply to frequently used or complex queries.

* To implement fast queries and analysis, you can create materialized views based on external data sources, such as the external tables of Object Storage Service (OSS) or MaxCompute. You can use the materialized views to store external data to on-premises storage. You can also create indexes for the materialized views.

  




References 
-------------------------------

For more information, see the [Pivotal Greenplum documentation](https://gpdb.docs.pivotal.io/6-3/ref_guide/sql_commands/CREATE_MATERIALIZED_VIEW.html).

