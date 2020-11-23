# Schema管理

Schema是数据库的命名空间，它是一个数据库内部的对象（表、索引、视图、存储过程、操作符）的集合。Schema在每个数据库中是唯一的。每个数据库都有一个名为public的默认Schema。

如果用户没有创建任何Schema，对象会被创建在public schema中。所有的该数据库角色（用户）都在默认的public schema中拥有CREATE和USAGE特权。

## 创建Schema

使用`CREATE SCHEMA`命令来创建一个新的Schema，命令如下：

```
CREATE SCHEMA <schema_name> [AUTHORIZATION <username>]
```

**说明：**

-   <schema\_name\>：schema名称。
-   <username\>：如指定authorization username，则创建的schema属于该用户。否则，属于执行该命令的用户。

**示例：**

```
CREATE SCHEMA myschema;
```

## 设置Schema搜索路径

数据库的search\_path用于配置参数设置Schema的搜索顺序。

使用`ALTER DATABASE`命令可以设置搜索路径。例如：

```
ALTER DATABASE mydatabase SET search_path TO myschema, public, pg_catalog;
```

您也可以使用`ALTERROLE`命令为特定的角色（用户）设置search\_path。例如：

```
ALTER ROLE sally SET search_path TO myschema, public, pg_catalog;
```

## 查看当前Schema

使用`current_schema()`函数可以查看当前的Schema。例如：

```
SELECT current_schema();
```

您也可以使用`SHOW`命令查看当前的搜索路径。例如：

```
SHOW search_path;
```

## 删除Schema

使用`DROP SCHEMA`命令删除一个空的Schema。例如：

```
DROP SCHEMA myschema;
```

**说明：** 默认情况下，Schema它必须为空才可以删除。

删除一个Schema连同其中的所有对象（表、数据、函数等等），可以使用：

```
DROP SCHEMA myschema CASCADE;
```

## 更多信息

详情请参考[Pivotal Greenplum 官方文档](http://gpdb.docs.pivotal.io/43330/ref_guide/sql_commands/CREATE_SCHEMA.html)。

