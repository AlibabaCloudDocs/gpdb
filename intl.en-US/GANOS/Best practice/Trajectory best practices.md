# Trajectory best practices

## Use appropriate spatio-temporal indexes

You can create appropriate indexes based on application requirements to accelerate queries. Trajectory data supports the following index types:

-   Spatial index: the index on space, which applies when you query only the spatial data of a trajectory.
-   Temporal index: the index on time, which applies when you query only the time range of a trajectory.
-   Spatio-temporal composite index: the composite index on both space and time, which applies when you query both the spatial data and time range of a trajectory.

```
-- Create a function-based spatial index to accelerate the filtering of spatial data.
create index tr_spatial_geometry_index on trajtab using gist (st_trajectoryspatial(traj));


-- Create a function-based temporal index to accelerate the filtering of time.
create index tr_timespan_time_index on trajtab using gist (st_timespan(traj));

-- Create function-based indexes on the start time and end time of a trajectory object.
create index tr_starttime_index on trajtab using btree (st_starttime(traj));
create index tr_endtime_index on trajtab using btree (st_endtime(traj));


-- Create a btree_gist extension.
create extension btree_gist;

-- Use btree_gist to create a spatio-temporal composite index on the start time, end time, and spatial data of a trajectory object.
create index tr_traj_test_stm_etm_sp_index on traj_test using gist (st_starttime(traj),st_endtime(traj),st_trajectoryspatial(traj));
```

## Use appropriate partitioned tables

The amount of trajectory data in a database keeps increasing along with the continuous use of the database. As a result, a larger number of database indexes are created and data queries slow down. In this case, you can use partitioned tables to decrease the data size of a single table.

For more information about table partitioning, visit [Table partitioning](https://www.postgresql.org/docs/current/ddl-partitioning.html) in the PostgreSQL documentation.

## Reduce the use of string-type attribute fields

A large number of string-type attribute fields in trajectory data lead to a waste of the storage space and performance deterioration.

-   If string-type attribute fields have fixed values, they can be converted into integers. We recommend that you convert the data type in code.
-   If string-type attribute fields are required, you can set a default length for the fields to save space.

    To set a default length for string-type attribute fields, execute the following statement:

    ```
    -- Set the default length of string-type attribute fields to 32.
    Set ganos.trajectory.attr_string_length = 32;
    ```


## Use multiple trajectory points to generate a trajectory object

We recommend that you use multiple trajectory points to generate a trajectory object and avoid appending trajectory points one by one.

## Use the advanced compression mode

LZ4 is an advanced compression algorithm, with a higher compression ratio and execution speed. To enable and disable the LZ4 compression algorithm, execute the following statements:

```
-- Enable LZ4 compression.
Set toast_compression_use_lz4 = true; 

-- Disable LZ4 compression and use the default PostgreSQL compression algorithm.
Set toast_compression_use_lz4 = false;
```

To enable and disable the LZ4 compression algorithm for the entire database by default, execute the following statements:

```
-- Enable LZ4 compression for the database.
Alter database dbname Set toast_compression_use_lz4 = true;

-- Disable LZ4 compression and use the default compression algorithm for the database.
Alter database dbname Set toast_compression_use_lz4 = false;
```

