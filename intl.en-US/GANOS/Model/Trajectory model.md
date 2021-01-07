# Trajectory model

## Overview

Trajectory data records the continuous location update information about a moving feature, such as a vehicle or a person. Trajectory data is a type of typical spatio-temporal data. You can use trajectory data for in-depth analysis.

Ganos Trajectory is an extension of AnalyticDB for PostgreSQL. Ganos Trajectory provides a series of data types, functions, and stored procedures. This allows you to manage, query, and analyze spatio-temporal trajectory data.

## Quick start

-   Create extensions.

    ```
    Create extension ganos_spatialref;
    Create extension ganos_geometry;
    Create Extension Ganos_trajectory;
    ```

-   Create the enumeration type for a trajectory.

    ```
    CREATE TYPE leaftype AS ENUM ('STPOINT', 'STPOLYGON');
    ```

-   Create a trajectory table.

    ```
    Create Table traj_table (id integer, traj trajectory) DISTRIBUTED BY (id);
    ```

-   Insert trajectory data records into the trajectory table.

    ```
    insert into traj_table values
    (1, ST_MakeTrajectory('STPOINT'::leaftype, st_geomfromtext('LINESTRING (114 35, 115 36, 116 37)', 4326), '[2010-01-01 14:30, 2010-01-01 15:30)'::tsrange, '{"leafcount": 3,"attributes" : {"velocity" : {"type":"integer","length":4,"nullable":false,"value":[120, 130, 140]},"accuracy":{"type":"integer","length":4,"nullable":false,"value":[120, 130, 140]},"bearing":{"type":"float","length":4,"nullable":false,"value":[120, 130, 140]},"acceleration":{"type":"float","length":4,"nullable":false,"value":[120, 130, 140]}}}')),
    (2, ST_MakeTrajectory('STPOINT'::leaftype, st_geomfromtext('LINESTRING (114 35, 115 36, 116 37)', 4326), '2010-01-01 14:30'::timestamp, '2010-01-01 15:30'::timestamp, '{"leafcount": 3,"attributes" : {"velocity" : {"type":"integer","length":4,"nullable":false,"value":[120, 130, 140]},"accuracy":{"type":"integer","length":4,"nullable":false,"value":[120, 130, 140]},"bearing":{"type":"float","length":4,"nullable":false,"value":[120, 130, 140]},"acceleration":{"type":"float","length":4,"nullable":false,"value":[120, 130, 140]}}}')),
    (3, ST_MakeTrajectory('STPOINT'::leaftype, st_geomfromtext('LINESTRING (114 35, 115 36, 116 37)', 4326),ARRAY['2010-01-01 14:30'::timestamp, '2010-01-01 15:00'::timestamp, '2010-01-01 15:30'::timestamp], '{"leafcount": 3,"attributes" : {"velocity" : {"type":"integer","length":4,"nullable":false,"value":[120, 130, 140]},"accuracy":{"type":"integer","length":4,"nullable":false,"value":[120, 130, 140]},"bearing":{"type":"float","length":4,"nullable":false,"value":[120, 130, 140]},"acceleration":{"type":"float","length":4,"nullable":false,"value":[120, 130, 140]}}}')),
    (4, ST_MakeTrajectory('STPOINT'::leaftype, st_geomfromtext('LINESTRING (114 35, 115 36, 116 37)', 4326), '[2010-01-01 14:30, 2010-01-01 15:30)'::tsrange, null));
    ```

-   Create a trajectory index on the trajectory table.

    ```
    --Create a trajectory index to accelerate the process of filtering spatio-temporal data records.
    create index tr_index on traj_table using gist (traj);
    
    --Run spatial data queries. You can find that the trajectory index accelerates the process of filtering spatial data records.
    select id, traj_id from traj_test where st_3dintersects(traj, ST_GeomFromText('POLYGON((116.46747851805917 39.92317964155052,116.4986540687358 39.92317964155052,116.4986540687358 39.94452401711516,116.46747851805917 39.94452401711516,116.46747851805917 39.92317964155052))'));
    
    --Run temporal data queries. You can find that the trajectory index accelerates the process of filtering temporal data records.
    select id, traj_id from traj_text where st_TContains(traj,'2008-02-02 13:30:44'::timestamp,'2008-02-03 17:30:44'::timestamp);
    
    --Run spatio-temporal data queries. You can find that the trajectory index accelerates the process of filtering spatio-temporal data records.
    select id, traj_id from traj_test where st_3dintersects(traj, ST_GeomFromText('POLYGON((116.46747851805917 39.92317964155052,116.4986540687358 39.92317964155052,116.4986540687358 39.94452401711516,116.46747851805917 39.94452401711516,116.46747851805917 39.92317964155052))'),'2008-02-02 13:30:44'::timestamp,'2008-02-03 17:30:44'::timestamp);
    ```

-   Create a trajectory index in a specific dimension.

    ```
    --If you want to analyze trajectory data only from a specific dimension, create a trajectory index in that dimension. For example, if you do not want to analyze trajectory data in the z dimension, create a two-dimensional temporal trajectory index by using trajgist_op_2dt.
    create index tr_timespan_time_index on traj_table using gist (traj trajgist_op_2dt);
    
    --Query two-dimensional temporal data. The queries are run at a fast rate.
    select id, traj_id from traj_test where st_2dintersects(traj, ST_GeomFromText('POLYGON((116.46747851805917 39.92317964155052,116.4986540687358 39.92317964155052,116.4986540687358 39.94452401711516,116.46747851805917 39.94452401711516,116.46747851805917 39.92317964155052))'),'2008-02-02 13:30:44'::timestamp,'2008-02-03 17:30:44'::timestamp);
    
    --Create more trajectory indexes based on your business requirements. If you create one trajectory index, all queries run based on the trajectory index. If you create multiple trajectory indexes, AnalyticDB for PostgreSQL selects the optimal trajectory index when you run queries.
    create index tr_timespan_time_index on trajtab using gist (traj trajgist_op_2d);
    select id, traj_id from traj_test where st_2dintersects(traj, ST_GeomFromText('POLYGON((116.46747851805917 39.92317964155052,116.4986540687358 39.92317964155052,116.4986540687358 39.94452401711516,116.46747851805917 39.94452401711516,116.46747851805917 39.92317964155052))'));
    ```

-   Query the start time and end time of trajectories.

    ```
    select st_startTime(traj), st_endTime(traj) from traj_table ;
        st_starttime     |     st_endtime      
    ---------------------+---------------------
     2010-01-01 14:30:00 | 2010-01-01 15:30:00
     2010-01-01 14:30:00 | 2010-01-01 15:30:00
     2010-01-01 14:30:00 | 2010-01-01 15:30:00
     2010-01-01 14:30:00 | 2010-01-01 15:30:00
     2010-01-01 14:30:00 | 2010-01-01 15:30:00
     2010-01-01 11:30:00 | 2010-01-01 15:00:00
     2010-01-01 11:30:00 | 2010-01-01 15:00:00
     2010-01-01 11:30:00 | 2010-01-01 15:00:00
    (8 rows)
    ```

-   Query the trajectories of moving objects.

    ```
    --Use an interpolation function to query the attributes of trajectory points.
    Select ST_velocityAtTime(traj, '2010-01-01 12:45') from traj_table  where id > 5; 
    st_velocityattime 
    -------------------                 
    5                 
    5  
    4.16666666666667
    (3 rows)
    ```

-   Analyze the proximity among trajectories.

    ```
    Select ST_euclideanDistance((Select traj From traj_table Where id = 6), 
                                (Select traj From traj_table Where id = 7));
     st_euclideandistance 
    ----------------------
       0.0334968923954815
    (1 row)
    ```

-   Delete the extensions.

    ```
    DROP Extension Ganos_Raster;
    DROP extension ganos_geometry;
    Drop Extension Ganos_trajectory;
    ```


## SQL reference

For more information, see [Trajectory SQL reference](/intl.en-US/Spatio-temporal Database/Trajectory SQL reference/Basic concepts.md).

