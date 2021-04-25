# Use zhparser to perform Chinese word segmentation

This topic describes how to use zhparser to perform Chinese word segmentation in AnalyticDB for PostgreSQL. Only AnalyticDB for PostgreSQL 6.0 supports full-text search.

## Overview

By default, PostgreSQL segments words based on spaces and punctuation marks. PostreSQL does not support Chinese word segmentation. However, AnalyticDB for PostgreSQL can be integrated with the zhparser extension to support Chinese word segmentation.

The following links provide related PostreSQL official documents:

-   Full-text search overview: [https://www.postgresql.org/docs/9.4/textsearch.html](http://www.postgres.cn/docs/9.4/textsearch.html)
-   Functions and operators for full-text search: [https://www.postgresql.org/docs/9.4/functions-textsearch.html](http://postgres.cn/docs/9.4/functions-textsearch.html)

In most cases, you can use the following methods to perform full-text search:

1.  Query data in a table:

    ```
    SELECT name FROM <table...>
    WHERE to_tsvector('english', name) @@ to_tsquery('english', 'friend');
    ```

2.  Create a generalized inverted \(GIN\) index:

    ```
    CREATE INDEX <idx_...> ON <table...> USING gin(to_tsvector('english', name));
    ```


## Use zhparser

1.  Install the zhparser extension. Run the following command as a superuser:

    ```
    create extension zhparser;
    ```

2.  Configure zhparser as the Chinese word parser. To query the test search configurations, run the `\dF` or `\dFp` command. You cannot define dictionaries.

    ```
    create text search configuration zh_cn (PARSER = zhparser);
    
    --# In most cases, you can configure the following token types for zhparser: noun (n), verb (v), adjective (a), idiom (i), exclamation (e), and temporary idiom (l).
    --# You must use the built-in simple dictionary.
    alter text search configuration zh_cn add mapping for n,v,a,i,e,l with simple;
    ```

3.  Query the token types that zhparser can recognize.

    ```
    --# ts_token_types
    select ts_token_type('zhparser');
    
    ---------------------------------
              ts_token_type          
    ---------------------------------
     (97,a,"adjective,形容词")
     (98,b,"differentiation,区别词")
     (99,c,"conjunction,连词")
     (100,d,"adverb,副词")
     (101,e,"exclamation,感叹词")
     (102,f,"position,方位词")
     (103,g,"root,词根")
     (104,h,"head,前连接成分")
     (105,i,"idiom,成语")
     (106,j,"abbreviation,简称")
     (107,k,"tail,后连接成分")
     (108,l,"tmp,习用语")
     (109,m,"numeral,数词")
     (110,n,"noun,名词")
     (111,o,"onomatopoeia,拟声词")
     (112,p,"prepositional,介词")
     (113,q,"quantity,量词")
     (114,r,"pronoun,代词")
     (115,s,"space,处所词")
     (116,t,"time,时语素")
     (117,u,"auxiliary,助词")
     (118,v,"verb,动词")
     (119,w,"punctuation,标点符号")
     (120,x,"unknown,未知词")
     (121,y,"modal,语气词")
     (122,z,"status,状态词")
     
     --# Query zh_cn configurations.
     select * from pg_ts_config_map 
     where mapcfg=(select oid from pg_ts_config where cfgname='zh_cn');
    ```

4.  Add or remove token types.

    ```
    --# Add token types.
    alter text search configuration zh_cn add mapping for m,q,t with simple;
    
    --# Remove token types.
    alter text search configuration zh_cn drop mapping if exists for m,q,t;
    ```

5.  Install and configure zhparser. Then, the Chinese word segmentation feature can be used.

    ```
    select to_tsvector('zh_cn', '有两种方法进行全文检索');
    select to_tsquery('zh_cn', '有两种方法进行全文检索');
    
    --# To form compound words from multiple short words, set zhparser.multi_short to on. 1
    set zhparser.multi_short = on; 
    ```


## Examples

```
--# Create a table.
create table zh_test (id int, name text) distributed by (id);

--# Insert data into the table.
insert into zh_test values (1, '北京大学'), (2, '清华大学'), (3, '人民大学'), (4, '地质大学');

--# Create an index.
set zhparser.multi_short = on;
create index idx_test on zh_test using gin(to_tsvector('zh_cn', coalesce(name,'')));

--# Perform full-text search.
select * from zh_test 
where to_tsvector('zh_cn', coalesce(name,'')) @@ to_tsquery('zh_cn', '大学')
order by ts_rank(to_tsvector('zh_cn', coalesce(name,'')), to_tsquery('zh_cn', '大学'));


--# Create a tsvector column to simplify the Chinese word segmentation process.
alter table zh_test add column tsv tsvector;
set zhparser.multi_short = on;
update zh_test set tsv = to_tsvector('zh_cn', coalesce(name,''));
create index idx_test1 on zh_test using gin(tsv);
select * from zh_test where tsv @@ to_tsquery('zh_cn', '大学');
```

