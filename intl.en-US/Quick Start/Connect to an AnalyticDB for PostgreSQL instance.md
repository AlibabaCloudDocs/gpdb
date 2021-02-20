# Connect to an AnalyticDB for PostgreSQL instance

This topic describes how to connect to an AnalyticDB for PostgreSQL instance. AnalyticDB for PostgreSQL is fully compatible with the message-based protocol of PostgreSQL. Therefore, you can connect to an AnalyticDB for PostgreSQL instance by using tools that support the protocol, such as psql, libpq, Java Database Connectivity \(JDBC\), Open Database Connectivity \(ODBC\), and psycopg2. You can also use graphical user interface \(GUI\) tools such as pgAdmin, DBeaver, and Navicat. AnalyticDB for PostgreSQL V4.3 supports only pgAdmin III 1.6.3 and earlier. AnalyticDB for PostgreSQL V6.0 supports the latest version of pgAdmin 4. You can use Data Management Service \(DMS\) to manage and develop AnalyticDB for PostgreSQL instances.

**Note:** AnalyticDB for PostgreSQL V4.3 is based on the PostgreSQL 8.3 kernel. AnalyticDB for PostgreSQL V6.0 is based on the PostgreSQL 9.4 kernel.

## DMS console

[DMS](https://www.alibabacloud.com/help/zh/doc-detail/47550.htm) can manage relational databases, such as ApsaraDB RDS for MySQL, ApsaraDB RDS for SQL Server, ApsaraDB RDS for PostgreSQL, ApsaraDB RDS for PPAS, and HybridDB for MySQL, OLTP databases such as PolarDB-X, OLAP databases such as AnalyticDB and Data Lake Analytics \(DLA\), and NoSQL databases such as ApsaraDB for MongoDB and ApsaraDB for Redis. DMS offers an integrated solution to manage data, schemas, and servers. You can also use DMS to authorize users, audit security, view BI charts and data trends, track data, and optimize performance.

This section describes how to use the DMS console to connect to an AnalyticDB for PostgreSQL instance.

1.  Log on to the [AnalyticDB for PostgreSQL console](https://gpdbnext.console.aliyun.com/gpdb/cn-hangzhou/list).
2.  Create an instance. For more information, see [Create an instance](/intl.en-US/Quick Start/Create an instance.md). If you have created one, find the instance and click its ID.
3.  Create an account. For more information, see [Configure an account](/intl.en-US/Quick Start/Configure an account.md).

    **Note:** This account is used to log on to the DMS console. A single account can be created for each instance.

4.  In the upper-right corner, click **Login Database**.
5.  On the RDS Database Logon page that appears, enter the username and password, and click **Log On**.

## psql

psql is a command line tool used together with Greenplum, and provides a variety of commands. Its binary files are located in the bin directory of Greenplum. To use psql to connect to an AnalyticDB for PostgreSQL instance, perform the following steps:

1.  Connect to the instance by using one of the following methods:

    -   Connection string

        ```
        psql "host=yourgpdbaddress.gpdb.rds.aliyuncs.com port=3432 dbname=postgres user=gpdbaccount password=gpdbpassword"
        ```

    -   Specified parameters

        ```
        psql  -h yourgpdbaddress.gpdb.rds.aliyuncs.com -p 3432 -d postgres -U gpdbaccount
        ```

        The following section describes the parameters.

        -   -h: the host address.
        -   -p: the port used to connect to the database.
        -   -d: the name of the database. The default value is postgres.
        -   -U: the account used to connect to the database.
        -   You can run the `psql - help` command to view more options. You can also run the `\?` command to view the commands supported in psql.
2.  Enter the password to go to the psql shell interface. The following figure shows the psql shell interface.

    ```
    postgres=>
    ```


**References**

-   For more information, visit [gp6 psql](http://docs.greenplum.org/6-4/utility_guide/ref/psql.html#topic1) in the Greenplum database documentation.

-   AnalyticDB for PostgreSQL also supports psql commands for PostgreSQL. Pay attention to the differences between psql in Greenplum and that in PostgreSQL. For more information, visit [PostgreSQL 8.3.23 Documentation - psql](https://www.postgresql.org/docs/8.3/static/app-psql.html) in the PostgreSQL documentation.


**How to download client tools**

The download method varies depending on your operating system version:

-   Download client tools for AnalyticDB for PostgreSQL V4.3:

    -   If you use Red Hat Enterprise Linux 6 or CentOS 6, click [ADBPG\_client\_package\_el6](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/43729/cn_zh/1491914418625/apsaradb_for_gp_client_package.redhat.el6.x86_64.tar.gz).

    -   If you use Red Hat Enterprise Linux 7 or CentOS 7, click [ADBPG\_client\_package\_el7](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/43729/cn_zh/1491914523043/apsaradb_for_gp_client_package.redhat.el7.x86_64.tar.gz).

-   Download the client tool for AnalyticDB for PostgreSQL V6.0:

    -   If you use Red Hat Enterprise Linux 6 or CentOS 6, click [ADBPG\_client\_package\_el6](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/181125/cn_zh/1598426142282/adbpg_client_package.el6.x86_64.tar.gz).

    -   If you use Red Hat Enterprise Linux 7 or CentOS 7, click [ADBPG\_client\_package\_el7](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/181125/cn_zh/1598426198114/adbpg_client_package.el7.x86_64.tar.gz).


**How to use client tools**

After you download the client tool package, you can use the client tools. The following example shows how to use a client tool to connect to an AnalyticDB for PostgreSQL V6.0 instance when you use CentOS 7.

1.  Go to the directory where the client tool package is downloaded and decompress the package.

    ```
    tar -xzvf adbpg_client_package.el7.x86_64.tar.gz
    ```

2.  Switch to the bin directory and run the following command:

    ```
    cd adbpg_client_package/bin
    ```

3.  The bin directory includes client tools such as psql and pg\_dump. Run the command based on the reference documentation of each tool.

    -   For information about how to use psql to connect to an AnalyticDB for PostgreSQL instance, see [psql](#section_nll_ssr_52b).

    -   pg\_dump is a logical backup tool for PostgreSQL. For information about how to use pg\_dump, see [pg\_dump](https://gpdb.docs.pivotal.io/6-10/utility_guide/ref/pg_dump.html).


## pgAdmin

pgAdmin is a graphical client tool for PostgreSQL and can be used to connect to an AnalyticDB for PostgreSQL instance. For more information, visit the [official pgAdmin website](https://www.pgadmin.org/). AnalyticDB for PostgreSQL V4.3 is based on the PostgreSQL 8.3 kernel. Therefore, you must use pgAdmin III 1.6.3 or earlier to connect to an AnalyticDB for PostgreSQL V4.3 instance. AnalyticDB for PostgreSQL V4.3 does not support pgAdmin 4. AnalyticDB for PostgreSQL V6.0 is based on the PostgreSQL 9.4 kernel and supports the latest version of pgAdmin 4.

You can download [pgAdmin III 1.6.3](https://www.postgresql.org/ftp/pgadmin/pgadmin3/v1.6.3/) or [pgAdmin 4](https://www.pgadmin.org/docs/pgadmin4/1.x/) from the PostgreSQL official website. pgAdmin is compatible with various operating systems such as Windows, macOS, and Linux. For other graphical client tools, see [Graphical client tools](#graphy_client).

**Procedure**

1.  Download and install the supported version of pgAdmin.

2.  Choose **File** \> **Add Server**.

3.  In the New Server Registration dialog box, set parameters as prompted.

4.  Click **OK** to connect to the AnalyticDB for PostgreSQL instance.


## JDBC

You must use the official PostgreSQL JDBC driver. Use one of the following methods to download the JDBC driver:

-   Click [PostgreSQL JDBC Driver](https://jdbc.postgresql.org/) to download the official PostgreSQL JDBC driver and add it to an environment variable.

-   Obtain the JDBC driver from the tool package provided at the official Greenplum website. For more information, visit [Greenplum Database 4.3 Connectivity Tools for UNIX](http://gpdb.docs.pivotal.io/43330/client_tool_guides/drivers/unix/unix_connect.html) or [Greenplum Client and Loader Tools Package](https://gpdb.docs.pivotal.io/6-3/client_tool_guides/intro.html).


**Sample code**

```
import java.sql.Connection;  
import java.sql.DriverManager;  
import java.sql.ResultSet;  
import java.sql.SQLException;  
import java.sql.Statement;  
public class gp_conn {  
    public static void main(String[] args) {  
        try {  
            Class.forName("org.postgresql.Driver");  
            Connection db = DriverManager.getConnection("jdbc:postgresql://mygpdbpub.gpdb.rds.aliyuncs.com:3432/postgres","mygpdb","mygpdb");  
            Statement st = db.createStatement();  
            ResultSet rs = st.executeQuery("select * from gp_segment_configuration;");  
            while (rs.next()) {  
                System.out.print(rs.getString(1));  
                System.out.print("    |    ");  
                System.out.print(rs.getString(2));  
                System.out.print("    |    ");  
                System.out.print(rs.getString(3));  
                System.out.print("    |    ");  
                System.out.print(rs.getString(4));  
                System.out.print("    |    ");  
                System.out.print(rs.getString(5));  
                System.out.print("    |    ");  
                System.out.print(rs.getString(6));  
                System.out.print("    |    ");  
                System.out.print(rs.getString(7));  
                System.out.print("    |    ");  
                System.out.print(rs.getString(8));  
                System.out.print("    |    ");  
                System.out.print(rs.getString(9));  
                System.out.print("    |    ");  
                System.out.print(rs.getString(10));  
                System.out.print("    |    ");  
                System.out.println(rs.getString(11));  
            }  
            rs.close();  
            st.close();  
        } catch (ClassNotFoundException e) {  
            e.printStackTrace();  
        } catch (SQLException e) {  
            e.printStackTrace();  
        }  
    }  
}
```

For more information, visit [The PostgreSQL JDBC Interface](https://jdbc.postgresql.org/documentation/94/index.html).

## Python

You can use psycopg2 to connect to Greenplum or PostgreSQL. The procedure is as follows:

1.  Install psycopg2. If you use CentOS, you can use one of the following installation methods:

    -   Method 1: Run the `yum -y install python-psycopg2` command.
    -   Method 2: Run the `pip install psycopg2` command.
    -   Method 3: Use the source code.

        ```
        yum install -y postgresql-devel*
        wget  http://initd.org/psycopg/tarballs/PSYCOPG-2-6/psycopg2-2.6.tar.gz
        tar xf psycopg2-2.6.tar.gz
        cd psycopg2-2.6
        python setup.py build
        sudo python setup.py install
        ```

2.  Set the PYTHONPATH environment variable and then import psycopg2:

    ```
     import psycopg2
     sql = 'select * from gp_segment_configuration;'
     conn = psycopg2.connect(database='gpdb', user='mygpdb', password='mygpdb', host='mygpdbpub.gpdb.rds.aliyuncs.com', port=3432)
     conn.autocommit = True
     cursor = conn.cursor()
     cursor.execute(sql)
     rows = cursor.fetchall()
     for row in rows:
         print row
     conn.commit()
     conn.close()
    ```

    The system displays information similar to the following:

    ```
    (1, -1, 'p', 'p', 's', 'u', 3022, '192.168.2.158', '192.168.2.158', None, None)
    (6, -1, 'm', 'm', 's', 'u', 3019, '192.168.2.47', '192.168.2.47', None, None)
    (2, 0, 'p', 'p', 's', 'u', 3025, '192.168.2.148', '192.168.2.148', 3525, None)
    (4, 0, 'm', 'm', 's', 'u', 3024, '192.168.2.158', '192.168.2.158', 3524, None)
    (3, 1, 'p', 'p', 's', 'u', 3023, '192.168.2.158', '192.168.2.158', 3523, None)
    (5, 1, 'm', 'm', 's', 'u', 3026, '192.168.2.148', '192.168.2.148', 3526, None)
    ```


## libpq

libpq is the C application programmer's interface to PostgreSQL. You can use libpq libraries to access and manage PostgreSQL databases in a C program. If Greenplum or PostgreSQL is deployed, you can find both the static and dynamic libraries of libpq in the lib directory.

For example programs, click [30.19. Example Programs](http://www.postgresql.org/docs/8.3/static/libpq-example.html).

For more information about libpq, visit [PostgreSQL 9.4.10 Documentation - Chapter 31. libpq - C Library](http://www.postgresql.org/docs/9.4/static/libpq.html).

## ODBC

The PostgreSQL ODBC driver is an open source tool licensed based on the GNU Lesser General Public License \(LGPL\) protocol. You can download it from the [official PostgreSQL website](https://odbc.postgresql.org/).

**Procedure**

1.  Install the driver.

    ```
    yum install -y unixODBC.x86_64  
    yum install -y postgresql-odbc.x86_64
    ```

2.  View the driver configuration.

    ```
    cat /etc/odbcinst.ini 
    # Example driver definitions
    # Driver from the postgresql-odbc package
    # Setup from the unixODBC package
    [PostgreSQL]
    Description     = ODBC for PostgreSQL
    Driver          = /usr/lib/psqlodbcw.so
    Setup           = /usr/lib/libodbcpsqlS.so
    Driver64        = /usr/lib64/psqlodbcw.so
    Setup64         = /usr/lib64/libodbcpsqlS.so
    FileUsage       = 1
    # Driver from the mysql-connector-odbc package
    # Setup from the unixODBC package
    [MySQL]
    Description     = ODBC for MySQL
    Driver          = /usr/lib/libmyodbc5.so
    Setup           = /usr/lib/libodbcmyS.so
    Driver64        = /usr/lib64/libmyodbc5.so
    Setup64         = /usr/lib64/libodbcmyS.so
    FileUsage       = 1
    ```

3.  Configure the DSN. Replace `****` in the following code with actual information.

    ```
    [mygpdb]
    Description = Test to gp
    Driver = PostgreSQL
    Database = ****
    Servername = ****.gpdb.rds.aliyuncs.com
    UserName = ****
    Password = ****
    Port = ****
    ReadOnly = 0
    ```

4.  Test the connectivity.

    ```
    echo "select count(*) from pg_class" | isql mygpdb
    +---------------------------------------+
    | Connected!                            |
    |                                       |
    | sql-statement                         |
    | help [tablename]                      |
    | quit                                  |
    |                                       |
    +---------------------------------------+
    SQL> select count(*) from pg_class
    +---------------------+
    | count               |
    +---------------------+
    | 388                 |
    +---------------------+
    SQLRowCount returns 1
    1 rows fetched
    ```

5.  After ODBC is connected to the instance, connect your application to ODBC. For more information, visit [PostgreSQL ODBC driver](https://odbc.postgresql.org/) and [psqlODBC HOWTO - C\#](https://odbc.postgresql.org/howto-csharp.html).


## More information

**Graphical client tools**

You can use the following graphical client tools to connect to AnalyticDB for PostgreSQL instances:

-   [pgadmin III \(1.6.3\)](https://www.postgresql.org/ftp/pgadmin/pgadmin3/v1.6.3/)
-   [pgAdmin 4](https://www.pgadmin.org/)
-   [dbeaver](https://dbeaver.io/)
-   [Navicat Premium](https://www.navicat.com/download/navicat-premium)
-   [Navicat For PostgreSQL](https://www.navicat.com/download/navicat-for-postgresql)
-   [SQL Workbench](http://www.sql-workbench.net/)

**Greenplum client tools**

The official Greenplum website provides a tool package, which includes JDBC, ODBC, and libpq. The package is easy to install and use. For more information, visit [Pivotal Greenplum documentation](http://gpdb.docs.pivotal.io/43330/client_tool_guides/drivers/unix/unix_connect.html).

## References

-   [Pivotal Greenplum Database 4.3.33 Documentation](http://gpdb.docs.pivotal.io/43330/common/welcome.html)

-   [Pivotal Greenplum 6.3 Documentation](https://gpdb.docs.pivotal.io/6-3/main/index.html)

-   [PostgreSQL psqlODBC](https://odbc.postgresql.org/)

-   [Compiling psqlODBC on Unix](https://odbc.postgresql.org/docs/unix-compilation.html)

-   [Download ODBC connectors](https://www.progress.com/odbc/pivotal-greenplum)

-   [Download JDBC connectors](https://www.progress.com/jdbc/pivotal-greenplum)


