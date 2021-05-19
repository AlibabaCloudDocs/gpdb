使用COPY ON CONFLICT覆盖导入数据 
=============================================

COPY ON CONFLICT是[AnalyticDB PostgreSQL](/intl.zh-CN/产品简介/产品概述.md)针对COPY覆盖写入需求新增的功能。目前，COPY ON CONFLICT仅支持全表约束检查及全列覆盖写入。

在[AnalyticDB PostgreSQL](/intl.zh-CN/产品简介/产品概述.md)中，可以通过COPY快速导入数据，但是在COPY导入数据的过程中，如果数据与表的约束冲突，会导致COPY任务报错退出。[AnalyticDB PostgreSQL](/intl.zh-CN/产品简介/产品概述.md)提供COPY ON CONFLICT功能，支持在COPY过程中处理表的约束问题，使COPY任务不会因为约束冲突而失败。



语法 
--------------------

COPY ON CONFLICT目前支持COPY from，语法如下：

    COPY table [(column [, ...])] FROM {'file' | STDIN}
         [ [WITH] 
           [BINARY]
           [OIDS]
           [HEADER]
           [DELIMITER [ AS ] 'delimiter']
           [NULL [ AS ] 'null string']
           [ESCAPE [ AS ] 'escape' | 'OFF']
           [NEWLINE [ AS ] 'LF' | 'CR' | 'CRLF']
           [CSV [QUOTE [ AS ] 'quote'] 
                [FORCE NOT NULL column [, ...]]
           [FILL MISSING FIELDS]
           [[LOG ERRORS]  
           SEGMENT REJECT LIMIT count [ROWS | PERCENT] ]
        [DO ON CONFLICT DO UPDATE | NOTHING]



相比较[COPY](/intl.zh-CN/开发入门/SQL语法.md)，增加了DO ON CONFLICT DO UPDATE和DO ON CONFLICT DO NOTHING两个语法，分别对应于表约束冲突时全列更新和忽略输入的功能。



使用约束 
----------------------

* 目标表需为堆表，不支持AO表（AO表不支持unique index，所以无法支持AO表）；

  

* 目标表不支持分区表；

  

* 目标表不支持Updatable View；

  

* 仅支持COPY FROM，不支持COPY TO；

  

* 不支持CONFLICT指定index列，COPY ON CONFLICT默认判断所有约束列。若指定约束index列，则COPY执行失败，示例如下：

  




    COPY NATION FROM stdin DO ON CONFLICT(n_nationkey) DO UPDATE;
    ERROR:  COPY ON CONFLICT does NOT support CONFLICT index params



* 不支持指定更新列，COPY ON CONFLICT默认更新所有列。若指定更新列，则COPY执行失败，示例如下：

  




    COPY NATION FROM stdin DO ON CONFLICT DO UPDATE SET n_nationkey = excluded.n_nationkey;
    ERROR:  COPY ON CONFLICT does NOT support UPDATE SET targets





使用示例 
----------------------

1. 首先新建测试表NATION，该表共有4列，其中N_NATIONKEY为主键列，具有主键约束：

   




    CREATE TABLE NATION (
        N_NATIONKEY  INTEGER,
        N_NAME       CHAR(25),
        N_REGIONKEY  INTEGER,
        N_COMMENT    VARCHAR(152),
        PRIMARY KEY (N_NATIONKEY)
    );



通过COPY导入一部分数据：

    COPY NATION FROM stdin;
    0 'ALGERIA' 0 ' haggle. carefully final deposits detect slyly agai'
    1 'ARGENTINA' 1 'al foxes promise slyly according to the regular accounts. bold requests alon'
    2 'BRAZIL'  1 'y alongside of the pending deposits. carefully special packages are about the ironic forges. slyly speci'
    3 'CANADA'  1 'eas hang ironic, silent packages. slyly regular packages are furiously over the tithes. fluffily bold'
    \.



查询NATION表，可以看到已经导入的数据：

    SELECT * from NATION;
    
     n_nationkey |          n_name           | n_regionkey |                                                 n_comment                                                
      
    -------------+---------------------------+-------------+----------------------------------------------------------------------------------------------------------
    --
               2 | 'BRAZIL'                  |           1 | 'y alongside of the pending deposits. carefully special packages are about the ironic forges. slyly speci
    '
               3 | 'CANADA'                  |           1 | 'eas hang ironic, silent packages. slyly regular packages are furiously over the tithes. fluffily bold'
               0 | 'ALGERIA'                 |           0 | ' haggle. carefully final deposits detect slyly agai'
               1 | 'ARGENTINA'               |           1 | 'al foxes promise slyly according to the regular accounts. bold requests alon'
    (4 rows)





2. 普通COPY情形下，再插入主键冲突的一行数据：

    COPY NATION FROM stdin;
    0 'GERMANY' 3 'l platelets. regular accounts x-ray: unusual, regular acco'
    \.



此时执行会报错：

    ERROR:  duplicate key value violates unique constraint "nation_pkey"
    DETAIL:  Key (n_nationkey)=(0) already exists.
    CONTEXT:  COPY nation, line 1





3. 使用COPY ON CONFLICT功能，在主键冲突的情况下，UPDATE数据：

    COPY NATION FROM stdin DO on conflict DO update;
    0 'GERMANY' 3 'l platelets. regular accounts x-ray: unusual, regular acco'
    \.



此时COPY不会报错，查询NATION表可以看到主键等于0的一行已经更新：

    SELECT * from NATION;
    
     n_nationkey |          n_name           | n_regionkey |                                                 n_comment                                                
      
    -------------+---------------------------+-------------+----------------------------------------------------------------------------------------------------------
    --
               2 | 'BRAZIL'                  |           1 | 'y alongside of the pending deposits. carefully special packages are about the ironic forges. slyly speci
    '
               3 | 'CANADA'                  |           1 | 'eas hang ironic, silent packages. slyly regular packages are furiously over the tithes. fluffily bold'
               1 | 'ARGENTINA'               |           1 | 'al foxes promise slyly according to the regular accounts. bold requests alon'
               0 | 'GERMANY'                 |           3 | 'l platelets. regular accounts x-ray: unusual, regular acco'
    (4 rows)





4. 使用COPY ON CONFLICT功能，在主键冲突的情况下，忽略输入：

    COPY NATION FROM stdin DO on conflict DO nothing;
    1 'GERMANY' 3 'l platelets. regular accounts x-ray: unusual, regular acco'
    \.



此时COPY不会报错，查询NATION表可以看到主键等于1的一行没有被更新：

    SELECT * from NATION;
    
     n_nationkey |          n_name           | n_regionkey |                                                 n_comment                                                
      
    -------------+---------------------------+-------------+----------------------------------------------------------------------------------------------------------
    --
               2 | 'BRAZIL'                  |           1 | 'y alongside of the pending deposits. carefully special packages are about the ironic forges. slyly speci
    '
               3 | 'CANADA'                  |           1 | 'eas hang ironic, silent packages. slyly regular packages are furiously over the tithes. fluffily bold'
               1 | 'ARGENTINA'               |           1 | 'al foxes promise slyly according to the regular accounts. bold requests alon'
               0 | 'GERMANY'                 |           3 | 'l platelets. regular accounts x-ray: unusual, regular acco'
    (4 rows)





