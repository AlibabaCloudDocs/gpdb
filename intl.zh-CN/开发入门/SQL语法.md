# SQL语法

本文介绍AnalyticDB for PostgreSQL支持的SQL命令及语法参考。

-   [ABORT](#section_jxg_vsg_thb)
-   [ALTER AGGREGATE](#section_bzv_1tg_thb)
-   [ALTER CONVERSION](#section_x3s_2tg_thb)
-   [ALTER DATABASE](#section_frf_htg_thb)
-   [ALTER DOMAIN](#section_in3_ktg_thb)
-   [ALTER EXTERNAL TABLE](#section_nsf_mtg_thb)
-   [ALTER FUNCTION](#section_phk_4tg_thb)
-   [ALTER GROUP](#section_ecl_qtg_thb)
-   [ALTER INDEX](#section_e1c_ttg_thb)
-   [ALTER OPERATOR](#section_thj_vtg_thb)
-   [ALTER RESOURCE QUEUE](#section_oz3_xtg_thb)
-   [ALTER ROLE](#section_xss_ztg_thb)
-   [ALTER SCHEMA](#section_jgh_c5g_thb)
-   [ALTER SEQUENCE](#section_bjs_tfh_thb)
-   [ALTER TABLE](#section_p5m_wfh_thb)
-   [ALTER TYPE](#section_vbv_yfh_thb)
-   [ALTER USER](#section_kzs_1gh_thb)
-   [ANALYZE](#section_bbf_dgh_thb)
-   [BEGIN](#section_xjm_fgh_thb)
-   [CHECKPOINT](#section_pcn_hgh_thb)
-   [CLOSE](#section_wrk_jgh_thb)
-   [CLUSTER](#section_y4y_lgh_thb)
-   [COMMENT](#section_dxy_ngh_thb)
-   [COMMIT](#section_i53_qgh_thb)
-   [COPY](#section_x51_sgh_thb)
-   [CREATE AGGREGATE](#section_olf_5gh_thb)
-   [CREATE CAST](#section_bmm_wgh_thb)
-   [CREATE CONVERSION](#section_tlm_ygh_thb)
-   [CREATE DATABASE](#section_o3l_1hh_thb)
-   [CREATE DOMAIN](#section_u2y_dhh_thb)
-   [CREATE EXTENSION](#section_wrd_hhh_thb)
-   [CREATE EXTERNAL TABLE](#section_dfy_3hh_thb)
-   [CREATE FUNCTION](#section_ywk_nhh_thb)
-   [CREATE GROUP](#section_ggl_phh_thb)
-   [CREATE INDEX](#section_icn_zhh_thb)
-   [CREATE LIBRARY](#section_d2n_l3h_thb)
-   [CREATE OPERATOR](#section_vnq_n3h_thb)
-   [CREATE RESOURCE QUEUE](#section_w1s_p3h_thb)
-   [CREATE ROLE](#section_dfs_r3h_thb)
-   [CREATE RULE](#section_gm3_t3h_thb)
-   [CREATE SCHEMA](#section_dzh_v3h_thb)
-   [CREATE SEQUENCE](#section_xqc_x3h_thb)
-   [CREATE TABLE](#section_hjr_cjh_thb)
-   [CREATE TABLE AS](#section_isp_fjh_thb)
-   [CREATE TYPE](#section_oxj_hjh_thb)
-   [CREATE USER](#section_rgd_kjh_thb)
-   [CREATE VIEW](#section_ff4_mjh_thb)
-   [DEALLOCATE](#section_yg2_4jh_thb)
-   [DECLARE](#section_khv_pjh_thb)
-   [DELETE](#section_mtt_rjh_thb)
-   [DROP AGGREGATE](#section_avr_tjh_thb)
-   [DROP CAST](#section_njn_vjh_thb)
-   [DROP CONVERSION](#section_zfh_xjh_thb)
-   [DROP DATABASE](#section_gg3_zjh_thb)
-   [DROP DOMAIN](#section_trv_1kh_thb)
-   [DROP EXTENSION](#section_gpp_ckh_thb)
-   [DROP EXTERNAL TABLE](#section_f13_2kh_thb)
-   [DROP FUNCTION](#section_txc_gkh_thb)
-   [DROP GROUP](#section_d31_3kh_thb)
-   [DROP INDEX](#section_rxn_jkh_thb)
-   [DROP LIBRARY](#section_itj_lkh_thb)
-   [DROP OPERATOR](#section_pvd_nkh_thb)
-   [DROP OWNED](#section_xft_4kh_thb)
-   [DROP RESOURCE QUEUE](#section_dzk_qkh_thb)
-   [DROP ROLE](#section_pcy_rkh_thb)
-   [DROP RULE](#section_wkn_tkh_thb)
-   [DROP SCHEMA](#section_elc_vkh_thb)
-   [DROP SEQUENCE](#section_bj4_wkh_thb)
-   [DROP TABLE](#section_bsz_xkh_thb)
-   [DROP TYPE](#section_pnb_1lh_thb)
-   [DROP USER](#section_b4w_blh_thb)
-   [DROP VIEW](#section_mny_dlh_thb)
-   [END](#section_osj_flh_thb)
-   [EXECUTE](#section_p5b_3lh_thb)
-   [EXPLAIN](#section_cjt_jlh_thb)
-   [FETCH](#section_sp3_llh_thb)
-   [GRANT](#section_rfx_mlh_thb)
-   [INSERT](#section_vgd_plh_thb)
-   [LOAD](#section_hlx_qlh_thb)
-   [LOCK](#section_hzt_wlh_thb)
-   [MOVE](#section_kcv_ylh_thb)
-   [PREPARE](#section_zy5_1mh_thb)
-   [REASSIGN OWNED](#section_bxv_cmh_thb)
-   [REINDEX](#section_all_lmh_thb)
-   [RELEASE SAVEPOINT](#section_vrd_nmh_thb)
-   [RESET](#section_zgs_qmh_thb)
-   [REVOKE](#section_emm_smh_thb)
-   [ROLLBACK](#section_vcx_5mh_thb)
-   [ROLLBACK TO SAVEPOINT](#section_ot3_wmh_thb)
-   [SAVEPOINT](#section_vhp_ymh_thb)
-   [SELECT](#section_qkn_1nh_thb)
-   [SELECT INTO](#section_mb3_cnh_thb)
-   [SET](#section_dbm_2nh_thb)
-   [SET ROLE](#section_adw_fnh_thb)
-   [SET SESSION AUTHORIZATION](#section_q5h_hnh_thb)
-   [SET TRANSACTION](#section_am3_jnh_thb)
-   [SHOW](#section_wlb_nnh_thb)
-   [START TRANSACTION](#section_mwq_4nh_thb)
-   [TRUNCATE](#section_l45_qnh_thb)
-   [UPDATE](#section_hxp_snh_thb)
-   [VACUUM](#section_y5j_5nh_thb)
-   [VALUES](#section_hjy_vnh_thb)

## ABORT

终止当前事务。

```
ABORT [WORK | TRANSACTION]
```

更多信息请参考[ABORT](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ABORT.html)。

## ALTER AGGREGATE

改变聚集函数的定义。

```
ALTER AGGREGATE name ( type [ , ... ] ) RENAME TO new_name
ALTER AGGREGATE name ( type [ , ... ] ) OWNER TO new_owner
ALTER AGGREGATE name ( type [ , ... ] ) SET SCHEMA new_schema
```

更多信息请参考[ALTER AGGREGATE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_AGGREGATE.html)。

## ALTER CONVERSION

修改转换的定义。

```
ALTER CONVERSION name RENAME TO newname
ALTER CONVERSION name OWNER TO newowner
```

更多信息请参考[ALTER CONVERSION](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_CONVERSION.html)。

## ALTER DATABASE

修改数据库属性。

```
ALTER DATABASE name [ WITH CONNECTION LIMIT connlimit ]
ALTER DATABASE name SET parameter { TO | = } { value | DEFAULT }
ALTER DATABASE name RESET parameter
ALTER DATABASE name RENAME TO newname
ALTER DATABASE name OWNER TO new_owner
```

更多信息请参考[ALTER DATABASE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_DATABASE.html)。

## ALTER DOMAIN

改变域的定义。

```
ALTER DOMAIN name { SET DEFAULT expression | DROP DEFAULT }
ALTER DOMAIN name { SET | DROP } NOT NULL
ALTER DOMAIN name ADD domain_constraint
ALTER DOMAIN name DROP CONSTRAINT constraint_name [RESTRICT | CASCADE]
ALTER DOMAIN name OWNER TO new_owner
ALTER DOMAIN name SET SCHEMA new_schema
```

更多信息请参阅[ALTER DOMAIN](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_DOMAIN.html)。

## ALTER EXTERNAL TABLE

改变外部表的定义。

```
ALTER EXTERNAL TABLE name RENAME [COLUMN] column TO new_column
ALTER EXTERNAL TABLE name RENAME TO new_name
ALTER EXTERNAL TABLE name SET SCHEMA new_schema
ALTER EXTERNAL TABLE name action [, ... ]
```

更多信息请参考[ALTER EXTERNAL TABLE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_EXTERNAL_TABLE.html)。

## ALTER FUNCTION

改变函数的定义。

```
ALTER FUNCTION name ( [ [argmode] [argname] argtype [, ...] ] ) 
   action [, ... ] [RESTRICT]
ALTER FUNCTION name ( [ [argmode] [argname] argtype [, ...] ] )
   RENAME TO new_name
ALTER FUNCTION name ( [ [argmode] [argname] argtype [, ...] ] ) 
   OWNER TO new_owner
ALTER FUNCTION name ( [ [argmode] [argname] argtype [, ...] ] ) 
   SET SCHEMA new_schema
```

更多信息请参考[ALTER FUNCTION](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_FUNCTION.html)。

## ALTER GROUP

改变角色名字或者成员信息。

```
ALTER GROUP groupname ADD USER username [, ... ]
ALTER GROUP groupname DROP USER username [, ... ]
ALTER GROUP groupname RENAME TO newname
```

更多信息请参考[ALTER GROUP](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_GROUP.html)。

## ALTER INDEX

改变索引的定义。

```
ALTER INDEX name RENAME TO new_name
ALTER INDEX name SET TABLESPACE tablespace_name
ALTER INDEX name SET ( FILLFACTOR = value )
ALTER INDEX name RESET ( FILLFACTOR )
```

更多信息请参考[ALTER INDEX](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_INDEX.html)。

## ALTER OPERATOR

改变操作符的定义。

```
ALTER OPERATOR name ( {lefttype | NONE} , {righttype | NONE} ) 
   OWNER TO newowner
```

更多信息请参考[ALTER OPERATOR](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_OPERATOR.html)。

## ALTER RESOURCE QUEUE

修改资源队列的限制。

```
ALTER RESOURCE QUEUE name WITH ( queue_attribute=value [, ... ] )
```

更多信息请参考[ALTER RESOURCE QUEUE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_RESOURCE_QUEUE.html)。

## ALTER ROLE

```
ALTER ROLE name RENAME TO newname

ALTER ROLE name SET config_parameter {TO | =} {value | DEFAULT}

ALTER ROLE name RESET config_parameter

ALTER ROLE name RESOURCE QUEUE {queue_name | NONE}

ALTER ROLE name [ [WITH] option [ ... ] ]
```

更多信息请参考[ALTER ROLE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_ROLE.html)。

## ALTER SCHEMA

改变模式的定义。

```
ALTER SCHEMA name RENAME TO newname

ALTER SCHEMA name OWNER TO newowner
```

更多信息请参考[ALTER SCHEMA](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_SCHEMA.html)。

## ALTER SEQUENCE

改变序列生成器的定义。

```
ALTER SEQUENCE name [INCREMENT [ BY ] increment] 
     [MINVALUE minvalue | NO MINVALUE] 
     [MAXVALUE maxvalue | NO MAXVALUE] 
     [RESTART [ WITH ] start] 
     [CACHE cache] [[ NO ] CYCLE] 
     [OWNED BY {table.column | NONE}]
ALTER SEQUENCE name SET SCHEMA new_schema
```

更多信息请参考[ALTER SEQUENCE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_SEQUENCE.html)。

## ALTER TABLE

改变的表的定义。

```
ALTER TABLE [ONLY] name RENAME [COLUMN] column TO new_column

ALTER TABLE name RENAME TO new_name

ALTER TABLE name SET SCHEMA new_schema

ALTER TABLE [ONLY] name SET 
     DISTRIBUTED BY (column, [ ... ] ) 
   | DISTRIBUTED RANDOMLY 
   | WITH (REORGANIZE=true|false)

ALTER TABLE [ONLY] name action [, ... ]

ALTER TABLE name
   [ ALTER PARTITION { partition_name | FOR (RANK(number)) 
   | FOR (value) } partition_action [...] ] 
   partition_action
```

更多信息请参考[ALTER TABLE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_TABLE.html)。

## ALTER TYPE

改变数据类型的定义。

```
ALTER TYPE name
   OWNER TO new_owner | SET SCHEMA new_schema
```

更多信息请参考[ALTER TYPE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_TYPE.html)。

## ALTER USER

修改数据库角色（用户）的定义。

```
ALTER USER name RENAME TO newname

ALTER USER name SET config_parameter {TO | =} {value | DEFAULT}

ALTER USER name RESET config_parameter

ALTER USER name [ [WITH] option [ ... ] ]
```

更多信息请参考[ALTER USER](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_USER.html)。

## ANALYZE

收集关于数据库的数据。

```
ANALYZE [VERBOSE] [ROOTPARTITION [ALL] ] 
   [table [ (column [, ...] ) ]]
```

更多信息请参考 [ANALYZE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ANALYZE.html)。

## BEGIN

启动事务块。

```
BEGIN [WORK | TRANSACTION] [transaction_mode]
      [READ ONLY | READ WRITE]
```

更多信息请参考[BEGIN](http://docs.greenplum.org/6-4/ref_guide/sql_commands/BEGIN.html) 。

## CHECKPOINT

强制事务记录检查点。

```
CHECKPOINT
```

更多信息请参考[CHECKPOINT](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CHECKPOINT.html)。

## CLOSE

关闭游标。

```
CLOSE cursor_name
```

更多信息请参考[CLOSE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CLOSE.html) 。

## CLUSTER

根据索引对磁盘上的堆存储表进行物理重新排序。不是推荐使用该操作。

```
CLUSTER indexname ON tablename

CLUSTER tablename

CLUSTER
```

更多信息请参考[CLUSTER](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CLUSTER.html)。

## COMMENT

定义或者修改对一个对象的注释。

```
COMMENT ON
{ TABLE object_name |
  COLUMN table_name.column_name |
  AGGREGATE agg_name (agg_type [, ...]) |
  CAST (sourcetype AS targettype) |
  CONSTRAINT constraint_name ON table_name |
  CONVERSION object_name |
  DATABASE object_name |
  DOMAIN object_name |
  FILESPACE object_name |
  FUNCTION func_name ([[argmode] [argname] argtype [, ...]]) |
  INDEX object_name |
  LARGE OBJECT large_object_oid |
  OPERATOR op (leftoperand_type, rightoperand_type) |
  OPERATOR CLASS object_name USING index_method |
  [PROCEDURAL] LANGUAGE object_name |
  RESOURCE QUEUE object_name |
  ROLE object_name |
  RULE rule_name ON table_name |
  SCHEMA object_name |
  SEQUENCE object_name |
  TABLESPACE object_name |
  TRIGGER trigger_name ON table_name |
  TYPE object_name |
  VIEW object_name } 
IS 'text'
```

更多信息请参考[COMMENT](http://docs.greenplum.org/6-4/ref_guide/sql_commands/COMMENT.html)。

## COMMIT

提交当前事务。

```
COMMIT [WORK | TRANSACTION]
```

更多信息请参考[COMMIT](http://docs.greenplum.org/6-4/ref_guide/sql_commands/COMMIT.html)。

## COPY

在文件和表之间拷贝数据。

```
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

COPY {table [(column [, ...])] | (query)} TO {'file' | STDOUT}
      [ [WITH] 
        [ON SEGMENT]
        [BINARY]
        [OIDS]
        [HEADER]
        [DELIMITER [ AS ] 'delimiter']
        [NULL [ AS ] 'null string']
        [ESCAPE [ AS ] 'escape' | 'OFF']
        [CSV [QUOTE [ AS ] 'quote'] 
             [FORCE QUOTE column [, ...]] ]
      [IGNORE EXTERNAL PARTITIONS ]
```

更多信息请参考[COPY](http://docs.greenplum.org/6-4/ref_guide/sql_commands/COPY.html) 。

## CREATE AGGREGATE

定义一个新的聚集函数。

```
CREATE [ORDERED] AGGREGATE name (input_data_type [ , ... ]) 
      ( SFUNC = sfunc,
        STYPE = state_data_type
        [, PREFUNC = prefunc]
        [, FINALFUNC = ffunc]
        [, INITCOND = initial_condition]
        [, SORTOP = sort_operator] )
```

更多信息请参考[CREATE AGGREGATE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_AGGREGATE.html)。

## CREATE CAST

定义一个新的CAST。

```
CREATE CAST (sourcetype AS targettype) 
       WITH FUNCTION funcname (argtypes) 
       [AS ASSIGNMENT | AS IMPLICIT]

CREATE CAST (sourcetype AS targettype) WITHOUT FUNCTION 
       [AS ASSIGNMENT | AS IMPLICIT]
```

更多信息请参考[CREATE CAST](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_CAST.html)。

## CREATE CONVERSION

定义一个新的编码转换。

```
CREATE [DEFAULT] CONVERSION name FOR source_encoding TO 
     dest_encoding FROM funcname
```

更多信息请参考[CREATE CONVERSION](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_CONVERSION.html)。

## CREATE DATABASE

创建一个新的数据库。

```
CREATE DATABASE name [ [WITH] [OWNER [=] dbowner]
                     [TEMPLATE [=] template]
                     [ENCODING [=] encoding]
                     [CONNECTION LIMIT [=] connlimit ] ]
```

更多信息请参考[CREATE DATABASE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_DATABASE.html)。

## CREATE DOMAIN

定义一个新的域。

```
CREATE DOMAIN name [AS] data_type [DEFAULT expression] 
       [CONSTRAINT constraint_name
       | NOT NULL | NULL 
       | CHECK (expression) [...]]
```

更多信息请参考[CREATE DOMAIN](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_DOMAIN.html)。

## CREATE EXTENSION

在数据库中注册一个EXTENSION。

```
CREATE EXTENSION [ IF NOT EXISTS ] extension_name
  [ WITH ] [ SCHEMA schema_name ]
           [ VERSION version ]
           [ FROM old_version ]
           [ CASCADE ]
```

更多信息请参考[CREATE EXTENSION](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_EXTENSION.html)。

## CREATE EXTERNAL TABLE

定义一张外部表。

```
CREATE [READABLE] EXTERNAL TABLE tablename
( columnname datatype [, ...] | LIKE othertable )
LOCATION ('ossprotocol')
FORMAT 'TEXT'
            [( [HEADER]
               [DELIMITER [AS] 'delimiter' | 'OFF']
               [NULL [AS] 'null string']
               [ESCAPE [AS] 'escape' | 'OFF']
               [NEWLINE [ AS ] 'LF' | 'CR' | 'CRLF']
               [FILL MISSING FIELDS] )]
           | 'CSV'
            [( [HEADER]
               [QUOTE [AS] 'quote']
               [DELIMITER [AS] 'delimiter']
               [NULL [AS] 'null string']
               [FORCE NOT NULL column [, ...]]
               [ESCAPE [AS] 'escape']
               [NEWLINE [ AS ] 'LF' | 'CR' | 'CRLF']
               [FILL MISSING FIELDS] )]
[ ENCODING 'encoding' ]
[ [LOG ERRORS [INTO error_table]] SEGMENT REJECT LIMIT count
       [ROWS | PERCENT] ]
CREATE WRITABLE EXTERNAL TABLE table_name
( column_name data_type [, ...] | LIKE other_table )
LOCATION ('ossprotocol')
FORMAT 'TEXT'
               [( [DELIMITER [AS] 'delimiter']
               [NULL [AS] 'null string']
               [ESCAPE [AS] 'escape' | 'OFF'] )]
          | 'CSV'
               [([QUOTE [AS] 'quote']
               [DELIMITER [AS] 'delimiter']
               [NULL [AS] 'null string']
               [FORCE QUOTE column [, ...]] ]
               [ESCAPE [AS] 'escape'] )]
[ ENCODING 'encoding' ]
[ DISTRIBUTED BY (column, [ ... ] ) | DISTRIBUTED RANDOMLY ]
ossprotocol:
   oss://oss_endpoint prefix=prefix_name
    id=userossid key=userosskey bucket=ossbucket compressiontype=[none|gzip] async=[true|false]
ossprotocol:
   oss://oss_endpoint dir=[folder/[folder/]...]/file_name
    id=userossid key=userosskey bucket=ossbucket compressiontype=[none|gzip] async=[true|false]
ossprotocol:
   oss://oss_endpoint filepath=[folder/[folder/]...]/file_name
id=userossid key=userosskey bucket=ossbucket compressiontype=[none|gzip] async=[true|false]
```

更多信息请参考[CREATE EXTERNAL TABLE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_EXTERNAL_TABLE.html)。

## CREATE FUNCTION

定义一个新的函数。

```
CREATE [OR REPLACE] FUNCTION name    
    ( [ [argmode] [argname] argtype [ { DEFAULT | = } defexpr ] [, ...] ] )
      [ RETURNS { [ SETOF ] rettype 
        | TABLE ([{ argname argtype | LIKE other table }
          [, ...]])
        } ]
    { LANGUAGE langname
    | IMMUTABLE | STABLE | VOLATILE
    | CALLED ON NULL INPUT | RETURNS NULL ON NULL INPUT | STRICT
    | [EXTERNAL] SECURITY INVOKER | [EXTERNAL] SECURITY DEFINE
    | COST execution_cost
    | SET configuration_parameter { TO value | = value | FROM CURRENT }
    | AS 'definition'
    | AS 'obj_file', 'link_symbol' } ...
    [ WITH ({ DESCRIBE = describe_function
           } [, ...] ) ]
```

更多信息请参考[CREATE FUNCTION](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_FUNCTION.html)。

## CREATE GROUP

定义一个新的数据库角色。

```
CREATE GROUP name [ [WITH] option [ ... ] ]
```

更多信息请参考[CREATE GROUP](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_GROUP.html)。

## CREATE INDEX

定义一个新的索引。

```
CREATE [UNIQUE] INDEX name ON table
       [USING btree|bitmap|gist]
       ( {column | (expression)} [opclass] [, ...] )
       [ WITH ( FILLFACTOR = value ) ]
       [TABLESPACE tablespace]
       [WHERE predicate]
```

更多信息请参考[CREATE INDEX](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_INDEX.html)。

## CREATE LIBRARY

定义一个用户自定义软件表。

```
CREATE LIBRARY library_name LANGUAGE [JAVA] FROM oss_location OWNER ownername
CREATE LIBRARY library_name LANGUAGE [JAVA] VALUES file_content_hex OWNER ownername
```

更多信息请参考[CREATE LIBRARY](https://www.alibabacloud.com/help/zh/doc-detail/126635.html)。

## CREATE OPERATOR

定义一个新的操作符。

```
CREATE OPERATOR name ( 
       PROCEDURE = funcname
       [, LEFTARG = lefttype] [, RIGHTARG = righttype]
       [, COMMUTATOR = com_op] [, NEGATOR = neg_op]
       [, RESTRICT = res_proc] [, JOIN = join_proc]
       [, HASHES] [, MERGES]
       [, SORT1 = left_sort_op] [, SORT2 = right_sort_op]
       [, LTCMP = less_than_op] [, GTCMP = greater_than_op] )
```

更多信息请参考[CREATE OPERATOR](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_OPERATOR.html)。

## CREATE RESOURCE QUEUE

定义一个新的资源队列。

```
CREATE RESOURCE QUEUE name WITH (queue_attribute=value [, ... ])
```

更多信息请参考[CREATE RESOURCE QUEUE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_RESOURCE_QUEUE.html)。

## CREATE ROLE

定义一个新的数据库角色（用户或组）。

```
CREATE ROLE name [[WITH] option [ ... ]]
```

更多信息请参考[CREATE ROLE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_ROLE.html)。

## CREATE RULE

定义一个新的重写规则。

```
CREATE [OR REPLACE] RULE name AS ON event
  TO table [WHERE condition] 
  DO [ALSO | INSTEAD] { NOTHING | command | (command; command 
  ...) }
```

更多信息请参考[CREATE RULE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_RULE.html)。

## CREATE SCHEMA

定义一个新的SCHEMA。

```
CREATE SCHEMA schema_name [AUTHORIZATION username] 
   [schema_element [ ... ]]

CREATE SCHEMA AUTHORIZATION rolename [schema_element [ ... ]]
```

更多信息请参考[CREATE SCHEMA](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_SCHEMA.html)。

## CREATE SEQUENCE

定义一个新的序列生成器。

```
CREATE [TEMPORARY | TEMP] SEQUENCE name
       [INCREMENT [BY] value] 
       [MINVALUE minvalue | NO MINVALUE] 
       [MAXVALUE maxvalue | NO MAXVALUE] 
       [START [ WITH ] start] 
       [CACHE cache] 
       [[NO] CYCLE] 
       [OWNED BY { table.column | NONE }]
```

更多信息请参考[CREATE SEQUENCE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_SEQUENCE.html)。

## CREATE TABLE

定义一个新的表。

```
CREATE [[GLOBAL | LOCAL] {TEMPORARY | TEMP}] TABLE table_name ( 
[ { column_name data_type [ DEFAULT default_expr ] 
   [column_constraint [ ... ]
[ ENCODING ( storage_directive [,...] ) ]
] 
   | table_constraint
   | LIKE other_table [{INCLUDING | EXCLUDING} 
                      {DEFAULTS | CONSTRAINTS}] ...}
   [, ... ] ]
   )
   [ INHERITS ( parent_table [, ... ] ) ]
   [ WITH ( storage_parameter=value [, ... ] )
   [ ON COMMIT {PRESERVE ROWS | DELETE ROWS | DROP} ]
   [ DISTRIBUTED BY (column, [ ... ] ) | DISTRIBUTED RANDOMLY ]
   [ PARTITION BY partition_type (column)
       [ SUBPARTITION BY partition_type (column) ] 
          [ SUBPARTITION TEMPLATE ( template_spec ) ]
       [...]
    ( partition_spec ) 
        | [ SUBPARTITION BY partition_type (column) ]
          [...]
    ( partition_spec
      [ ( subpartition_spec
           [(...)] 
         ) ] 
    )
```

更多信息请参考[CREATE TABLE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_TABLE.html)。

## CREATE TABLE AS

从查询的结果中定义一个新的表。

```
CREATE [ [GLOBAL | LOCAL] {TEMPORARY | TEMP} ] TABLE table_name
   [(column_name [, ...] )]
   [ WITH ( storage_parameter=value [, ... ] ) ]
   [ON COMMIT {PRESERVE ROWS | DELETE ROWS | DROP}]
   [TABLESPACE tablespace]
   AS query
   [DISTRIBUTED BY (column, [ ... ] ) | DISTRIBUTED RANDOMLY]
```

更多信息请参考[CREATE TABLE AS](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_TABLE_AS.html)。

## CREATE TYPE

定义一个新的类型。

```
CREATE TYPE name AS ( attribute_name data_type [, ... ] )

CREATE TYPE name AS ENUM ( 'label' [, ... ] )

CREATE TYPE name (
    INPUT = input_function,
    OUTPUT = output_function
    [, RECEIVE = receive_function]
    [, SEND = send_function]
    [, TYPMOD_IN = type_modifier_input_function ]
    [, TYPMOD_OUT = type_modifier_output_function ]
    [, INTERNALLENGTH = {internallength | VARIABLE}]
    [, PASSEDBYVALUE]
    [, ALIGNMENT = alignment]
    [, STORAGE = storage]
    [, DEFAULT = default]
    [, ELEMENT = element]
    [, DELIMITER = delimiter] )

CREATE TYPE name
```

更多信息请参考[CREATE TYPE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_TYPE.html)。

## CREATE USER

定义一个默认带有LOGIN权限的数据库角色。

```
CREATE USER name [ [WITH] option [ ... ] ]
```

更多信息请参考[CREATE USER](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_USER.html)。

## CREATE VIEW

定义一个新的视图。

```
CREATE [OR REPLACE] [TEMP | TEMPORARY] VIEW name
       [ ( column_name [, ...] ) ]
       AS query
```

更多信息请参考[CREATE VIEW](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_VIEW.html)。

## DEALLOCATE

取消分配一个预编译的语句。

```
DEALLOCATE [PREPARE] name
```

更多信息请参考[DEALLOCATE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DEALLOCATE.html)。

## DECLARE

定义一个游标。

```
DECLARE name [BINARY] [INSENSITIVE] [NO SCROLL] CURSOR 
     [{WITH | WITHOUT} HOLD] 
     FOR query [FOR READ ONLY]
```

更多信息请参考[DECLARE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DECLARE.html)。

## DELETE

从表中删除行。

```
DELETE FROM [ONLY] table [[AS] alias]
      [USING usinglist]
      [WHERE condition | WHERE CURRENT OF cursor_name ]
```

更多信息请参考[DELETE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DELETE.html)。

## DROP AGGREGATE

删除聚集函数。

```
DROP AGGREGATE [IF EXISTS] name ( type [, ...] ) [CASCADE | RESTRICT]
```

更多信息请参考[DROP AGGREGATE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_AGGREGATE.html)。

## DROP CAST

删除一个CAST。

```
DROP CAST [IF EXISTS] (sourcetype AS targettype) [CASCADE | RESTRICT]
```

更多信息请参考[DROP CAST](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_CAST.html)。

## DROP CONVERSION

删除一个转换。

```
DROP CONVERSION [IF EXISTS] name [CASCADE | RESTRICT]
```

更多信息请参考[DROP CONVERSION](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_CONVERSION.html)。

## DROP DATABASE

删除一个数据库。

```
DROP DATABASE [IF EXISTS] name
```

更多信息请参考[DROP DATABASE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_DATABASE.html)。

## DROP DOMAIN

删除一个域。

```
DROP DOMAIN [IF EXISTS] name [, ...]  [CASCADE | RESTRICT]
```

更多信息请参考[DROP DOMAIN](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_DOMAIN.html)。

## DROP EXTENSION

从数据库中删除一个扩展。

```
DROP EXTENSION [ IF EXISTS ] name [, ...] [ CASCADE | RESTRICT ]
```

更多信息请参考[DROP EXTENSION](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_EXTENSION.html)。

## DROP EXTERNAL TABLE

删除一个外部表定义。

```
DROP EXTERNAL [WEB] TABLE [IF EXISTS] name [CASCADE | RESTRICT]
```

更多信息请参考[DROP EXTERNAL TABLE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_EXTERNAL_TABLE.html)。

## DROP FUNCTION

删除一个函数。

```
DROP FUNCTION [IF EXISTS] name ( [ [argmode] [argname] argtype 
    [, ...] ] ) [CASCADE | RESTRICT]
```

更多信息请参考[DROP FUNCTION](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_FUNCTION.html)。

## DROP GROUP

删除一个数据库角色。

```
DROP GROUP [IF EXISTS] name [, ...]
```

更多信息请参考[DROP GROUP](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_GROUP.html)。

## DROP INDEX

删除一个索引。

```
DROP INDEX [IF EXISTS] name [, ...] [CASCADE | RESTRICT]
```

更多信息请参考[DROP INDEX](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_INDEX.html)。

## DROP LIBRARY

删除一个用户定义软件包。

```
DROP LIBRARY library_name
```

更多信息请参考[DROP LIBRARY](https://www.alibabacloud.com/help/zh/doc-detail/126635.html)。

## DROP OPERATOR

删除一个操作符。

```
DROP OPERATOR [IF EXISTS] name ( {lefttype | NONE} , 
    {righttype | NONE} ) [CASCADE | RESTRICT]
```

更多信息请参考[DROP OPERATOR](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_OPERATOR.html)。

## DROP OWNED

删除数据库角色所拥有的数据库对象。

```
DROP OWNED BY name [, ...] [CASCADE | RESTRICT]
```

更多信息请参考[DROP OWNED](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_OWNED.html)。

## DROP RESOURCE QUEUE

删除一个资源队列。

```
DROP RESOURCE QUEUE queue_name
```

更多信息请参考[DROP RESOURCE QUEUE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_RESOURCE_QUEUE.html)。

## DROP ROLE

删除一个数据库角色。

```
DROP ROLE [IF EXISTS] name [, ...]
```

更多信息请参考[DROP ROLE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_ROLE.html)。

## DROP RULE

删除一个重写规则。

```
DROP RULE [IF EXISTS] name ON relation [CASCADE | RESTRICT]
```

更多信息请参考[DROP RULE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_RULE.html)。

## DROP SCHEMA

删除一个SCHEMA。

```
DROP SCHEMA [IF EXISTS] name [, ...] [CASCADE | RESTRICT]
```

更多信息请参考[DROP SCHEMA](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_SCHEMA.html)。

## DROP SEQUENCE

删除一个序列。

```
DROP SEQUENCE [IF EXISTS] name [, ...] [CASCADE | RESTRICT]
```

更多信息请参考[DROP SEQUENCE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_SEQUENCE.html)。

## DROP TABLE

删除一个表。

```
DROP TABLE [IF EXISTS] name [, ...] [CASCADE | RESTRICT]
```

更多信息请参考[DROP TABLE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_TABLE.html)。

## DROP TYPE

删除一个数据类型。

```
DROP TYPE [IF EXISTS] name [, ...] [CASCADE | RESTRICT]
```

更多信息请参考[DROP TYPE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_TYPE.html)。

## DROP USER

删除一个数据库角色。

```
DROP USER [IF EXISTS] name [, ...]
```

更多信息请参考[DROP USER](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_USER.html)。

## DROP VIEW

删除一个视图。

```
DROP VIEW [IF EXISTS] name [, ...] [CASCADE | RESTRICT]
```

更多信息请参考[DROP VIEW](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_VIEW.html)。

## END

提交当前事务。

```
END [WORK | TRANSACTION]
```

更多信息请参考[END](http://docs.greenplum.org/6-4/ref_guide/sql_commands/END.html)。

## EXECUTE

执行一个已经准备好的SQL语句。

```
EXECUTE name [ (parameter [, ...] ) ]
```

更多信息请参考[EXECUTE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/EXECUTE.html)。

## EXPLAIN

展示语句的查询计划。

```
EXPLAIN [ANALYZE] [VERBOSE] statement
```

更多信息请参考[EXPLAIN](http://docs.greenplum.org/6-4/ref_guide/sql_commands/EXPLAIN.html)。

## FETCH

使用游标获取查询结果的行。

```
FETCH [ forward_direction { FROM | IN } ] cursorname
```

更多信息请参考[FETCH](http://docs.greenplum.org/6-4/ref_guide/sql_commands/FETCH.html) 。

## GRANT

定义一个访问权限。

```
GRANT { {SELECT | INSERT | UPDATE | DELETE | REFERENCES | 
TRIGGER | TRUNCATE } [,...] | ALL [PRIVILEGES] }
    ON [TABLE] tablename [, ...]
    TO {rolename | PUBLIC} [, ...] [WITH GRANT OPTION]

GRANT { {USAGE | SELECT | UPDATE} [,...] | ALL [PRIVILEGES] }
    ON SEQUENCE sequencename [, ...]
    TO { rolename | PUBLIC } [, ...] [WITH GRANT OPTION]

GRANT { {CREATE | CONNECT | TEMPORARY | TEMP} [,...] | ALL 
[PRIVILEGES] }
    ON DATABASE dbname [, ...]
    TO {rolename | PUBLIC} [, ...] [WITH GRANT OPTION]

GRANT { EXECUTE | ALL [PRIVILEGES] }
    ON FUNCTION funcname ( [ [argmode] [argname] argtype [, ...] 
] ) [, ...]
    TO {rolename | PUBLIC} [, ...] [WITH GRANT OPTION]

GRANT { USAGE | ALL [PRIVILEGES] }
    ON LANGUAGE langname [, ...]
    TO {rolename | PUBLIC} [, ...] [WITH GRANT OPTION]

GRANT { {CREATE | USAGE} [,...] | ALL [PRIVILEGES] }
    ON SCHEMA schemaname [, ...]
    TO {rolename | PUBLIC} [, ...] [WITH GRANT OPTION]

GRANT { CREATE | ALL [PRIVILEGES] }
    ON TABLESPACE tablespacename [, ...]
    TO {rolename | PUBLIC} [, ...] [WITH GRANT OPTION]

GRANT parent_role [, ...] 
    TO member_role [, ...] [WITH ADMIN OPTION]

GRANT { SELECT | INSERT | ALL [PRIVILEGES] } 
    ON PROTOCOL protocolname
    TO username
```

更多信息请参考[GRANT](http://docs.greenplum.org/6-4/ref_guide/sql_commands/GRANT.html)。

## INSERT

在表中创建新的行。

```
INSERT INTO table [( column [, ...] )]
   {DEFAULT VALUES | VALUES ( {expression | DEFAULT} [, ...] ) 
   [, ...] | query}
```

更多信息请参考[INSERT](http://docs.greenplum.org/6-4/ref_guide/sql_commands/INSERT.html)。

## LOAD

加载或重新加载共享库文件。

```
LOAD 'filename'
```

更多信息请参考 [LOAD](http://docs.greenplum.org/6-4/ref_guide/sql_commands/LOAD.html)。

## LOCK

锁住一张表。

```
LOCK [TABLE] name [, ...] [IN lockmode MODE] [NOWAIT]
```

更多信息请参考[LOCK](http://docs.greenplum.org/6-4/ref_guide/sql_commands/LOCK.html)。

## MOVE

放置一个游标。

```
MOVE [ forward_direction {FROM | IN} ] cursorname
```

更多信息请参考[MOVE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/MOVE.html)。

## PREPARE

准备一个执行的语句。

```
PREPARE name [ (datatype [, ...] ) ] AS statement
```

更多信息请参考[PREPARE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/PREPARE.html)。

## REASSIGN OWNED

改变数据库角色所拥有的数据库对象的所有权。

```
REASSIGN OWNED BY old_role [, ...] TO new_role
```

更多信息请参考[REASSIGN OWNED](http://docs.greenplum.org/6-4/ref_guide/sql_commands/REASSIGN_OWNED.html)。

## REINDEX

重新构建索引。

```
REINDEX {INDEX | TABLE | DATABASE | SYSTEM} name
```

更多信息请参考[REINDEX](http://docs.greenplum.org/6-4/ref_guide/sql_commands/REINDEX.html)。

## RELEASE SAVEPOINT

销毁一个之前定义过的SAVEPOINT。

```
RELEASE [SAVEPOINT] savepoint_name
```

更多信息请参考[RELEASE SAVEPOINT](http://docs.greenplum.org/6-4/ref_guide/sql_commands/RELEASE_SAVEPOINT.html)。

## RESET

恢复系统配置参数的值为默认值。

```
RESET configuration_parameter

RESET ALL
```

更多信息请参考[RESET](http://docs.greenplum.org/6-4/ref_guide/sql_commands/RESET.html)。

## REVOKE

撤销访问权限。

```
REVOKE [GRANT OPTION FOR] { {SELECT | INSERT | UPDATE | DELETE 
       | REFERENCES | TRIGGER | TRUNCATE } [,...] | ALL [PRIVILEGES] }
       ON [TABLE] tablename [, ...]
       FROM {rolename | PUBLIC} [, ...]
       [CASCADE | RESTRICT]

REVOKE [GRANT OPTION FOR] { {USAGE | SELECT | UPDATE} [,...] 
       | ALL [PRIVILEGES] }
       ON SEQUENCE sequencename [, ...]
       FROM { rolename | PUBLIC } [, ...]
       [CASCADE | RESTRICT]

REVOKE [GRANT OPTION FOR] { {CREATE | CONNECT 
       | TEMPORARY | TEMP} [,...] | ALL [PRIVILEGES] }
       ON DATABASE dbname [, ...]
       FROM {rolename | PUBLIC} [, ...]
       [CASCADE | RESTRICT]

REVOKE [GRANT OPTION FOR] {EXECUTE | ALL [PRIVILEGES]}
       ON FUNCTION funcname ( [[argmode] [argname] argtype
                              [, ...]] ) [, ...]
       FROM {rolename | PUBLIC} [, ...]
       [CASCADE | RESTRICT]

REVOKE [GRANT OPTION FOR] {USAGE | ALL [PRIVILEGES]}
       ON LANGUAGE langname [, ...]
       FROM {rolename | PUBLIC} [, ...]
       [ CASCADE | RESTRICT ]

REVOKE [GRANT OPTION FOR] { {CREATE | USAGE} [,...] 
       | ALL [PRIVILEGES] }
       ON SCHEMA schemaname [, ...]
       FROM {rolename | PUBLIC} [, ...]
       [CASCADE | RESTRICT]

REVOKE [GRANT OPTION FOR] { CREATE | ALL [PRIVILEGES] }
       ON TABLESPACE tablespacename [, ...]
       FROM { rolename | PUBLIC } [, ...]
       [CASCADE | RESTRICT]

REVOKE [ADMIN OPTION FOR] parent_role [, ...] 
       FROM member_role [, ...]
       [CASCADE | RESTRICT]
```

更多信息请参考[REVOKE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/REVOKE.html)。

## ROLLBACK

中止当前事务。

```
ROLLBACK [WORK | TRANSACTION]
```

更多信息请参考[ROLLBACK](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ROLLBACK.html)。

## ROLLBACK TO SAVEPOINT

将当前事务回滚到某个SAVEPOINT。

```
ROLLBACK [WORK | TRANSACTION] TO [SAVEPOINT] savepoint_name
```

更多信息请参考[ROLLBACK TO SAVEPOINT](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ROLLBACK_TO_SAVEPOINT.html)。

## SAVEPOINT

在当前事务定义一个新的savepoint。

```
SAVEPOINT savepoint_name
```

更多信息请参考[SAVEPOINT](http://docs.greenplum.org/6-4/ref_guide/sql_commands/SAVEPOINT.html)。

## SELECT

从表或者视图中检索行。

```
[ WITH with_query [, ...] ]
SELECT [ALL | DISTINCT [ON (expression [, ...])]]
  * | expression [[AS] output_name] [, ...]
  [FROM from_item [, ...]]
  [WHERE condition]
  [GROUP BY grouping_element [, ...]]
  [HAVING condition [, ...]]
  [WINDOW window_name AS (window_specification)]
  [{UNION | INTERSECT | EXCEPT} [ALL] select]
  [ORDER BY expression [ASC | DESC | USING operator] [NULLS {FIRST | LAST}] [, ...]]
  [LIMIT {count | ALL}]
  [OFFSET start]
  [FOR {UPDATE | SHARE} [OF table_name [, ...]] [NOWAIT] [...]]
```

更多信息请参考[SELECT](http://docs.greenplum.org/6-4/ref_guide/sql_commands/SELECT.html)。

## SELECT INTO

从查询结果中定义一个新的表。

```
[ WITH with_query [, ...] ]
SELECT [ALL | DISTINCT [ON ( expression [, ...] )]]
    * | expression [AS output_name] [, ...]
    INTO [TEMPORARY | TEMP] [TABLE] new_table
    [FROM from_item [, ...]]
    [WHERE condition]
    [GROUP BY expression [, ...]]
    [HAVING condition [, ...]]
    [{UNION | INTERSECT | EXCEPT} [ALL] select]
    [ORDER BY expression [ASC | DESC | USING operator] [NULLS {FIRST | LAST}] [, ...]]
    [LIMIT {count | ALL}]
    [OFFSET start]
    [FOR {UPDATE | SHARE} [OF table_name [, ...]] [NOWAIT] 
    [...]]
```

更多信息请参考[SELECT INTO](http://docs.greenplum.org/6-4/ref_guide/sql_commands/SELECT_INTO.html)。

## SET

改变数据库配置参数的值。

```
SET [SESSION | LOCAL] configuration_parameter {TO | =} value | 
    'value' | DEFAULT}

SET [SESSION | LOCAL] TIME ZONE {timezone | LOCAL | DEFAULT}
```

更多信息请参考[SET](http://docs.greenplum.org/6-4/ref_guide/sql_commands/SET.html)。

## SET ROLE

设置当前会话当前角色的标识符。

```
SET [SESSION | LOCAL] ROLE rolename

SET [SESSION | LOCAL] ROLE NONE

RESET ROLE
```

更多信息请参考[SET ROLE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/SET_ROLE.html)。

## SET SESSION AUTHORIZATION

设置会话角色标识符和当前会话当前角色的标识符。

```
SET [SESSION | LOCAL] SESSION AUTHORIZATION rolename

SET [SESSION | LOCAL] SESSION AUTHORIZATION DEFAULT

RESET SESSION AUTHORIZATION
```

更多信息请参考[SET SESSION AUTHORIZATION](http://docs.greenplum.org/6-4/ref_guide/sql_commands/SET_SESSION_AUTHORIZATION.html)。

## SET TRANSACTION

设置当前事务的特征。

```
SET TRANSACTION [transaction_mode] [READ ONLY | READ WRITE]

SET SESSION CHARACTERISTICS AS TRANSACTION transaction_mode 
     [READ ONLY | READ WRITE]
```

更多信息请参考[SET TRANSACTION](http://docs.greenplum.org/6-4/ref_guide/sql_commands/SET_TRANSACTION.html)。

## SHOW

显示当前系统配置参数的值。

```
SHOW configuration_parameter

SHOW ALL
```

更多信息请参考[SHOW](http://docs.greenplum.org/6-4/ref_guide/sql_commands/SHOW.html)。

## START TRANSACTION

开始一个事务块。

```
START TRANSACTION [SERIALIZABLE | READ COMMITTED | READ UNCOMMITTED]
                  [READ WRITE | READ ONLY]
```

更多信息请参考[START TRANSACTION](http://docs.greenplum.org/6-4/ref_guide/sql_commands/START_TRANSACTION.html)。

## TRUNCATE

清空表的所有行。

```
TRUNCATE [TABLE] name [, ...] [CASCADE | RESTRICT]
```

更多信息请参考[TRUNCATE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/TRUNCATE.html)。

## UPDATE

更新表的行。

```
UPDATE [ONLY] table [[AS] alias]
   SET {column = {expression | DEFAULT} |
   (column [, ...]) = ({expression | DEFAULT} [, ...])} [, ...]
   [FROM fromlist]
   [WHERE condition | WHERE CURRENT OF cursor_name ]
```

更多信息请参考[UPDATE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/UPDATE.html)。

## VACUUM

垃圾收集和选择性分析数据库。

```
VACUUM [FULL] [FREEZE] [VERBOSE] [table]

VACUUM [FULL] [FREEZE] [VERBOSE] ANALYZE
              [table [(column [, ...] )]]
```

更多信息请参考[VACUUM](http://docs.greenplum.org/6-4/ref_guide/sql_commands/VACUUM.html)。

## VALUES

计算一组行。

```
VALUES ( expression [, ...] ) [, ...]
   [ORDER BY sort_expression [ASC | DESC | USING operator] [, ...]]
   [LIMIT {count | ALL}] [OFFSET start]
```

更多信息请参考[VALUES](http://docs.greenplum.org/6-4/ref_guide/sql_commands/VALUES.html)。

