AnalyticDB PostgreSQL版错误码 
==============================================

AnalyticDB PostgreSQL版服务器发出的所有消息都赋予了五个字符的错误代码， 这些代码遵循SQL的 "SQLSTATE" 代码的习惯。本文中列出了PostgreSQL 8.1定义的所有错误代码。


|  错误码  |                                  含义                                  |
|-------|----------------------------------------------------------------------|
| 00 类：操作成功                                                                   ||
| 00000 | 成功完成（SUCCESSFUL COMPLETION）                                          |
| 01 类：警告                                                                     ||
| 01000 | 警告（WARNING）                                                          |
| 0100C | 返回了动态结果（DYNAMIC RESULT SETS RETURNED）                                |
| 01008 | 警告，隐含补齐了零比特位（IMPLICIT ZERO BIT PADDING）                              |
| 01003 | 在集合函数里消除了空值（NULL VALUE ELIMINATED IN SET FUNCTION）                   |
| 01007 | 无权限（PRIVILEGE NOT GRANTED）                                           |
| 01006 | 没有撤销权限（PRIVILEGE NOT REVOKED）                                        |
| 01004 | 字串数据在右端截断（STRING DATA RIGHT TRUNCATION）                              |
| 01P01 | 废弃的特性（DEPRECATED FEATURE）                                            |
| 02 类：没有数据 --- 按照 SQL 标准的要求，这也是警告类                                           ||
| 02000 | 没有数据（NO DATA）                                                        |
| 02001 | 返回了没有附加动态结果集（NO ADDITIONAL DYNAMIC RESULT SETS RETURNED）             |
| 03 类：SQL语句尚未结束                                                              ||
| 03000 | SQL语句尚未结束（SQL STATEMENT NOT YET COMPLETE）                            |
| 08 类：连接异常                                                                   ||
| 08000 | 连接异常（CONNECTION EXCEPTION）                                           |
| 08003 | 连接不存在（CONNECTION DOES NOT EXIST）                                     |
| 08006 | 连接失败（CONNECTION FAILURE）                                             |
| 08001 | SQL客户端不能建立SQL连接（SQLCLIENT UNABLE TO ESTABLISH SQLCONNECTION）         |
| 08004 | SQL服务器拒绝建立SQL连接（SQLSERVER REJECTED ESTABLISHMENT OF SQLCONNECTION）   |
| 08007 | 未知的事务解析（TRANSACTION RESOLUTION UNKNOWN）                              |
| 08P01 | 违反协议（PROTOCOL VIOLATION）                                             |
| 09 类：触发器动作异常                                                                ||
| 09000 | 触发的动作异常（TRIGGERED ACTION EXCEPTION）                                  |
| 0A 类：不支持特性                                                                  ||
| 0A000 | 不支持此特性（FEATURE NOT SUPPORTED）                                        |
| 0B 类：非法事务初始化                                                                ||
| 0B000 | 非法事务初始化（INVALID TRANSACTION INITIATION）                              |
| 0F 类：指示器异常                                                                  ||
| 0F000 | 指示器异常（LOCATOR EXCEPTION）                                             |
| 0F001 | 非法的定位器声明（INVALID LOCATOR SPECIFICATION）                              |
| 0L 类：非法赋权                                                                   ||
| 0L000 | 非法赋权（INVALID GRANTOR）                                                |
| 0LP01 | 非法赋权操作（INVALID GRANT OPERATION）                                      |
| 0P 类：非法角色声明                                                                 ||
| 0P000 | 非法角色声明（INVALID ROLE SPECIFICATION）                                   |
| 21 类：势违反                                                                    ||
| 21000 | 势违反（CARDINALITY VIOLATION）                                           |
| 22 类：数据异常                                                                   ||
| 22000 | 数据异常（DATA EXCEPTION）                                                 |
| 2202E | 数组下标错误（ARRAY SUBSCRIPT ERROR）                                        |
| 22021 | 字符不在准备好的范围内（CHARACTER NOT IN REPERTOIRE）                             |
| 22008 | 日期时间字段溢出（DATETIME FIELD OVERFLOW）                                    |
| 22012 | 被零除（DIVISION BY ZERO）                                                |
| 22005 | 赋值中出错（ERROR IN ASSIGNMENT）                                           |
| 2200B | 逃逸字符冲突（ESCAPE CHARACTER CONFLICT）                                    |
| 22022 | 指示器溢出（INDICATOR OVERFLOW）                                            |
| 22015 | 内部字段溢出（INTERVAL FIELD OVERFLOW）                                      |
| 2201E | 对数运算的非法参数（INVALID ARGUMENT FOR LOGARITHM）                            |
| 2201F | 指数函数的非法参数（INVALID ARGUMENT FOR POWER FUNCTION）                       |
| 2201G | 宽桶函数的非法参数（INVALID ARGUMENT FOR WIDTH BUCKET FUNCTION）                |
| 22018 | 类型转换时非法的字符值（INVALID CHARACTER VALUE FOR CAST）                        |
| 22007 | 非法日期时间格式（INVALID DATETIME FORMAT）                                    |
| 22019 | 非法的逃逸字符（INVALID ESCAPE CHARACTER）                                    |
| 2200D | 非法的逃逸字节（INVALID ESCAPE OCTET）                                        |
| 22025 | 非法逃逸序列（INVALID ESCAPE SEQUENCE）                                      |
| 22P06 | 非标准使用逃逸字符（NONSTANDARD USE OF ESCAPE CHARACTER）                       |
| 22010 | 非法指示器参数值（INVALID INDICATOR PARAMETER VALUE）                          |
| 22020 | 非法限制值（INVALID LIMIT VALUE）                                           |
| 22023 | 非法参数值（INVALID PARAMETER VALUE）                                       |
| 2201B | 非法正则表达式（INVALID REGULAR EXPRESSION）                                  |
| 22009 | 非法时区显示值（INVALID TIME ZONE DISPLACEMENT VALUE）                        |
| 2200C | 非法使用逃逸字符（INVALID USE OF ESCAPE CHARACTER）                            |
| 2200G | 最相关类型不匹配（MOST SPECIFIC TYPE MISMATCH）                                |
| 22004 | 不允许NULL值（NULL VALUE NOT ALLOWED）                                     |
| 22002 | NULL值不能做指示器参数（NULL VALUE NO INDICATOR PARAMETER）                     |
| 22003 | 数字值超出范围（NUMERIC VALUE OUT OF RANGE）                                  |
| 22026 | 字串数据长度不匹配（STRING DATA LENGTH MISMATCH）                               |
| 22001 | 字串数据右边被截断（STRING DATA RIGHT TRUNCATION）                              |
| 22011 | 抽取子字串错误（SUBSTRING ERROR）                                             |
| 22027 | 截断错误（TRIM ERROR）                                                     |
| 22024 | 未结束的C字串（UNTERMINATED C STRING）                                       |
| 2200F | 零长度的字符串（ZERO LENGTH CHARACTER STRING）                                |
| 22P01 | 浮点异常（FLOATING POINT EXCEPTION）                                       |
| 22P02 | 非法文本表现形式（INVALID TEXT REPRESENTATION）                                |
| 22P03 | 非法二进制表现形式（INVALID BINARY REPRESENTATION）                             |
| 22P04 | 错误的COPY格式（BAD COPY FILE FORMAT）                                      |
| 22P05 | 不可翻译字符（UNTRANSLATABLE CHARACTER）                                     |
| 23 类：违反完整性约束                                                                ||
| 23000 | 违反完整性约束（INTEGRITY CONSTRAINT VIOLATION）                              |
| 23001 | 违反限制（RESTRICT VIOLATION）                                             |
| 23502 | 违反非空（NOT NULL VIOLATION）                                             |
| 23503 | 违反外键约束（FOREIGN KEY VIOLATION）                                        |
| 23505 | 违反唯一约束（UNIQUE VIOLATION）                                             |
| 23514 | 违反检查（CHECK VIOLATION）                                                |
| 24 类：非法游标状态                                                                 ||
| 24000 | 非法游标状态（INVALID CURSOR STATE）                                         |
| 25 类：非法事务状态                                                                 ||
| 25000 | 非法事务状态（INVALID TRANSACTION STATE）                                    |
| 25001 | 活跃的SQL状态（ACTIVE SQL TRANSACTION）                                     |
| 25002 | 分支事务已经激活（BRANCH TRANSACTION ALREADY ACTIVE）                          |
| 25008 | 持有的游标要求同样的隔离级别（HELD CURSOR REQUIRES SAME ISOLATION LEVEL）            |
| 25003 | 对分支事务的不恰当的访问方式（INAPPROPRIATE ACCESS MODE FOR BRANCH TRANSACTION）     |
| 25004 | 对分支事务的不恰当的隔离级别（INAPPROPRIATE ISOLATION LEVEL FOR BRANCH TRANSACTION） |
| 25005 | 分支事务没有活跃的SQL事务（NO ACTIVE SQL TRANSACTION FOR BRANCH TRANSACTION）     |
| 25006 | 只读的SQL事务（READ ONLY SQL TRANSACTION）                                  |
| 25007 | 不支持混和的模式和数据语句（SCHEMA AND DATA STATEMENT MIXING NOT SUPPORTED）        |
| 25P01 | 没有活跃的SQL事务（NO ACTIVE SQL TRANSACTION）                                |
| 25P02 | 在失败的SQL事务中（IN FAILED SQL TRANSACTION）                                |
| 26 类：非法SQL语句名                                                               ||
| 26000 | 非法SQL语句名（INVALID SQL STATEMENT NAME）                                 |
| 27 类：触发的数据改变违规                                                              ||
| 27000 | 触发的数据改变违规（TRIGGERED DATA CHANGE VIOLATION）                           |
| 28 类：非法授权声明                                                                 ||
| 28000 | 非法授权声明（INVALID AUTHORIZATION SPECIFICATION）                          |
| 2B 类：依然存在依赖的优先级描述符                                                          ||
| 2B000 | 依然存在依赖的优先级描述符（DEPENDENT PRIVILEGE DESCRIPTORS STILL EXIST）           |
| 2BP01 | 依赖性对象仍然存在（DEPENDENT OBJECTS STILL EXIST）                             |
| 2D 类：非法的事务终止                                                                ||
| 2D000 | 非法的事务终止（INVALID TRANSACTION TERMINATION）                             |
| 2F 类：SQL过程异常                                                                ||
| 2F000 | SQL过程异常（SQL ROUTINE EXCEPTION）                                       |
| 2F005 | 执行的函数没有返回语句（FUNCTION EXECUTED NO RETURN STATEMENT）                   |
| 2F002 | 不允许修改SQL数据（MODIFYING SQL DATA NOT PERMITTED）                         |
| 2F003 | 企图使用禁止的SQL语句（PROHIBITED SQL STATEMENT ATTEMPTED）                     |
| 2F004 | 不允许读取SQL数据（READING SQL DATA NOT PERMITTED）                           |
| 34 类：非法游标名                                                                  ||
| 34000 | 非法游标名（INVALID CURSOR NAME）                                           |
| 38 类：外部过程异常                                                                 ||
| 38000 | 外部过程异常（EXTERNAL ROUTINE EXCEPTION）                                   |
| 38001 | 不允许包含的SQL（CONTAINING SQL NOT PERMITTED）                              |
| 38002 | 不允许修改SQL数据（MODIFYING SQL DATA NOT PERMITTED）                         |
| 38003 | 企图使用禁止的SQL语句（PROHIBITED SQL STATEMENT ATTEMPTED）                     |
| 38004 | 不允许读取SQL数据（READING SQL DATA NOT PERMITTED）                           |
| 39 类：外部过程调用异常                                                               ||
| 39000 | 外部过程调用异常（EXTERNAL ROUTINE INVOCATION EXCEPTION）                      |
| 39001 | 返回了非法的SQLSTATE（INVALID SQLSTATE RETURNED）                            |
| 39004 | 不允许空值（NULL VALUE NOT ALLOWED）                                        |
| 39P01 | 违反触发器协议（TRIGGER PROTOCOL VIOLATED）                                   |
| 39P02 | 违反SRF协议（SRF PROTOCOL VIOLATED）                                       |
| 3B 类：保存点异常                                                                  ||
| 3B000 | 保存点异常（SAVEPOINT EXCEPTION）                                           |
| 3B001 | 无效的保存点声明（INVALID SAVEPOINT SPECIFICATION）                            |
| 3D 类：非法数据库名                                                                 ||
| 3D000 | 非法数据库名（INVALID CATALOG NAME）                                         |
| 3F 类：非法模式名                                                                  ||
| 3F000 | 非法模式名（INVALID SCHEMA NAME）                                           |
| 40 类：事务回滚                                                                   ||
| 40000 | 事务回滚（TRANSACTION ROLLBACK）                                           |
| 40002 | 违反事务完整性约束（TRANSACTION INTEGRITY CONSTRAINT VIOLATION）                |
| 40001 | 串行化失败（SERIALIZATION FAILURE）                                         |
| 40003 | 不知道语句是否结束（STATEMENT COMPLETION UNKNOWN）                              |
| 40P01 | 侦测到死锁（DEADLOCK DETECTED）                                             |
| 42 类：语法错误或者违反访问规则                                                           ||
| 42000 | 语法错误或者违反访问规则（SYNTAX ERROR OR ACCESS RULE VIOLATION）                  |
| 42601 | 语法错误（SYNTAX ERROR）                                                   |
| 42501 | 权限不够（INSUFFICIENT PRIVILEGE）                                         |
| 42846 | 无法进行类型转换（CANNOT COERCE）                                              |
| 42803 | 分组错误（GROUPING ERROR）                                                 |
| 42830 | 非法的外键（INVALID FOREIGN KEY）                                           |
| 42602 | 非法名字（INVALID NAME）                                                   |
| 42622 | 名字太长（NAME TOO LONG）                                                  |
| 42939 | 保留名字（RESERVED NAME）                                                  |
| 42804 | 数据类型不匹配（DATATYPE MISMATCH）                                           |
| 42P18 | 未决的数据类型（INDETERMINATE DATATYPE）                                      |
| 42809 | 错误的对象类型（WRONG OBJECT TYPE）                                           |
| 42703 | 未定义的字段（UNDEFINED COLUMN）                                             |
| 42883 | 未定义的函数（UNDEFINED FUNCTION）                                           |
| 42P01 | 未定义的表（UNDEFINED TABLE）                                               |
| 42P02 | 未定义的参数（UNDEFINED PARAMETER）                                          |
| 42704 | 未定义对象（UNDEFINED OBJECT）                                              |
| 42701 | 重复的字段（DUPLICATE COLUMN）                                              |
| 42P03 | 重复的游标（DUPLICATE CURSOR）                                              |
| 42P04 | 重复的数据库（DUPLICATE DATABASE））                                          |
| 42723 | 重复的函数（DUPLICATE FUNCTION）                                            |
| 42P05 | 重复的准备好语句（DUPLICATE PREPARED STATEMENT）                               |
| 42P06 | 重复的模式（DUPLICATE SCHEMA）                                              |
| 42P07 | 重复的表（DUPLICATE TABLE）                                                |
| 42712 | 重复的别名（DUPLICATE ALIAS）                                               |
| 42710 | 重复的对象（DUPLICATE OBJECT）                                              |
| 42702 | 模糊的字段（AMBIGUOUS COLUMN）                                              |
| 42725 | 模糊的函数（AMBIGUOUS FUNCTION）                                            |
| 42P08 | 模糊的参数（AMBIGUOUS PARAMETER）                                           |
| 42P09 | 模糊的别名（AMBIGUOUS ALIAS）                                               |
| 42P10 | 非法字段引用（INVALID COLUMN REFERENCE）                                     |
| 42611 | 非法字段定义（INVALID COLUMN DEFINITION）                                    |
| 42P11 | 非法游标定义（INVALID CURSOR DEFINITION）                                    |
| 42P12 | 非法的数据库定义（INVALID DATABASE DEFINITION）                                |
| 42P13 | 非法函数定义（INVALID FUNCTION DEFINITION）                                  |
| 42P14 | 非法准备好语句定义（INVALID PREPARED STATEMENT DEFINITION）                     |
| 42P15 | 非法模式定义（INVALID SCHEMA DEFINITION）                                    |
| 42P16 | 非法表定义（INVALID TABLE DEFINITION）                                      |
| 42P17 | 非法对象定义（INVALID OBJECT DEFINITION）                                    |
| 44 类：违反 WITH CHECK 选项                                                       ||
| 44000 | 违反 WITH CHECK 选项（WITH CHECK OPTION VIOLATION）                        |
| 53 类：资源不够                                                                   ||
| 53000 | 资源不够（INSUFFICIENT RESOURCES）                                         |
| 53100 | 磁盘满（DISK FULL）                                                       |
| 53200 | 内存耗尽（OUT OF MEMORY）                                                  |
| 53300 | 太多连接（TOO MANY CONNECTIONS）                                           |
| 54 类：超过程序限制                                                                 ||
| 54000 | 超过程序限制（PROGRAM LIMIT EXCEEDED）                                       |
| 54001 | 语句太复杂（STATEMENT TOO COMPLEX）                                         |
| 54011 | 太多字段（TOO MANY COLUMNS）                                               |
| 54023 | 参数太多（TOO MANY ARGUMENTS）                                             |
| 55 类：对象不在预先要求的状态                                                            ||
| 55000 | 对象不在预先要求的状态（OBJECT NOT IN PREREQUISITE STATE）                        |
| 55006 | 对象在使用中（OBJECT IN USE）                                                |
| 55P02 | 无法修改运行时参数（CANT CHANGE RUNTIME PARAM）                                 |
| 55P03 | 锁不可获得（LOCK NOT AVAILABLE）                                            |
| 57 类：操作者干涉                                                                  ||
| 57000 | 操作者干涉（OPERATOR INTERVENTION）                                         |
| 57014 | 查询被取消（QUERY CANCELED）                                                |
| 57P01 | 管理员关机（ADMIN SHUTDOWN）                                                |
| 57P02 | 崩溃关机（CRASH SHUTDOWN）                                                 |
| 57P03 | 现在无法连接（CANNOT CONNECT NOW）                                           |
| 58 类：系统错误( PostgreSQL 自己内部的错误)                                              ||
| 58030 | IO错误（IO ERROR）                                                       |
| 58P01 | 未定义的文件（UNDEFINED FILE）                                               |
| 58P02 | 重复的文件（DUPLICATE FILE）                                                |
| F0 类：配置文件错误                                                                 ||
| F0000 | 配置文件错误（CONFIG FILE ERROR）                                            |
| F0001 | 锁文件存在（LOCK FILE EXISTS）                                              |
| P0 类：PL/PGSQL 错误                                                            ||
| P0000 | PL/PGSQL 错误（PLPGSQL ERROR）                                           |
| P0001 | 抛出异常（RAISE EXCEPTION）                                                |
| XX 类：内部错误                                                                   ||
| XX000 | 内部错误（INTERNAL ERROR）                                                 |
| XX001 | 数据损坏（DATA CORRUPTED）                                                 |
| XX002 | 索引损坏（INDEX CORRUPTED）                                                |



