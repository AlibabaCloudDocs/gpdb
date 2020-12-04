# PL/Python 使用

云原生数据仓库AnalyticDB PostgreSQL支持用户使用 PL/Python 过程语言自定义函数。

## 限制

-   不支持在 PL/Python 中使用触发器。
-   不支持可更新的游标（比如 `UPDATE...WHERE CURRENT OF` and `DELETE...WHERE CURRENT OF`）。
-   只支持python2，暂不支持python3。

## 创建或删除 PL/Python 插件

在 AnalyticDB for PostgreSQL 中，执行如下命令，创建或删除 PL/Python 插件（每个数据库只需执行一次）。因为 PL/Python 是非授信语言，只支持使用数据库的高权限（初始）账号，执行该命令。以下示例中，高权限账号为admin，需要创建 PL/Python 所在的数据库名为 testdb :

创建 PL/Python 插件

```
$ psql -U admin -d testdb -c 'CREATE EXTENSION plpythonu;'
```

删除 PL/Python 插件

```
$ psql -U admin -d testdb -c 'DROP EXTENSION plpythonu;'
```

## 使用 PL/Python 开发 Python 函数

**说明：** 由于安全原因 PL/Python 没有直接开放，创建Python函数请[提交工单](https://workorder.console.aliyun.com/console.htm#/ticket/add?productCode=gpdb)，经ADB PG后台技术人员确认无安全风险后，由后台创建。

示例：创建 Python 函数

```
CREATE FUNCTION return_int_array()
  RETURNS int[]
AS $$
  return [1, 2, 3, 4]
$$ LANGUAGE plpythonu;
```

使用函数

```
SELECT return_int_array();
return_int_array
---------------------
{1,11,21,31}
(1 row) 
```

## 参考文档

关于 Python 的更多用法，请参见[Python官网](https://www.python.org/)。

关于 PL/Python 的更多用法，请参见[PostgreSQL文档](https://www.postgresql.org/docs/9.4/plpython.html)。

