# SQL statements

This topic describes the SQL statements that are available in AnalyticDB for PostgreSQL and their syntax.

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
-   [CREATE TRIGGER](#section_slz_cs9_0bv)
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

Aborts the current transaction.

```
ABORT [WORK | TRANSACTION]
```

For more information, see [ABORT](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ABORT.html).

## ALTER AGGREGATE

Changes the definition of an aggregate function.

```
ALTER AGGREGATE name ( type [ , ... ] ) RENAME TO new_name
ALTER AGGREGATE name ( type [ , ... ] ) OWNER TO new_owner
ALTER AGGREGATE name ( type [ , ... ] ) SET SCHEMA new_schema
```

For more information, see [ALTER AGGREGATE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_AGGREGATE.html).

## ALTER CONVERSION

Changes the definition of a conversion.

```
ALTER CONVERSION name RENAME TO newname
ALTER CONVERSION name OWNER TO newowner
```

For more information, see [ALTER CONVERSION](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_CONVERSION.html).

## ALTER DATABASE

Changes the attributes of a database.

```
ALTER DATABASE name [ WITH CONNECTION LIMIT connlimit ]
ALTER DATABASE name SET parameter { TO | = } { value | DEFAULT }
ALTER DATABASE name RESET parameter
ALTER DATABASE name RENAME TO newname
ALTER DATABASE name OWNER TO new_owner
```

For more information, see [ALTER DATABASE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_DATABASE.html).

## ALTER DOMAIN

Changes the definition of a domain.

```
ALTER DOMAIN name { SET DEFAULT expression | DROP DEFAULT }
ALTER DOMAIN name { SET | DROP } NOT NULL
ALTER DOMAIN name ADD domain_constraint
ALTER DOMAIN name DROP CONSTRAINT constraint_name [RESTRICT | CASCADE]
ALTER DOMAIN name OWNER TO new_owner
ALTER DOMAIN name SET SCHEMA new_schema
```

For more information, see [ALTER DOMAIN](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_DOMAIN.html).

## ALTER EXTERNAL TABLE

Changes the definition of an external table.

```
ALTER EXTERNAL TABLE name RENAME [COLUMN] column TO new_column
ALTER EXTERNAL TABLE name RENAME TO new_name
ALTER EXTERNAL TABLE name SET SCHEMA new_schema
ALTER EXTERNAL TABLE name action [, ... ]
```

For more information, see [ALTER EXTERNAL TABLE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_EXTERNAL_TABLE.html).

## ALTER FUNCTION

Changes the definition of a function.

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

For more information, see [ALTER FUNCTION](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_FUNCTION.html).

## ALTER GROUP

Changes a role name or membership.

```
ALTER GROUP groupname ADD USER username [, ... ]
ALTER GROUP groupname DROP USER username [, ... ]
ALTER GROUP groupname RENAME TO newname
```

For more information, see [ALTER GROUP](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_GROUP.html).

## ALTER INDEX

Changes the definition of an index.

```
ALTER INDEX name RENAME TO new_name
ALTER INDEX name SET TABLESPACE tablespace_name
ALTER INDEX name SET ( FILLFACTOR = value )
ALTER INDEX name RESET ( FILLFACTOR )
```

For more information, see [ALTER INDEX](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_INDEX.html).

## ALTER OPERATOR

Changes the definition of an operator.

```
ALTER OPERATOR name ( {lefttype | NONE} , {righttype | NONE} ) 
   OWNER TO newowner
```

For more information, see [ALTER OPERATOR](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_OPERATOR.html).

## ALTER RESOURCE QUEUE

Changes the limits of a resource queue.

```
ALTER RESOURCE QUEUE name WITH ( queue_attribute=value [, ... ] )
```

For more information, see [ALTER RESOURCE QUEUE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_RESOURCE_QUEUE.html).

## ALTER ROLE

Changes a database role \(user or group\).

```
ALTER ROLE name RENAME TO newname

ALTER ROLE name SET config_parameter {TO | =} {value | DEFAULT}

ALTER ROLE name RESET config_parameter

ALTER ROLE name RESOURCE QUEUE {queue_name | NONE}

ALTER ROLE name [ [WITH] option [ ... ] ]
```

For more information, see [ALTER ROLE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_ROLE.html).

## ALTER SCHEMA

Changes the definition of a schema.

```
ALTER SCHEMA name RENAME TO newname

ALTER SCHEMA name OWNER TO newowner
```

For more information, see [ALTER SCHEMA](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_SCHEMA.html).

## ALTER SEQUENCE

Changes the definition of a sequence generator.

```
ALTER SEQUENCE name [INCREMENT [ BY ] increment] 
     [MINVALUE minvalue | NO MINVALUE] 
     [MAXVALUE maxvalue | NO MAXVALUE] 
     [RESTART [ WITH ] start] 
     [CACHE cache] [[ NO ] CYCLE] 
     [OWNED BY {table.column | NONE}]
ALTER SEQUENCE name SET SCHEMA new_schema
```

For more information, see [ALTER SEQUENCE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_SEQUENCE.html).

## ALTER TABLE

Changes the definition of a table.

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

For more information, see [ALTER TABLE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_TABLE.html).

## ALTER TYPE

Changes the definition of a data type.

```
ALTER TYPE name
   OWNER TO new_owner | SET SCHEMA new_schema
```

For more information, see [ALTER TYPE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_TYPE.html).

## ALTER USER

Changes the definition of a database role \(user\).

```
ALTER USER name RENAME TO newname

ALTER USER name SET config_parameter {TO | =} {value | DEFAULT}

ALTER USER name RESET config_parameter

ALTER USER name [ [WITH] option [ ... ] ]
```

For more information, see [ALTER USER](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ALTER_USER.html).

## ANALYZE

Collects statistics about a database.

```
ANALYZE [VERBOSE] [ROOTPARTITION [ALL] ] 
   [table [ (column [, ...] ) ]]
```

For more information, see [ANALYZE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ANALYZE.html).

## BEGIN

Starts a transaction block.

```
BEGIN [WORK | TRANSACTION] [transaction_mode]
      [READ ONLY | READ WRITE]
```

For more information, see [BEGIN](http://docs.greenplum.org/6-4/ref_guide/sql_commands/BEGIN.html).

## CHECKPOINT

Forces a transaction log checkpoint.

```
CHECKPOINT
```

For more information, see [CHECKPOINT](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CHECKPOINT.html).

## CLOSE

Closes a cursor.

```
CLOSE cursor_name
```

For more information, see [CLOSE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CLOSE.html).

## CLUSTER

Physically reorders heap storage tables on a disk based on an index. We recommend that you do not use this statement.

```
CLUSTER indexname ON tablename

CLUSTER tablename

CLUSTER
```

For more information, see [CLUSTER](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CLUSTER.html).

## COMMENT

Defines or changes the comments of an object.

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

For more information, see [COMMENT](http://docs.greenplum.org/6-4/ref_guide/sql_commands/COMMENT.html).

## COMMIT

Commits the current transaction.

```
COMMIT [WORK | TRANSACTION]
```

For more information, see [COMMIT](http://docs.greenplum.org/6-4/ref_guide/sql_commands/COMMIT.html).

## COPY

Copies data between a file and a table.

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

For more information, see [COPY](http://docs.greenplum.org/6-4/ref_guide/sql_commands/COPY.html).

## CREATE AGGREGATE

Defines an aggregate function.

```
CREATE [ORDERED] AGGREGATE name (input_data_type [ , ... ]) 
      ( SFUNC = sfunc,
        STYPE = state_data_type
        [, PREFUNC = prefunc]
        [, FINALFUNC = ffunc]
        [, INITCOND = initial_condition]
        [, SORTOP = sort_operator] )
```

For more information, see [CREATE AGGREGATE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_AGGREGATE.html).

## CREATE CAST

Defines a cast.

```
CREATE CAST (sourcetype AS targettype) 
       WITH FUNCTION funcname (argtypes) 
       [AS ASSIGNMENT | AS IMPLICIT]

CREATE CAST (sourcetype AS targettype) WITHOUT FUNCTION 
       [AS ASSIGNMENT | AS IMPLICIT]
```

For more information, see [CREATE CAST](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_CAST.html).

## CREATE CONVERSION

Defines an encoding conversion.

```
CREATE [DEFAULT] CONVERSION name FOR source_encoding TO 
     dest_encoding FROM funcname
```

For more information, see [CREATE CONVERSION](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_CONVERSION.html).

## CREATE DATABASE

Creates a database.

```
CREATE DATABASE name [ [WITH] [OWNER [=] dbowner]
                     [TEMPLATE [=] template]
                     [ENCODING [=] encoding]
                     [CONNECTION LIMIT [=] connlimit ] ]
```

For more information, see [CREATE DATABASE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_DATABASE.html).

## CREATE DOMAIN

Defines a domain.

```
CREATE DOMAIN name [AS] data_type [DEFAULT expression] 
       [CONSTRAINT constraint_name
       | NOT NULL | NULL 
       | CHECK (expression) [...]]
```

For more information, see [CREATE DOMAIN](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_DOMAIN.html).

## CREATE EXTENSION

Registers an extension in a database.

```
CREATE EXTENSION [ IF NOT EXISTS ] extension_name
  [ WITH ] [ SCHEMA schema_name ]
           [ VERSION version ]
           [ FROM old_version ]
           [ CASCADE ]
```

For more information, see [CREATE EXTENSION](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_EXTENSION.html).

## CREATE EXTERNAL TABLE

Defines an external table.

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

For more information, see [CREATE EXTERNAL TABLE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_EXTERNAL_TABLE.html).

## CREATE FUNCTION

Defines a function.

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

For more information, see [CREATE FUNCTION](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_FUNCTION.html).

## CREATE GROUP

Defines a database role.

```
CREATE GROUP name [ [WITH] option [ ... ] ]
```

For more information, see [CREATE GROUP](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_GROUP.html).

## CREATE INDEX

Defines an index.

```
CREATE [UNIQUE] INDEX name ON table
       [USING btree|bitmap|gist]
       ( {column | (expression)} [opclass] [, ...] )
       [ WITH ( FILLFACTOR = value ) ]
       [TABLESPACE tablespace]
       [WHERE predicate]
```

For more information, see [CREATE INDEX](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_INDEX.html).

## CREATE LIBRARY

Defines a custom library package.

```
CREATE LIBRARY library_name LANGUAGE [JAVA] FROM oss_location OWNER ownername
CREATE LIBRARY library_name LANGUAGE [JAVA] VALUES file_content_hex OWNER ownername
```

For more information, see [CREATE LIBRARY](https://www.alibabacloud.com/help/zh/doc-detail/126635.html).

## CREATE OPERATOR

Defines an operator.

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

For more information, see [CREATE OPERATOR](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_OPERATOR.html).

## CREATE RESOURCE QUEUE

Defines a resource queue.

```
CREATE RESOURCE QUEUE name WITH (queue_attribute=value [, ... ])
```

For more information, see [CREATE RESOURCE QUEUE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_RESOURCE_QUEUE.html).

## CREATE ROLE

Defines a database role \(user or group\).

```
CREATE ROLE name [[WITH] option [ ... ]]
```

For more information, see [CREATE ROLE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_ROLE.html).

## CREATE RULE

Defines a rewrite rule.

```
CREATE [OR REPLACE] RULE name AS ON event
  TO table [WHERE condition] 
  DO [ALSO | INSTEAD] { NOTHING | command | (command; command 
  ...) }
```

For more information, see [CREATE RULE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_RULE.html).

## CREATE SCHEMA

Defines a schema.

```
CREATE SCHEMA schema_name [AUTHORIZATION username] 
   [schema_element [ ... ]]

CREATE SCHEMA AUTHORIZATION rolename [schema_element [ ... ]]
```

For more information, see [CREATE SCHEMA](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_SCHEMA.html).

## CREATE SEQUENCE

Defines a sequence generator.

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

For more information, see [CREATE SEQUENCE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_SEQUENCE.html).

## CREATE TABLE

Defines a table.

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

For more information, see [CREATE TABLE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_TABLE.html).

## CREATE TABLE AS

Defines a table from the results of a query.

```
CREATE [ [GLOBAL | LOCAL] {TEMPORARY | TEMP} ] TABLE table_name
   [(column_name [, ...] )]
   [ WITH ( storage_parameter=value [, ... ] ) ]
   [ON COMMIT {PRESERVE ROWS | DELETE ROWS | DROP}]
   [TABLESPACE tablespace]
   AS query
   [DISTRIBUTED BY (column, [ ... ] ) | DISTRIBUTED RANDOMLY]
```

For more information, see [CREATE TABLE AS](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_TABLE_AS.html).

## CREATE TRIGGER

Defines a trigger.

```
CREATE TRIGGER name {BEFORE | AFTER} {event [OR ...]}
       ON table [ FOR [EACH] {ROW | STATEMENT} ]
       EXECUTE PROCEDURE funcname ( arguments )
```

For more information, see [CREATE TRIGGER](https://docs.greenplum.org/5280/ref_guide/sql_commands/CREATE_TRIGGER.html).

## CREATE TYPE

Defines a data type.

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

For more information, see [CREATE TYPE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_TYPE.html).

## CREATE USER

Defines a database role that has the LOGIN permission.

```
CREATE USER name [ [WITH] option [ ... ] ]
```

For more information, see [CREATE USER](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_USER.html).

## CREATE VIEW

Defines a view.

```
CREATE [OR REPLACE] [TEMP | TEMPORARY] VIEW name
       [ ( column_name [, ...] ) ]
       AS query
```

For more information, see [CREATE VIEW](http://docs.greenplum.org/6-4/ref_guide/sql_commands/CREATE_VIEW.html).

## DEALLOCATE

Deallocates a prepared statement.

```
DEALLOCATE [PREPARE] name
```

For more information, see [DEALLOCATE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DEALLOCATE.html).

## DECLARE

Defines a cursor.

```
DECLARE name [BINARY] [INSENSITIVE] [NO SCROLL] CURSOR 
     [{WITH | WITHOUT} HOLD] 
     FOR query [FOR READ ONLY]
```

For more information, see [DECLARE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DECLARE.html).

## DELETE

Deletes rows from a table.

```
DELETE FROM [ONLY] table [[AS] alias]
      [USING usinglist]
      [WHERE condition | WHERE CURRENT OF cursor_name ]
```

For more information, see [DELETE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DELETE.html).

## DROP AGGREGATE

Deletes an aggregate function.

```
DROP AGGREGATE [IF EXISTS] name ( type [, ...] ) [CASCADE | RESTRICT]
```

For more information, see [DROP AGGREGATE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_AGGREGATE.html).

## DROP CAST

Deletes a cast.

```
DROP CAST [IF EXISTS] (sourcetype AS targettype) [CASCADE | RESTRICT]
```

For more information, see [DROP CAST](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_CAST.html).

## DROP CONVERSION

Deletes a conversion.

```
DROP CONVERSION [IF EXISTS] name [CASCADE | RESTRICT]
```

For more information, see [DROP CONVERSION](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_CONVERSION.html).

## DROP DATABASE

Deletes a database.

```
DROP DATABASE [IF EXISTS] name
```

For more information, see [DROP DATABASE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_DATABASE.html).

## DROP DOMAIN

Deletes a domain.

```
DROP DOMAIN [IF EXISTS] name [, ...]  [CASCADE | RESTRICT]
```

For more information, see [DROP DOMAIN](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_DOMAIN.html).

## DROP EXTENSION

Deletes an extension from a database.

```
DROP EXTENSION [ IF EXISTS ] name [, ...] [ CASCADE | RESTRICT ]
```

For more information, see [DROP EXTENSION](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_EXTENSION.html).

## DROP EXTERNAL TABLE

Deletes the definition of an external table.

```
DROP EXTERNAL [WEB] TABLE [IF EXISTS] name [CASCADE | RESTRICT]
```

For more information, see [DROP EXTERNAL TABLE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_EXTERNAL_TABLE.html).

## DROP FUNCTION

Deletes a function.

```
DROP FUNCTION [IF EXISTS] name ( [ [argmode] [argname] argtype 
    [, ...] ] ) [CASCADE | RESTRICT]
```

For more information, see [DROP FUNCTION](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_FUNCTION.html).

## DROP GROUP

Deletes a database role.

```
DROP GROUP [IF EXISTS] name [, ...]
```

For more information, see [DROP GROUP](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_GROUP.html).

## DROP INDEX

Deletes an index.

```
DROP INDEX [IF EXISTS] name [, ...] [CASCADE | RESTRICT]
```

For more information, see [DROP INDEX](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_INDEX.html).

## DROP LIBRARY

Deletes a custom library package.

```
DROP LIBRARY library_name
```

For more information, see [DROP LIBRARY](https://www.alibabacloud.com/help/zh/doc-detail/126635.html).

## DROP OPERATOR

Deletes an operator.

```
DROP OPERATOR [IF EXISTS] name ( {lefttype | NONE} , 
    {righttype | NONE} ) [CASCADE | RESTRICT]
```

For more information, see [DROP OPERATOR](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_OPERATOR.html).

## DROP OWNED

Deletes database objects owned by a database role.

```
DROP OWNED BY name [, ...] [CASCADE | RESTRICT]
```

For more information, see [DROP OWNED](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_OWNED.html).

## DROP RESOURCE QUEUE

Deletes a resource queue.

```
DROP RESOURCE QUEUE queue_name
```

For more information, see [DROP RESOURCE QUEUE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_RESOURCE_QUEUE.html).

## DROP ROLE

Deletes a database role.

```
DROP ROLE [IF EXISTS] name [, ...]
```

For more information, see [DROP ROLE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_ROLE.html).

## DROP RULE

Deletes a rewrite rule.

```
DROP RULE [IF EXISTS] name ON relation [CASCADE | RESTRICT]
```

For more information, see [DROP RULE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_RULE.html).

## DROP SCHEMA

Deletes a schema.

```
DROP SCHEMA [IF EXISTS] name [, ...] [CASCADE | RESTRICT]
```

For more information, see [DROP SCHEMA](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_SCHEMA.html).

## DROP SEQUENCE

Deletes a sequence.

```
DROP SEQUENCE [IF EXISTS] name [, ...] [CASCADE | RESTRICT]
```

For more information, see [DROP SEQUENCE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_SEQUENCE.html).

## DROP TABLE

Deletes a table.

```
DROP TABLE [IF EXISTS] name [, ...] [CASCADE | RESTRICT]
```

For more information, see [DROP TABLE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_TABLE.html).

## DROP TYPE

Deletes a data type.

```
DROP TYPE [IF EXISTS] name [, ...] [CASCADE | RESTRICT]
```

For more information, see [DROP TYPE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_TYPE.html).

## DROP USER

Deletes a database role.

```
DROP USER [IF EXISTS] name [, ...]
```

For more information, see [DROP USER](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_USER.html).

## DROP VIEW

Deletes a view.

```
DROP VIEW [IF EXISTS] name [, ...] [CASCADE | RESTRICT]
```

For more information, see [DROP VIEW](http://docs.greenplum.org/6-4/ref_guide/sql_commands/DROP_VIEW.html).

## END

Commits the current transaction.

```
END [WORK | TRANSACTION]
```

For more information, see [END](http://docs.greenplum.org/6-4/ref_guide/sql_commands/END.html).

## EXECUTE

Executes a prepared SQL statement.

```
EXECUTE name [ (parameter [, ...] ) ]
```

For more information, see [EXECUTE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/EXECUTE.html).

## EXPLAIN

Shows the query plan of a statement.

```
EXPLAIN [ANALYZE] [VERBOSE] statement
```

For more information, see [EXPLAIN](http://docs.greenplum.org/6-4/ref_guide/sql_commands/EXPLAIN.html).

## FETCH

Retrieves rows from a query by using a cursor.

```
FETCH [ forward_direction { FROM | IN } ] cursorname
```

For more information, see [FETCH](http://docs.greenplum.org/6-4/ref_guide/sql_commands/FETCH.html).

## GRANT

Grants access permissions.

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

For more information, see [GRANT](http://docs.greenplum.org/6-4/ref_guide/sql_commands/GRANT.html).

## INSERT

Creates rows in a table.

```
INSERT INTO table [( column [, ...] )]
   {DEFAULT VALUES | VALUES ( {expression | DEFAULT} [, ...] ) 
   [, ...] | query}
```

For more information, see [INSERT](http://docs.greenplum.org/6-4/ref_guide/sql_commands/INSERT.html).

## LOAD

Loads or reloads a shared library file.

```
LOAD 'filename'
```

For more information, see [LOAD](http://docs.greenplum.org/6-4/ref_guide/sql_commands/LOAD.html).

## LOCK

Locks a table.

```
LOCK [TABLE] name [, ...] [IN lockmode MODE] [NOWAIT]
```

For more information, see [LOCK](http://docs.greenplum.org/6-4/ref_guide/sql_commands/LOCK.html).

## MOVE

Positions a cursor.

```
MOVE [ forward_direction {FROM | IN} ] cursorname
```

For more information, see [MOVE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/MOVE.html).

## PREPARE

Prepares a statement for execution.

```
PREPARE name [ (datatype [, ...] ) ] AS statement
```

For more information, see [PREPARE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/PREPARE.html).

## REASSIGN OWNED

Changes the ownership of database objects owned by a database role.

```
REASSIGN OWNED BY old_role [, ...] TO new_role
```

For more information, see [REASSIGN OWNED](http://docs.greenplum.org/6-4/ref_guide/sql_commands/REASSIGN_OWNED.html).

## REINDEX

Rebuilds an index.

```
REINDEX {INDEX | TABLE | DATABASE | SYSTEM} name
```

For more information, see [REINDEX](http://docs.greenplum.org/6-4/ref_guide/sql_commands/REINDEX.html).

## RELEASE SAVEPOINT

Destroys a defined savepoint.

```
RELEASE [SAVEPOINT] savepoint_name
```

For more information, see [RELEASE SAVEPOINT](http://docs.greenplum.org/6-4/ref_guide/sql_commands/RELEASE_SAVEPOINT.html).

## RESET

Restores the value of a system configuration parameter to the default value.

```
RESET configuration_parameter

RESET ALL
```

For more information, see [RESET](http://docs.greenplum.org/6-4/ref_guide/sql_commands/RESET.html).

## REVOKE

Revokes access permissions.

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

For more information, see [REVOKE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/REVOKE.html).

## ROLLBACK

Aborts the current transaction.

```
ROLLBACK [WORK | TRANSACTION]
```

For more information, see [ROLLBACK](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ROLLBACK.html).

## ROLLBACK TO SAVEPOINT

Rolls back the current transaction to a savepoint.

```
ROLLBACK [WORK | TRANSACTION] TO [SAVEPOINT] savepoint_name
```

For more information, see [ROLLBACK TO SAVEPOINT](http://docs.greenplum.org/6-4/ref_guide/sql_commands/ROLLBACK_TO_SAVEPOINT.html).

## SAVEPOINT

Defines a savepoint within the current transaction.

```
SAVEPOINT savepoint_name
```

For more information, see [SAVEPOINT](http://docs.greenplum.org/6-4/ref_guide/sql_commands/SAVEPOINT.html).

## SELECT

Retrieves rows from a table or view.

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

For more information, see [SELECT](http://docs.greenplum.org/6-4/ref_guide/sql_commands/SELECT.html).

## SELECT INTO

Defines a table from the results of a query.

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

For more information, see [SELECT INTO](http://docs.greenplum.org/6-4/ref_guide/sql_commands/SELECT_INTO.html).

## SET

Changes the value of a database configuration parameter.

```
SET [SESSION | LOCAL] configuration_parameter {TO | =} value | 
    'value' | DEFAULT}

SET [SESSION | LOCAL] TIME ZONE {timezone | LOCAL | DEFAULT}
```

For more information, see [SET](http://docs.greenplum.org/6-4/ref_guide/sql_commands/SET.html).

## SET ROLE

Sets the current role identifier of the current session.

```
SET [SESSION | LOCAL] ROLE rolename

SET [SESSION | LOCAL] ROLE NONE

RESET ROLE
```

For more information, see [SET ROLE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/SET_ROLE.html).

## SET SESSION AUTHORIZATION

Sets the session role identifier and the current role identifier of the current session.

```
SET [SESSION | LOCAL] SESSION AUTHORIZATION rolename

SET [SESSION | LOCAL] SESSION AUTHORIZATION DEFAULT

RESET SESSION AUTHORIZATION
```

For more information, see [SET SESSION AUTHORIZATION](http://docs.greenplum.org/6-4/ref_guide/sql_commands/SET_SESSION_AUTHORIZATION.html).

## SET TRANSACTION

Sets the characteristics of the current transaction.

```
SET TRANSACTION [transaction_mode] [READ ONLY | READ WRITE]

SET SESSION CHARACTERISTICS AS TRANSACTION transaction_mode 
     [READ ONLY | READ WRITE]
```

For more information, see [SET TRANSACTION](http://docs.greenplum.org/6-4/ref_guide/sql_commands/SET_TRANSACTION.html).

## SHOW

Shows the value of a system configuration parameter.

```
SHOW configuration_parameter

SHOW ALL
```

For more information, see [SHOW](http://docs.greenplum.org/6-4/ref_guide/sql_commands/SHOW.html).

## START TRANSACTION

Starts a transaction block.

```
START TRANSACTION [SERIALIZABLE | READ COMMITTED | READ UNCOMMITTED]
                  [READ WRITE | READ ONLY]
```

For more information, see [START TRANSACTION](http://docs.greenplum.org/6-4/ref_guide/sql_commands/START_TRANSACTION.html).

## TRUNCATE

Clears all rows of a table.

```
TRUNCATE [TABLE] name [, ...] [CASCADE | RESTRICT]
```

For more information, see [TRUNCATE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/TRUNCATE.html).

## UPDATE

Updates rows of a table.

```
UPDATE [ONLY] table [[AS] alias]
   SET {column = {expression | DEFAULT} |
   (column [, ...]) = ({expression | DEFAULT} [, ...])} [, ...]
   [FROM fromlist]
   [WHERE condition | WHERE CURRENT OF cursor_name ]
```

For more information, see [UPDATE](http://docs.greenplum.org/6-4/ref_guide/sql_commands/UPDATE.html).

## VACUUM

Collects garbage and optionally analyzes a database.

```
VACUUM [FULL] [FREEZE] [VERBOSE] [table]

VACUUM [FULL] [FREEZE] [VERBOSE] ANALYZE
              [table [(column [, ...] )]]
```

For more information, see [VACUUM](http://docs.greenplum.org/6-4/ref_guide/sql_commands/VACUUM.html).

## VALUES

Computes a set of rows.

```
VALUES ( expression [, ...] ) [, ...]
   [ORDER BY sort_expression [ASC | DESC | USING operator] [, ...]]
   [LIMIT {count | ALL}] [OFFSET start]
```

For more information, see [VALUES](http://docs.greenplum.org/6-4/ref_guide/sql_commands/VALUES.html).

