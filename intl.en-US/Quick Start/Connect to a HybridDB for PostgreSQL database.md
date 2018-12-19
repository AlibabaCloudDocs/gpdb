# Connect to a HybridDB for PostgreSQL database {#concept_ncj_gmr_52b .concept}

Cloud Database HybridDB for PostgreSQL is fully compatible with the message protocols of PostgreSQL 8.2 and allows direct access to tools that support the PostgreSQL 8.2 message protocols such as libpq, JDBC, ODBC, psycopg2, pgadmin III, and so on.

Greenplum also provides an installation package that includes JDBC, ODBC, and libpq, which can be easily installed and used by users. For more information please see [the Greenplum official documentation](http://gpdb.docs.pivotal.io/4380/client_tool_guides/drivers/%20Unix/unix_connect.html).

## GUI Tools { .section}

HybridDB for PostgreSQL users can use Greenplum-supported graphical client tools directly, such as [SQL Workbench](http://www.sql-workbench.net/), [Navicat Premium](https://www.navicat.com/) [Navicat For PostgreSQL](https://www.navicat.com/download/navicat-for-postgresql), [pgAdmin III \(1.6.3\)](https://www.postgresql.org/ftp/pgadmin3/release/v1.6.3/) and so on.

The following content takes pgAdmin III as an example to illustrate the use of graphical client tools.

**pgAdmin III**

pgAdmin III is a GUI client of PostgreSQL, which can be used directly to connect to HybridDB for PostgreSQL. For more information, see [pgAdmin official page](https://www.pgadmin.org/).

You can download pgAdmin III 1.6.3 from [PostgreSQL official website](https://www.postgresql.org/ftp/pgadmin3/release/v1.6.3/). pgAdmin III 1.6.3 supports a variety of platforms, such as Windows, MacOS, and Linux.

**Note:** 

HybridDB for PostgreSQL supports PostgreSQL 8.2 version, and you must use the matching pgAdmin version. The matching version is pgAdmin III 1.6.3 or earlier versions.

**Procedure**

1.  Download and install pgAdmin III 1.6.3 or an earlier version.

2.  Select **File** \> **Add Server** to go to the New Server Registration page.

3.  Enter the **Properties** as shown in the following figure:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/16845/154520991012878_en-US.png)

4.  Click **OK** to connect to the HybridDB for PostgreSQL database.


## Command Line tools { .section}

Users can also use following several command line tools to connect to HybridDB for PostgreSQL instance’s database.

**psql**

psql is a common command line client tool for HybridDB for PostgreSQL. For RHEL \(Red Hat Enterprise Linux\) 6 or RHEL 7 and CentOS 6 or CentOS 7, Alibaba Cloud provides compressed software packages that can be used directly after decompression. .

-   For RHEL 6 or CentOS 6 platforms, click [download](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/43729/cn_zh/1483450178261/apsaradb_for_gp_client_package.redhat.el6.x86_64.tar.gz).

-   For RHEL 7 or CentOS 7 platforms, click [download](http://docs-aliyun.cn-hangzhou.oss.aliyun-inc.com/assets/attach/43729/cn_zh/1483450208287/apsaradb_for_gp_client_package.redhat.el7.x86_64.tar).


For other Linux platforms, users need to download the source code and use it after compiling and installation. The compiling methods is as follows:

1.  To get the source code, the following methods are available:
    -   Get the git directory directly\(make sure that you have installed the git tool\).

        ```
        git clone https://github.com/greenplum-db/gpdb.git
        cd gpdb
        git checkout 5d870156
        ```

    -   Directly download codes.

        ```
        wget https://github.com/greenplum-db/gpdb/archive/5d87015609abd330c68a5402c1267fc86cbc9e1f.zip
        unzip 5d87015609abd330c68a5402c1267fc86cbc9e1f.zip
        cd gpdb-5d87015609abd330c68a5402c1267fc86cbc9e1f
        ```

2.  You need the GCC or other compilers to compile the code and install the software.

    ```
    ./configure
    make -j32
    make install
    ```


After the installation the path of psql is as follows:

```
psql: `/usr/local/pgsql/bin/psql`
```

Enter the preceding directory, and use psql to connect to HybridDB for PostgreSQL instance’s database following to the procedure:

1.  Use one of the following methods to connect to the database:

    -   Connection strings

        ```
        psql "host=yourgpdbaddress.gpdb.rds.aliyuncs.com port=3432 dbname=postgres user=gpdbaccount password=gpdbpassword"
        ```

    -   Specify parameters

        ```
        psql  -h yourgpdbaddress.gpdb.rds.aliyuncs.com -p 3432 -d postgres -U gpdbaccount
        ```

        Parameter descriptions are as follows:

        -   -h: specifies the host address.
        -   -p: specifies the port number.
        -   -d: specifies the database \(the default database is postgres\).
        -   -U: specifies the connected user.

        You can view more parameters by performing `psql-- help`. And in the psql prompt, you can view more supported psql commands by performing `\?`.

2.  Enter the password to go to the psql shell interface. The psql shell is as follows:

    ```
    postgres=>
    ```


**Reference**

-   For more usage descriptions of Greenplum psql, see [psql](http://gpdb.docs.pivotal.io/4340/client_tool_guides/client/unix/psql.html).
-   You also use the PostgreSQL psql command, but do note the difference in usage details. For more information, see [PostgreSQL 8.3.23 Documentation — psql](https://www.postgresql.org/docs/8.3/static/app-psql.html).

**JDBC**

Users can use JDBC to connect to HybridDB for PostgreSQL instance’s database. Here are two ways to get this tool:

-   Download JDBC provided by PostgreSQL official website, click [PostgreSQL JDBC Driver](https://jdbc.postgresql.org/) and add the downloaded JDBC to the CLASSPATH Class variable before using it.

-   Use the tool package provided by the Greenplum official website. For more information, see [Greenplum Database 4.3 Connectivity Tools for UNIX](http://gpdb.docs.pivotal.io/4380/client_tool_guides/drivers/unix/unix_connect.html).


Providing the follow code example as a reference, and the users can modify it according to the practices.

**Code example:**

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

For more detailed information, see [PostgreSQL JDBC Interface](https://jdbc.postgresql.org/documentation/94/index.html).

**Python**

Users can also use Python to connect to HybridDB for PostgreSQL instance’s database. Python uses the psycopg2 library to connect to Greenplum and PostgreSQL. The procedure for using the tool is described as follows:

1.  Install psycopg2. In CentOS, three methods are available:

    -   Perform `yum -y install python-psycopg2`

    -   Perform `pip install psycopg2`

    -   Install from the source code.

        ```
          yum install -y postgresql-devel*
          wget  http://initd.org/psycopg/tarballs/PSYCOPG-2-6/psycopg2-2.6.tar.gz
          tar xf psycopg2-2.6.tar.gz
          cd psycopg2-2.6
          python setup.py build
          sudo python setup.py install
        ```

2.  After the installation, set the PYTHONPATH environment variable before using the tool. For example:

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

    A result similar to the following is returned.

    ```
    (1, -1, 'p', 'p', 's', 'u', 3022, '192.168.2.158', '192.168.2.158', None, None)
    (6, -1, 'm', 'm', 's', 'u', 3019, '192.168.2.47', '192.168.2.47', None, None)
    (2, 0, 'p', 'p', 's', 'u', 3025, '192.168.2.148', '192.168.2.148', 3525, None)
    (4, 0, 'm', 'm', 's', 'u', 3024, '192.168.2.158', '192.168.2.158', 3524, None)
    (3, 1, 'p', 'p', 's', 'u', 3023, '192.168.2.158', '192.168.2.158', 3523, None)
    (5, 1, 'm', 'm', 's', 'u', 3026, '192.168.2.148', '192.168.2.148', 3526, None)
    ```


**libpq**

Libpq is the C language interface of PostgreSQL database. You can access a PostgreSQL database in a C program through libpq to manipulate database. After Greenplum or PostgreSQL is installed, you can find its static libraries and dynamic libraries under the *lib* directory.

-   For related cases, see [libpq Example Programs](http://www.postgresql.org/docs/8.3/static/libpq-example.html).
-   For the details of libpq, see [PostgreSQL 9.4 Documentation - Chapter 31. libpq - C Library](http://www.postgresql.org/docs/9.4/static/libpq.html).

**ODBC**

PostgreSQL ODBC is an open-source version based on the LGPL \(GNU Lesser General Public License\) protocol. You can download it from [the official website of PostgreSQL](https://odbc.postgresql.org/).

**Procedure**

1.  Install the driver.

    ```
    yum install -y unixODBC.x86_64  
    yum install -y postgresql-odbc.x86_64
    ```

2.  Check the driver’s configuration.

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

3.  Configure DSN as the following codes. Change `****` in the codes to the actual connection information.

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

5.  After ODBC is connected to the instance, connect applications to ODBC. For more information, see [PostgreSQL ODBC driver](https://odbc.postgresql.org/) and [psqlODBC HOWTO - C\#](https://odbc.postgresql.org/howto-csharp.html).


## Windows and other platforms { .section}

Go to [Pivotal Greenplum Client](https://network.pivotal.io/products/pivotal-gpdb#/releases/2059/file_groups/408) for download links of other client tools for Windows and other platforms.

## Reference { .section}

-   [Pivotal Greenplum Official Documentation](http://gpdb.docs.pivotal.io/4380/common/welcome.html)
-   [PostgreSQL psql ODBC](https://odbc.postgresql.org/)
-   [PostgreSQL ODBC Compilation](https://odbc.postgresql.org/docs/unix-compilation.html)
-   [Greenplum ODBC Download](https://www.progress.com/odbc/pivotal-greenplum)
-   [Greenplum JDBC Download](https://www.progress.com/jdbc/pivotal-greenplum)

