# 使用INSERT ON CONFLICT覆盖写入数据

本文介绍在云原生数据仓库AnalyticDB PostgreSQL版数据库中，如何使用覆盖写入数据。

INSERT ON CONFLICT语法可以被用于写入时有主键冲突下，对冲突主键行进行覆盖写入，将针对该主键的INSERT行为转换为UPDATE行为。该特性又称UPSERT覆盖写，可以实现与MySQL的REPLACE INTO类似功能。

该特性为AnalyticDB PostgreSQL 6.0版本新功能，在4.3版本中不支持。

## SQL语法

覆盖写入语法基于INSERT语句，INSERT语句的语法大纲如下：

```
[ WITH [ RECURSIVE ] with_query [, ...] ]
INSERT INTO table_name [ AS alias ] [ ( column_name [, ...] ) ]
    { DEFAULT VALUES | VALUES ( { expression | DEFAULT } [, ...] ) [, ...] | query }
    [ ON CONFLICT [ conflict_target ] conflict_action ]
    [ RETURNING * | output_expression [ [ AS ] output_name ] [, ...] ]

其中，conflict_target为:

    ( { index_column_name | ( index_expression ) } [ COLLATE collation ] [ opclass ] [, ...] )

其中，conflict_action为:

    DO NOTHING
    DO UPDATE SET { column_name = { expression | DEFAULT } |
                    ( column_name [, ...] ) = ( { expression | DEFAULT } [, ...] )
                  } [, ...]
              [ WHERE condition ]
			
```

相对于普通INSERT语法，覆盖写主要增加了ON CONFLICT子句，该子句分为两部分，分别是：

-   conflict\_target，用于指定在哪些列上有冲突。conflict\_target在conflict\_action为DO NOTHING时可省略，在conflict\_action为DO UPDATE时，需要指定一个列表，指定主键列的列表或Unique Index列的列表
-   conflict\_action，用于指定冲突后需要执行的动作。分为DO NOTHING和DO UPDATE两种。 （1）DO NOTHING表示如果有冲突，则丢弃待插入的数据。 （2）DO UPDATE表示如果有冲突，则按照后面的UPDATE语法进行数据覆盖。

在DO UPDATE SET子句中，可以使用EXCLUDED来表示冲突的数据构成的伪表，引用其中的列。比如表tbl有一主键列pri\_key，有一列非主键列col\_name，要在有冲突的情况下，使用插入的col\_name值覆盖掉原来的col\_name的值，则可以写成：

```
insert into tbl values (0, 1), (2, 3), (4, 5)
on conflict (pri_key) do update set tbl.col_name = excluded.col_name;
			
```

上述语句中，新插入的数据\(0, 1\), \(2, 3\), \(4, 5\)构成了一个伪表，3行2列，表名为EXCLUDED，可以使用excluded.col\_name去引用这个表中的列。

## 约束及需要注意的点

-   覆盖写入特性只在ADB PG 6.0版本中有效，ADB PG 4.3版本无此特性
-   目标表需为行存表，不支持列存表（列存不支持unique index，所以无法支持列存表）
-   目标表不支持分区表
-   不支持在UPDATE的SET子句中更新分布列和主键列
-   不支持在UPDATE的WHERE子句中使用子查询
-   目标表不支持Updatable View
-   不支持在同一条INSERT语句中对同一主键插入多条数据（国际SQL标准约束）

## SQL示例

-   基本功能示例

首先来看一个例子，创建一个表t1，包含4列，其中第1列（a）是它的主键（primary key）：

```
create table t1 (a int primary key, b int, c int, d int default 0);
			
```

对该表t1插入一行数据，主键列a的值为0：

```
insert into t1 values (0,0,0,0);

select * from t1;
 a | b | c | d
---+---+---+---
 0 | 0 | 0 | 0
(1 row)
			
```

如果再对该表t1插入一行数据，主键列a的值还是0，会得到一个报错：

```
insert into t1 values (0,1,1,1);

ERROR:  duplicate key value violates unique constraint "t1_pkey"
DETAIL:  Key (a)=(0) already exists.
			
```

但是，在很多场景中，不希望这个插入报错。有些场景希望主键冲突后什么都不做，有些场景希望主键冲突后更新非主键列的数据。那么在这些场景中就需要用到本文重点介绍的覆盖写入特性了。

主键冲突后，希望什么都不做（适用于有冲突丢弃冲突数据的场景）：

```
-- 使用on conflict do nothing子句

insert into t1 values (0,1,1,1) on conflict do nothing;

select * from t1;
 a | b | c | d
---+---+---+---
 0 | 0 | 0 | 0
(1 row)
			
```

主键冲突后，希望更新非主键列（适用于全部列覆盖写入的场景）：

```
-- 使用on conflict do update子句

insert into t1 values (0,2,2,2) on conflict (a) do update set (b, c, d) = (excluded.b, excluded.c, excluded.d);

select * from t1;
 a | b | c | d
---+---+---+---
 0 | 2 | 2 | 2
(1 row)
			
```

可以从上面的例子看出，在INSERT语句中加入ON CONFLICT DO NOTHING/UPDATE子句后，就可以轻松实现覆盖写入了。其中excluded为表示插入`values (0,2,2,2)`构成的伪表。

此外，上述语句等价为：

```
insert into t1 values (0,2,2,2) on conflict (a) do update set b = excluded.b, c = excluded.c, d = excluded.d;
			
```

当然，覆盖写入的功能还很强大，下面将具体介绍。

-   有冲突，覆盖部分列为待更新数据（适用于基于冲突数据覆盖部分列的场景）

```
-- 只覆盖c列的数据（excluded.c为冲突数据的c列）

insert into t1 values (0,0,3,0) on conflict (a) do update set c = excluded.c;

select * from t1;
 a | b | c | d
---+---+---+---
 0 | 2 | 3 | 2
(1 row)
			
```

-   有冲突，更新部分列的数据（适用于基于原始数据更新部分列场景）

```
-- 将原来的d列数据加1

insert into t1 values (0,0,3,0) on conflict (a) do update set d = t1.d + 1;

select * from t1;
 a | b | c | d
---+---+---+---
 0 | 2 | 3 | 3
(1 row)
			
```

-   有冲突，更新数据为默认值（适用于冲突后，回退数据到默认值的场景）

```
-- 将d列数据设置为其默认值（上文中d列的默认值为0）

insert into t1 values (0,0,3,0) on conflict (a) do update set d = default;

select * from t1;
 a | b | c | d
---+---+---+---
 0 | 2 | 3 | 0
(1 row)
			
```

-   插入多条数据

```
-- 插入2行数据，其中a=0的行有主键冲突（进行do nothing），a=1的行没有主键冲突（进行了插入）

insert into t1 values (0,0,0,0), (1,1,1,1) on conflict do nothing;

select * from t1;
 a | b | c | d
---+---+---+---
 0 | 2 | 3 | 0
 1 | 1 | 1 | 1
(2 rows)

-- 插入2行数据，其中a=0的行有主键冲突（进行覆盖写入），a=2的行没有主键冲突（进行了插入）

insert into t1 values (0,0,0,0), (2,2,2,2) on conflict (a) do update set (b, c, d) = (excluded.b, excluded.c, excluded.d);

select * from t1;
 a | b | c | d
---+---+---+---
 0 | 0 | 0 | 0
 1 | 1 | 1 | 1
 2 | 2 | 2 | 2
(3 rows)
			
```

-   插入数据来自于子查询（用于合并两表数据或更复杂的INSERT INTO SELECT场景）

```
create table t2 (like t1);
insert into t2 values (2,22,22,22),(3,33,33,33);

-- 将t2的数据插入t1，如果有冲突，则覆盖写入

insert into t1 select * from t2 on conflict (a) do update set (b, c, d) = (excluded.b, excluded.c, excluded.d);

select * from t1;
 a | b  | c  | d
---+----+----+----
 0 |  0 |  0 |  0
 1 |  1 |  1 |  1
 2 | 22 | 22 | 22
 3 | 33 | 33 | 33
(4 rows)
			
```

