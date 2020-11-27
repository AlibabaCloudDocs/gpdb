# Trajecotry常见问题

本文为您介绍Trajecotry常见问题和解答，有助于您使用时空数据库。

## 如何把坐标点数据转换为轨迹对象？

采用ST\_MakeTrajectory 构造函数可以将其转换为轨迹对象，设置方法如下：

```
-- 创建扩展
create extension ganos_trajectory cascade;

-- 创建点表
create table points(id integer, x float8, y float8, t timestamp, speed float8);
insert into points values(1, 128.1, 28.1, '2019-01-01 00:00:00', 100);
insert into points values(2, 128.2, 28.2, '2019-01-01 00:00:01', 101);
insert into points values(3, 128.3, 28.3, '2019-01-01 00:00:02', 102);
insert into points values(4, 128.4, 28.4, '2019-01-01 00:00:04', 103);

-- 创建轨迹表
create table traj(id integer, traj trajectory);

-- 插入数据
insert into traj(id, traj) 
select 1, 
    ST_MakeTrajectory('STPOINT'::leaftype, x, y, 4326, t, ARRAY['speed'], NULL, s, NULL) 
FROM (select array_agg(x order by id) as x,  
      array_agg(y order by id) as y,
      array_agg(t order by id) as t,
      array_agg(speed order by id) as s
  From points) a;
```

## 系统自带的ST\_MakTrajectory构造函数不满足需求怎么办？

如果系统自带的ST\_MakeTrajectory函数参数不符合应用需求，您可以进行自定义扩展。例如轨迹对象包含2个int8、2个float4和1个timestamp类型的属性，可以创建如下构造函数：

```
CREATE OR REPLACE FUNCTION  ST_MakeTrajectory(type leaftype, x float8[], y float8[] , 
               srid integer, timespan timestamp[],attrs_name cstring[], attr1 int8[], 
               attr2 int8[], attr3 float4[], attr4 float4[], attr5 timestamp[])
    RETURNS trajectory
    AS '$libdir/libpg-trajectory16','sqltr_traj_make_all_array'
    LANGUAGE 'c' IMMUTABLE Parallel SAFE;
```

其中前6个参数为固定类型的参数，后5个参数为用户自定义的轨迹属性类型。使用时直接使用用户定义的重载函数即可。

## 如何向轨迹中追加轨迹点？

使用ST\_append函数向现有轨迹追加轨迹点，ST\_append函数声明如下：

```
trajectory ST_append(trajectory traj, geometry spatial, timestamp[] timespan, text str_theme_json) ;

trajectory ST_append(trajectory traj, trajectroy tail) ;
```

例如，基于点表向轨迹中追加轨迹点方法如下：

```
-- 创建扩展
create extension ganos_trajectory cascade;

-- 创建点表
create table points(id integer, x float8, y float8, t timestamp, speed float8);
insert into points values(1, 128.1, 28.1, '2019-01-01 00:00:00', 100);
insert into points values(2, 128.2, 28.2, '2019-01-01 00:00:01', 101);
insert into points values(3, 128.3, 28.3, '2019-01-01 00:00:02', 102);
insert into points values(4, 128.4, 28.4, '2019-01-01 00:00:04', 103);

-- 创建轨迹表
create table traj(id integer, traj trajectory);

-- 插入数据
insert into traj(id, traj) 
select 1, 
    ST_MakeTrajectory('STPOINT'::leaftype, x, y, 4326, t, ARRAY['speed'], NULL, s, NULL) 
FROM (select array_agg(x order by id) as x,  
      array_agg(y order by id) as y,
      array_agg(t order by id) as t,
      array_agg(speed order by id) as s
  From points) a;

-- 插入新轨迹点
insert into points values(5, 128.5, 28.5, '2019-01-01 00:00:05', 105);
insert into points values(6, 128.6, 28.6, '2019-01-01 00:00:06', 106);
insert into points values(7, 128.7, 28.7, '2019-01-01 00:00:07', 107);

-- 基于单点更新轨迹
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

-- 如果使用程序生成SQL也可以写成
With point_traj as (
 select ST_MakeTrajectory('STPOINT'::leaftype, ARRAY[128.5::float8], 
      ARRAY[28.5::float8], 4326, ARRAY['2019-01-01 00:00:05'::timestamp], 
      ARRAY['speed'], NULL, ARRAY[106::float8], NULL) AS traj
)
Update traj 
set traj = ST_append(traj.traj, a.traj)
From point_traj a
WHERE traj.ID =1;

-- 基于多点更新轨迹
With point_traj as (
 select ST_MakeTrajectory('STPOINT'::leaftype, x, y, 4326, t, ARRAY['speed'], NULL, s, NULL) AS traj
 FROM (select array_agg(x order by id) as x,  
      array_agg(y order by id) as y,
      array_agg(t order by id) as t,
      array_agg(speed order by id) as s
      From points WHERE ID > 5 ) a 
)
Update traj 
set traj = ST_append(traj.traj, a.traj)
From point_traj a
WHERE traj.ID =1;
```

## 如何启用优化压缩功能？

lz4压缩算法是一种优秀的压缩算法，具有更高的压缩率和更快的执行速度。如果要启用lz4压缩算法，设置方法如下：

```
-- 设置使用lz4 压缩
Set toast_compression_use_lz4 = true; 

-- 使用pg默认压缩算法
Set toast_compression_use_lz4 = false;
```

如果要对整个数据默认采用lz4压缩算法，设置方法如下：

```
-- 数据库使用lz4压缩
Alter database dbname Set toast_compression_use_lz4 = true;

-- 数据库使用默认压缩
Alter database dbname Set toast_compression_use_lz4 = false;
```

## 如何设置字符串类型属性默认长度？

guc变量ganos.trajectory.attr\_string\_length用于设置字符串类型属性。设置方法如下：

```
Set ganos.trajectory.attr_string_length = 32;
```

## 如何计算某个属性的最大值（最小值/平均值）？

计算某个属性的最大值（最小值/平均值），方法如下：

```
With traj AS (
  select '{"trajectory":{"version":1,"type":"STPOINT","leafcount":2,"start_time":"2010-01-01 11:30:00","end_time":"2010-01-01 12:30:00","spatial":"SRID=4326;LINESTRING(1 1,3 5)","timeline":["2010-01-01 11:30:00","2010-01-01 12:30:00"],"attributes":{"leafcount":2,"velocity":{"type":"integer","length":4,"nullable":true,"value":[1,100]},"speed":{"type":"float","length":8,"nullable":true,"value":[null,1.0]},"angel":{"type":"string","length":64,"nullable":true,"value":["test",null]}, "tngel2":{"type":"timestamp","length":8,"nullable":true,"value":["2010-01-01 12:30:00",null]},"bearing":{"type":"bool","length":1,"nullable":true,"value":[null,true]}}}}'::trajectory a)
select avg(v) from 
(
Select unnest(st_trajAttrsAsInteger(a, 'velocity')) as v from traj
) t;
```

