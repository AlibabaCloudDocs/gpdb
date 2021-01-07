# Trajectory FAQ

## How do I convert the data of coordinate points into a trajectory object?

You can use the ST\_makeTrajectory constructor to convert the data of coordinate points into a trajectory object. The procedure is as follows:

```
-- Create an extension.
create extension ganos_trajectory cascade;

-- Create a point table.
create table points(id integer, x float8, y float8, t timestamp, speed float8);
insert into points values(1, 128.1, 28.1, '2019-01-01 00:00:00', 100);
insert into points values(2, 128.2, 28.2, '2019-01-01 00:00:01', 101);
insert into points values(3, 128.3, 28.3, '2019-01-01 00:00:02', 102);
insert into points values(4, 128.4, 28.4, '2019-01-01 00:00:04', 103);

-- Create a trajectory table.
create table traj(id integer, traj trajectory);

-- Insert data.
insert into traj(id, traj) 
select 1, 
    ST_MakeTrajectory('STPOINT'::leaftype, x, y, 4326, t, ARRAY['speed'], NULL, s, NULL) 
FROM (select array_agg(x order by id) as x,  
      array_agg(y order by id) as y,
      array_agg(t order by id) as t,
      array_agg(speed order by id) as s
  From points) a;
```

## What shall I do if the built-in ST\_makeTrajectory constructor does not meet my requirements?

If the built-in ST\_makeTrajectory constructor does not meet your requirements, you can customize one with required attributes. For example, to create a trajectory object with five attributes, including two of the int8 type, two of the float4 type, and one of the timestamp type, you can customize the following constructor:

```
CREATE OR REPLACE FUNCTION ST_MakeTrajectory(type leaftype, x float8[], y float8[],  
               srid integer, timespan timestamp[],attrs_name cstring[], attr1 int8[], 
               attr2 int8[], attr3 float4[], attr4 float4[], attr5 timestamp[])
    RETURNS trajectory
    AS '$libdir/libpg-trajectory16','sqltr_traj_make_all_array'
    LANGUAGE 'c' IMMUTABLE Parallel SAFE;
```

In this constructor, the first six parameters are fixed, and the last five parameters are customized attributes. You can directly use this constructor with the customized attributes as a user-defined overloaded function.

## How do I append trajectory points to a trajectory object?

You can use the ST\_append constructor to append trajectory points to an existing trajectory object. The syntax of the ST\_append constructor is as follows:

```
trajectory ST_append(trajectory traj, geometry spatial, timestamp[] timespan, text str_theme_json);

trajectory ST_append(trajectory traj, trajectory tail);
```

The following example shows how to append trajectory points to a trajectory object based on a point table.

```
-- Create an extension.
create extension ganos_trajectory cascade;

-- Create a point table.
create table points(id integer, x float8, y float8, t timestamp, speed float8);
insert into points values(1, 128.1, 28.1, '2019-01-01 00:00:00', 100);
insert into points values(2, 128.2, 28.2, '2019-01-01 00:00:01', 101);
insert into points values(3, 128.3, 28.3, '2019-01-01 00:00:02', 102);
insert into points values(4, 128.4, 28.4, '2019-01-01 00:00:04', 103);

-- Create a trajectory table.
create table traj(id integer, traj trajectory);

-- Insert data.
insert into traj(id, traj) 
select 1, 
    ST_MakeTrajectory('STPOINT'::leaftype, x, y, 4326, t, ARRAY['speed'], NULL, s, NULL) 
FROM (select array_agg(x order by id) as x,  
      array_agg(y order by id) as y,
      array_agg(t order by id) as t,
      array_agg(speed order by id) as s
  From points) a;
  
-- Insert trajectory points.
insert into points values(5, 128.5, 28.5, '2019-01-01 00:00:05', 105);
insert into points values(6, 128.6, 28.6, '2019-01-01 00:00:06', 106);
insert into points values(7, 128.7, 28.7, '2019-01-01 00:00:07', 107);

-- Update the trajectory object based on a single trajectory point.
With point_traj as (
 select ST_MakeTrajectory('STPOINT'::leaftype, x, y, 4326, t, ARRAY['speed'], NULL, s, NULL) AS traj
 FROM (select array_agg(x order by id) as x,  
      array_agg(y order by id) as y,
      array_agg(t order by id) as t,
      array_agg(speed order by id) as s
      From points WHERE ID = 5) a 
)
Update traj 
set traj = ST_append(traj.traj, a.traj)
From point_traj a
WHERE traj.ID=1;

-- Use a generated SQL script to update the trajectory object.
With point_traj as (
 select ST_MakeTrajectory('STPOINT'::leaftype, ARRAY[128.5::float8], 
      ARRAY[28.5::float8], 4326, ARRAY['2019-01-01 00:00:05'::timestamp], 
      ARRAY['speed'], NULL, ARRAY[106::float8], NULL) AS traj
)
Update traj 
set traj = ST_append(traj.traj, a.traj)
From point_traj a
WHERE traj.ID =1;

-- Update the trajectory object based on multiple trajectory points.
With point_traj as (
 select ST_MakeTrajectory('STPOINT'::leaftype, x, y, 4326, t, ARRAY['speed'], NULL, s, NULL) AS traj
 FROM (select array_agg(x order by id) as x,  
      array_agg(y order by id) as y,
      array_agg(t order by id) as t,
      array_agg(speed order by id) as s
      From points WHERE ID > 5) a 
)
Update traj 
set traj = ST_append(traj.traj, a.traj)
From point_traj a
WHERE traj.ID =1;
```

## How do I enable advanced compression?

LZ4 is an advanced compression algorithm, with a higher compression ratio and execution speed. To enable the LZ4 compression algorithm, do as follows:

```
-- Enable LZ4 compression.
Set toast_compression_use_lz4 = true; 

-- Disable LZ4 compression to use the default PostgreSQL compression algorithm.
Set toast_compression_use_lz4 = false;
```

To enable the LZ4 compression algorithm for the entire database by default, do as follows:

```
-- Enable LZ4 compression for the database.
Alter database dbname Set toast_compression_use_lz4 = true;

-- Disable LZ4 compression to use the default compression algorithm for the database.
Alter database dbname Set toast_compression_use_lz4 = false;
```

## How do I set a default length for string-type attribute fields?

You can use the GUC variable ganos.trajectory.attr\_string\_length to set a default length for string-type attribute fields. You can do as follows:

```
Set ganos.trajectory.attr_string_length = 32;
```

## How do I compute the maximum, minimum, or average value of an attribute field?

To compute the maximum, minimum, or average value of an attribute field, do as follows:

```
With traj AS (
  select '{"trajectory":{"version":1,"type":"STPOINT","leafcount":2,"start_time":"2010-01-01 11:30:00","end_time":"2010-01-01 12:30:00","spatial":"SRID=4326;LINESTRING(1 1,3 5)","timeline":["2010-01-01 11:30:00","2010-01-01 12:30:00"],"attributes":{"leafcount":2,"velocity":{"type":"integer","length":4,"nullable":true,"value":[1,100]},"speed":{"type":"float","length":8,"nullable":true,"value":[null,1.0]},"angle":{"type":"string","length":64,"nullable":true,"value":["test",null]}, "tngel2":{"type":"timestamp","length":8,"nullable":true,"value":["2010-01-01 12:30:00",null]},"bearing":{"type":"bool","length":1,"nullable":true,"value":[null,true]}}}}'::trajectory a)
select avg(v) from 
(
Select unnest(st_trajAttrsAsInteger(a, 'velocity')) as v from traj
) t;
```

