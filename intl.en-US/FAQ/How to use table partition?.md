# How to use table partition? {#concept_g1n_nlg_v2b .concept}

We recommend that you enable table partition for fact tables or large tables in the database. Table partition simplies the deleting data and importaning data operations on a regular basis. With table partition,

-   You can use the `alter table drop partition` command to delete all the data in a partition.
-   You can use partition exchange, that is, the `alter table exchange partition` command to add a new data partition.

AnalyticDB for PostgreSQL supports range partition, list partition, and composite partition. But range partition only supports partitioning by number or time-based fields.

An example table using range partitions is shown as follows.

```


CREATE TABLE LINEITEM (
L_ORDERKEY BIGINT NOT NULL, 
L_PARTKEY BIGINT NOT NULL, 
L_SUPPKEY BIGINT NOT NULL, 
L_LINENUMBER INTEGER,
L_QUANTITY FLOAT8,
L_EXTENDEDPRICE FLOAT8,
L_DISCOUNT FLOAT8,
L_TAX FLOAT8,
L_RETURNFLAG CHAR(1),
L_LINESTATUS CHAR(1),
L_SHIPDATE DATE,
L_COMMITDATE DATE,
L_RECEIPTDATE DATE,
L_SHIPINSTRUCT CHAR(25),
L_SHIPMODE CHAR(10),
L_COMMENT VARCHAR(44)
) WITH (APPENDONLY=true, ORIENTATION=column, COMPRESSTYPE=zlib, COMPRESSLEVEL=5, BLOCKSIZE=1048576, OIDS=false) DISTRIBUTED BY (l_orderkey)
PARTITION BY RANGE (L_SHIPDATE) (START (date '1992-01-01') INCLUSIVE END (date '2000-01-01') EXCLUSIVE EVERY (INTERVAL '1 month' ));
```

