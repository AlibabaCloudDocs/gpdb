# 基于Client SDK数据写入

AnalyticDB PostgreSQL版Client SDK旨在通过 API 方式提供高性能COPY数据到AnalyticDB PostgreSQL版的方式。

AnalyticDB PostgreSQL版Client SDK通过 API 形式旨在为用户提供高性能写入数据到AnalyticDB PostgreSQL版的方式，支持用户定制化开发或对接写入程序。通过 SDK 开发写入程序，可简化在AnalyticDB PostgreSQL版中写入数据的流程，无需担心连接池、缓存等问题，相比较直接COPY/INSERT写入，通过并行化等内部机制有几倍性能提升

**说明：** AnalyticDB PostgreSQL版Client SDK主要职责是将您传入的数据高效地写入，不负责原始数据的读取、处理等工作。

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1686072851/p52757.jpg)

## Maven repositories

您可以通过Maven管理配置新SDK的版本。Maven的配置信息如下：

```
<dependency>
  <groupId>com.alibaba.cloud.analyticdb</groupId>
  <artifactId>adb4pgclient</artifactId>
  <version>1.0.0</version>
</dependency>
```

**说明：**

-   AnalyticDB PostgreSQL版Client SDK本身依赖druid\(1.1.17\)、postgresql\(jdbc 42.2.5\)、commons-lang3\(3.4\)、slf4j-api\(1.7.24\)、slf4j-log4j12\(1.7.24\)。
-   如果在使用过程中出现版本冲突，请检查这几个包的版本并解决冲突。

## 接口列表

|接口名|描述|
|---|--|
|setHost\(String adbHost\)|需要连接的AnalyticDB PostgreSQL版的连接地址。|
|setPort\(int port\)|需要连接的AnalyticDB PostgreSQL版的端口，默认为3432。|
|setDatabase\(String database\)|需要连接的AnalyticDB PostgreSQL版数据库名称。|
|setUser\(String username\)|需要连接的AnalyticDB PostgreSQL版使用的用户名。|
|setPassword\(String pwd\)|设置连接的AnalyticDB PostgreSQL版使用的密码。|
|addTable\(List<String\> table, String schema\)|需要写入的表名List，请按照表所属schema分别添加。该方法可调用多次，但在使用DatabaseConfig构造Adb4PGClient对象之后再调用不再生效。|
|setColumns\(List<String\> columns, String tableName, String schemaName\)|需要插入表的字段名（若是全字段插入，`columnList.add("*")`即可, table列表中的所有表都需要设置字段名，不然检查不会通过|
|setInsertIgnore\(boolean insertIgnore\)|设置是否忽略发生主键冲突错误的数据行，要根据业务的使用场景进行判断，针对配置的所有表，默认为true|
|setEmptyAsNull\(boolean emptyAsNull\)|设置empty数据设置为null，默认false，针对配置的所有表|
|setParallelNumber\(int parallelNumber\)|设置写入ADB PG版时的并发线程数，默认4，针对配置的所有表，一般情况不建议修改|
|setLogger\(Logger logger\)|设置client中使用的logger对象，此处使用slf4j.Logger|
|setRetryTimes\(int retryTimes\)|设置commit时，写入ADB PG版出现异常时重试的次数，默认为3|
|setRetryIntervalTime\(long retryIntervalTime\)|设置重试间隔的时间，单位是ms，默认为 1000 ms|
|setCommitSize\(long commitSize\)|设置自动提交的数据量（单位Byte），默认为10MB，一般不建议设置|

|接口名称|描述|
|----|--|
|setColumn\(int index, Object value\)|设置Row字段列表的值，要求必须按照字段的顺序（此种方式，Row实例不可复用，每条数据必须单独的Row实例）|
|setColumnValues\(List<Object\> values\)|直接将List格式数据行写入Row中|
|updateColumn\(int index, Object value\)|更新Row字段列表的值，注意更新的字段数据（此方法，Row实例可以复用，只需更新Row实例中的数据即可|

|接口名称|描述|
|----|--|
|addRow\(Row row, String tableName, String schemaName\) / addRows\(List<Row\> rows, String tableName, String schemaName\)|插入对应表的Row格式化的数据，即一条记录，数据会存储在SDK的缓冲区中，等待commit。如果数据量超过commitSize会在addRow/addRows的时候做一次自动commit，然后将最新的数据add进来；如果在自动commit失败的时候失败，调用方需要处理此异常，并且会在异常中得到失败的数据list|
|addMap\(Map<String, String\> dataMap,String tableName, String schemaName\) / addMaps\(List<Map<String, String\>\> dataMaps, String tableName, String schemaName\)|对应于addRow，支持map格式数据的写入，如果数据量满了会在addMap/addMaps的时候做一次自动commit，然后将最新的数据add进来；如果在自动commit失败的时候失败，调用方需要处理此异常，并且会在异常中得到失败的数据list|
|commit\(\)|将缓存的数据进行提交，写入ADB PG版中，若commit失败，会把执行错误的语句放在异常中抛出，调用方需要对此异常进行处理|
|TableInfo getTableInfo\(String tableName, String schemaName\)|获取对应table的结构信息|
|List<ColumnInfo\> getColumnInfo\(String tableName, String schemaName\)|获取对应table的字段列表信息，字段类是ColumnInfo，可以通过columnInfo.isNullable\(\)获取该字段是否能为null|
|stop\(\)|实例使用完之后，stop释放内部线程池及资源，如果内存中有数据未commit，则会抛Exception，若需要强行stop，请使用forceStop\(\)|
|forceStop\(\)|强行释放内部线程池及资源，会丢失掉缓存在内存中未commit的数据，一般不推荐使用|
|Connection getConnection\(\) throws SQLException|从client连接池获取ADB PG Connection连接，调用方可以使用获得的Connection做非copy操作，使用方式和jdbc的连接使用方式一致。 **说明：** 使用结束后一定要释放掉相应的资源（如ResultSet、Statement、Connection） |

|接口名称|描述|
|----|--|
|boolean isNullable\(\)|判断该字段是否能为null|

|错误码名|错误码值|描述|
|----|----|--|
|COMMIT\_ERROR\_DATA\_LIST|101|commit中某些数据出现异常，会返回异常的数据。 **说明：** 通过e.getErrData\(\)即可获得异常数据List<String\>，此错误码在addMap\(s\)、addRow\(s\)、commit操作的时候都可能会发生，因此在这些操作的时候需要单独处理此错误码的异常 |
|COMMIT\_ERROR\_OTHER|102|commit中的其他异常|
|ADD\_DATA\_ERROR|103|add数据过程中出现的异常|
|CREATE\_CONNECTION\_ERROR|104|创建连接出现异常|
|CLOSE\_CONNECTION\_ERROR|105|关闭连接出现异常|
|CONFIG\_ERROR|106|配置DatabaseConfig出现配置错误|
|STOP\_ERROR|107|停止实例时的报错|
|OTHER|999|默认异常错误码|

## 代码示例

```
public class Adb4pgClientUsage {
    public void demo() {
        DatabaseConfig databaseConfig = new DatabaseConfig();
        // Should set your database real host or url
        databaseConfig.setHost("100.100.100.100");
        // Should set your database real port
        databaseConfig.setPort(8888);
        // 连接数据库的用户名
        databaseConfig.setUser("your user name");
        // 连接数据库的密码
        databaseConfig.setPassword("your password");
      // 需要连接的database
        databaseConfig.setDatabase("your database name");
        // 设置需要写入的表名列表
        List<String> tables = new ArrayList<String>();
        tables.add("your table name 1");
        tables.add("your table name 2");

        // 不同schema下的表可分别addTable，但是一旦使用databseconfig 创造Client实例之后，table配置是不可修改的/
        // schema传入null, 则默认schema为public
        databaseConfig.addTable(tables, "table schema name");

        // 设置需要写入的表字段
        List<String> columns = new ArrayList<String>();
        columns.add("column1");
        columns.add("column2");
        // 如果是所有字段，字段列表使用 columns.add("*") 即可
        databaseConfig.setColumns(columns, "your table name 1", "table schema name");
        databaseConfig.setColumns(Collections.singletonList("*"),"your table name 2", "table schema name");


        // If the value of column is empty, set null
        databaseConfig.setEmptyAsNull(false);
        // 使用insert ignore into方式进行插入
        databaseConfig.setInsertIgnore(true);
        // commit时，写入ADB出现异常时重试的3次
        databaseConfig.setRetryTimes(3);
        // 重试间隔的时间为1s，单位是ms
        databaseConfig.setRetryIntervalTime(1000);
        // Initialize AdbClient，初始化实例之后，databaseConfig的配置信息不能再修改
        Adb4pgClient adbClient = new Adb4pgClient(databaseConfig);

        // 数据需要攒批，多次add，再commit，具体攒批数量见"注意事项"
        for (int i = 0; i < 10; i++) {
            // Add row(s) to buffer. One instance for one record
            Row row = new Row(columns.size());
            // Set column
            // the column index must be same as the sequence of columns
            // the column value can be any type, internally it will be formatted according to column type
            row.setColumn(0, i); // Number value
            row.setColumn(1, "string value"); // String value
            // 如果sql长度满了会在addRow或者addMap的时候会进行一次自动提交
            // 如果提交失败会返回AdbClientException异常，错误码为COMMIT_ERROR_DATA_LIST
            adbClient.addRow(row, "your table name 1", "table schema name");
        }

        Row row = new Row();
        row.setColumn(0, 10); // Number value
        row.setColumn(1, "2018-01-01 08:00:00"); // Date/Timestamp/Time value
        adbClient.addRow(row, "your table name 1", "table schema name");
        // Update column. Row实例可复用
        row.updateColumn(0, 11);
        row.updateColumn(1, "2018-01-02 08:00:00");
        adbClient.addRow(row, "your table name 1", "table schema name");

        // Add map(s) to buffer
        Map<String, String> rowMap = new HashMap<String, String>();
        rowMap.put("t1", "12");
        rowMap.put("t2", "string value");
        // 这边需要攒批的，最好多次add之后在进行commit
        adbClient.addMap(rowMap, "your table name 2", "table schema name");

        // Commit buffer to ADS
        // Buffer is cleaned after successfully commit to ADS
        try {
            adbClient.commit();
        } catch (Exception e) {
            // TODO: Handle exception here
        } finally {
            adbClient.stop();
        }
    }

}
```

## 注意事项

-   ADB PG 版Client SDK是非线程安全的，所以如果多线程调用的情况，需要每个线程维护自己的Client对象

    **说明：** 强烈不建议多线程共用SDK实例，除了线程安全问题外，容易让Client成为写入性能的瓶颈。

-   数据必须在调用commit成功后才能认为是写入ADB PG版成功的。
-   针对Client抛出的异常，调用方要根据错误码的意义自行判断如何处理，如果是数据写入有问题，可以重复提交或者记录下有问题的数据后跳过。
-   很多时候写入线程并不是越多越好，因为业务程序会涉及到攒数据的场景，对内存的消耗是比较明显的，所以业务调用方一定要多多关注应用程序的GC情况。
-   数据攒批数量不要太小，如果太小，攒批写入意义就不大了，条件允许的情况下可以add 10000条进行一次commit。
-   DatabaseConfig配置在实例化client对象成功之后是不能再修改的，所有配置项必须在client对象初始化之前完成配置。
-   Client SDK目的是对写入（INSERT）提供性能优化，对于其他SQL操作，可以通过getConnection\(\)获得JDBC连接，通过标准JDBC接口进行处理

