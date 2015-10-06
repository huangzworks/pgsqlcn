SQL 语言简介
======================


引言
--------------

..
    This chapter provides an overview of how to use SQL to perform simple operations. 

    This tutorial is only intended to give you an introduction 
    and is in no way a complete tutorial on SQL. 

    Numerous books have been written on SQL, 
    including Understanding the New SQL and A Guide to the SQL Standard. 
    You should be aware that 
    some PostgreSQL language features are extensions to the standard.

本章将介绍一些使用 SQL 去执行简单操作的方法。
这个教程介绍的都是一些入门知识，
这里并不会对 SQL 进行完整的介绍。
不过关于 SQL 有很多好的参考书可以参考，
比如《Understanding the New SQL》和《A Guide to the SQL Standard》。
另外需要注意的一点是，
某些 PostgreSQL 语言的特性是扩展自 SQL 标准的，
它们并不属于 SQL 标准。

..
    In the examples that follow, 
    we assume that you have created a database named mydb, 
    as described in the previous chapter, 
    and have been able to start psql.

接下来的示例将假设你已经按照前面介绍的方法创建了一个名为 ``mydb`` 的数据库，
并且已经启动了 ``psql`` 。

..
    Examples in this manual can also be found in the PostgreSQL source distribution 
    in the directory src/tutorial/. 
    (Binary distributions of PostgreSQL might not compile these files.) 
    To use those files, 
    first change to that directory and run make:

这个教程展示的示例也可以在 PostgreSQL 的源码发行版里面的 ``src/tutorial`` 文件夹里面找到。
（但是 PostgreSQL 的二进制分发版可能并没有包含这些文件。）
要使用这些文件，
你可以进入到文件的文件夹里面，
并执行以下命令：

.. code-block:: bash

    $ cd ..../src/tutorial
    $ make

..
    This creates the scripts and compiles the C files containing user-defined functions and types. 
    Then, 
    to start the tutorial, 
    do the following:

上面的这个命令将创建出一些脚本，
并对包含了用户定义的函数和类型的 C 文件进行编译。
之后，
为了跟随教程进行操作，
我们需要执行以下命令：

.. code-block:: bash

    $ cd ..../tutorial
    $ psql -s mydb
    ...

    mydb=> \i basics.sql

..
    The \i command reads in commands from the specified file. 
    psql's -s option puts you in single step mode 
    which pauses before sending each statement to the server. 
    The commands used in this section are in the file basics.sql.

``-s`` 选项可以让 ``psql`` 程序进入单步模式，
使得 ``psql`` 在将每条语句发送至服务器之前暂停。
``\i`` 命令将从指定的文件里面读入命令。
本节要用到的命令都包含在了 ``basics.sql`` 文件里面。


概念
-----------

..
    PostgreSQL is a *relational database management system* (RDBMS). 
    That means it is a system for managing data stored in *relations*. 
    Relation is essentially a mathematical term for *table*. 

    The notion of storing data in tables is so commonplace today that it might seem inherently obvious, 
    but there are a number of other ways of organizing databases. 

    Files and directories on Unix-like operating systems form an example of a hierarchical database. 
    A more modern development is the object-oriented database.

PostgreSQL 是一个\ *关系数据库管理系统*\ （RDBMS），
它负责对储存在\ *关系*\ （relation）里面的数据进行管理，
而关系本质上就是\ *表格*\ （table）在数学上的对应术语。
在当今世界，
将数据储存到表格里面的做法是如此地常见，
看上去就像是理所当然似的，
但实际上目前还是存在着好几种不同的数据库组织方法。
比如 Unix 这一类系统上面就使用继承数据库去处理文件和文件夹，
而更现代化的开发可能会用到面向对象数据库。

..
    Each table is a named collection of *rows*. 
    Each row of a given table has the same set of named *columns*, 
    and each column is of a specific data type. 
    Whereas columns have a fixed order in each row, 
    it is important to remember that 
    SQL does not guarantee the order of the rows within the table in any way 
    (although they can be explicitly sorted for display).

数据库中的每个表格都是\ *行*\ （row）的具名集合（named collection），
这些行由它们在表格中所属的\ *列*\ （column）进行命名，
并且每个列都属于某种指定的数据类型。
尽管列在每一行中的位置都是固定的，
并且用户也可以在打印行的时候，
显式地对行进行排序，
但是我们需要记住，
SQL 并不会以任何方式保证行在表格中的位置。

..
    Tables are grouped into databases, 
    and a collection of databases managed by a single PostgreSQL server instance 
    constitutes a database *cluster*.

不同的表格会被组织到不同的数据库里面，
而由单个 PostgreSQL 服务器实例（instance）管理的多个数据库则构成了一个数据库\ *集群*\ （cluster）。


创建新的表格
---------------------

..
    You can create a new table by specifying the table name, 
    along with all column names and their types:

通过为表格指定一个名字，
并为表格的所有列指定名字和类型，
我们可以创建出一个新的表格：

::

    CREATE TABLE weather (
        city            varchar(80),
        temp_lo         int,           -- 最低温度
        temp_hi         int,           -- 最高温度
        prcp            real,          -- 降雨量（precipitation）
        date            date
    );

..
    You can enter this into psql with the line breaks. 
    psql will recognize that the command is not terminated 
    until the semicolon.

在输入表格的定义时，
你可以在定义中加入空行，
``psql`` 会正确地识别这些空行，
并将分号 ``;`` 看做是输入的终结。

..
    White space (i.e., spaces, tabs, and newlines) can be used freely in SQL commands. 
    That means you can type the command aligned differently than above, 
    or even all on one line. 
    Two dashes ("--") introduce comments. 
    Whatever follows them is ignored up to the end of the line. 
    SQL is case insensitive about key words and identifiers, 
    except when identifiers are double-quoted to preserve the case (not done above).

用户可以自由地在 SQL 命令中使用空白（包括空格、制表符和新行）。
这意味着用户可以以不同的对齐方式输入上面展示的命令，
又或者将所有命令都放到同一行里面。
两个破折号 ``--`` 用于引出注释，
位于注释之后的内容将被忽略，
注释的效果会一直延续到那一行的结尾。
在一般情况下，
SQL 并不会区分关键词和标识符的大小写，
除非用户使用双引号去显式地保留标识符的大小写
（上面展示的例子并没有这样做）。

..
    varchar(80) specifies a data type 
    that can store arbitrary character strings up to 80 characters in length. 
    int is the normal integer type. 
    real is a type for storing single precision floating-point numbers. 
    date should be self-explanatory. 
    (Yes, 
    the column of type date is also named date. 
    This might be convenient or confusing — you choose.)

``varchar(80)`` 指定了一种数据类型，
这种数据类型可以储存包含任意字符的字符串，
但这个字符串的长度不能超过 80 个字。
``int`` 是标准的整数类型。
``real`` 类型用于储存单精度浮点数。
``date`` 类型则用于储存日期。
（在上面的定义中，
名字为 ``date`` 的列的数据类型同样为 ``date`` ，
这种做法到底是好是坏则是一个见仁见智的问题。）

..
    PostgreSQL supports the standard SQL types int, smallint, real, double precision, char(N), varchar(N), date, time, timestamp, and interval, 
    as well as other types of general utility 
    and a rich set of geometric types. 

    PostgreSQL can be customized with an arbitrary number of user-defined data types. 

    Consequently, 
    type names are not key words in the syntax, 
    except where required to support special cases in the SQL standard.

PostgreSQL 支持包括 ``int`` 、 ``smallint`` 、 ``real`` 、 双精度浮点数、 ``char(N)`` 、 ``varchar(N)`` 、 ``date`` 、 ``timestamp`` 、 ``interval`` 在内的标准 SQL 类型，
一些通用类型（general utility）以及一系列丰富多样的地理类型（geometric types）。
除此之外，
PostgreSQL 还允许用户自行制定任意数量的用户定义数据类型（user-defined data types）。
因为这个原因，
除了那些 SQL 标准中定义的数据类型之外，
PostgreSQL 中的类型名都不是语法上的关键字（key words）。

..
    The second example will store cities and their associated geographical location:

第二个表定义将展示如何储存城市以及城市的地理位置：

::

    CREATE TABLE cities (
        name            varchar(80),
        location        point
    );

..
    The point type is an example of a PostgreSQL-specific data type.

表格定义中展示的 ``point`` 类型是其中一种 PostgreSQL 特有数据类型。

..
    Finally, 
    it should be mentioned that 
    if you don't need a table any longer 
    or want to recreate it differently 
    you can remove it using the following command:

最后，
如果你不再需要某个表格，
又或者想要以不同的定义重新创建某个表格，
那么你可以使用以下命令去移除那个表格：

::

    DROP TABLE tablename;


将行插入到表格
-------------------

..
    The INSERT statement is used to populate a table with rows:

``INSERT`` 语句用于将一个行插入到表格里面：

::

    INSERT INTO weather VALUES ('San Francisco', 46, 50, 0.25, '1994-11-27');

..
    Note that all data types use rather obvious input formats. 
    Constants that are not simple numeric values usually must be surrounded by single quotes ('), 
    as in the example. 
    The date type is actually quite flexible in what it accepts, 
    but for this tutorial we will stick to the unambiguous format shown here.

注意所有数据类型都使用相当明显的输入格式。
非数字值的常量通常由单引号 ``'`` 包围，
就像上面的例子所展示的那样。
数据类型在接受输入方面实际上是非常灵活的，
不过在这个教程里面，
我们只会使用那些意义明确的格式。

..
    The point type requires a coordinate pair as input, 
    as shown here:

``point`` 类型要求一对坐标点作为输入，
就像这样：

::

    INSERT INTO cities VALUES ('San Francisco', '(-194.0, 53.0)');

..
    The syntax used so far requires you to remember the order of the columns. 
    An alternative syntax allows you to list the columns explicitly:

目前展示过的两种 ``INSERT`` 语法都要求你记住表格中各个列的排列顺序，
但以下这种语法允许你显式地指定各个值要被插入到哪个列里面：

::

    INSERT INTO weather (city, temp_lo, temp_hi, prcp, date)
        VALUES ('San Francisco', 43, 57, 0.0, '1994-11-29');

..
    You can list the columns in a different order if you wish 
    or even omit some columns, 
    e.g., if the precipitation is unknown:

通过这种语法，
用户可以以不同的顺序列出表格中的各个列，
又或者省略某个列。
比如说，
如果 ``Hayward`` 城市的降雨量是未知的，
那么我们可以执行以下 ``INSERT`` 语句：

::

    INSERT INTO weather (date, city, temp_hi, temp_lo)
        VALUES ('1994-11-29', 'Hayward', 54, 37);

..
    Many developers consider explicitly listing the columns better style 
    than relying on the order implicitly.

比起隐式地依赖列的定义顺序，
很多开发者都认为显式地列出各个列是一种更好的风格。

..
    Please enter all the commands shown above 
    so you have some data to work with in the following sections.

请你键入上面展示的所有命令，
以便获得接下来的章节将要用到的数据。

..
    You could also have used COPY to load large amounts of data from flat-text files. 

    This is usually faster 
    because the COPY command is optimized for this application 
    while allowing less flexibility than INSERT. 
    An example would be:

用户也可以通过 ``COPY`` 命令，
从文本文件里面载入大量数据。
因为 ``COPY`` 命令为这种用法做了优化，
所以使用 ``COPY`` 命令载入数据的速度一般会比 ``INSERT`` 语句快一些，
不过 ``COPY`` 命令的灵活性比 ``INSERT`` 语句要差一些。

以下是一个使用 ``COPY`` 命令载入数据的例子：

::

    COPY weather FROM '/home/user/weather.txt';

..
    where the file name for the source file must be available on the machine running the backend process, 
    not the client, 
    since the backend process reads the file directly. 
    You can read more about the COPY command in COPY.

因为读入源码文件的工作是由后端进程而不是客户端进程负责执行的，
所以用户指示 ``COPY`` 命令去读取的源码文件必须对正在运行后端进程的机器可用。
要了解关于 ``COPY`` 命令的更多信息，
请查看 `COPY 命令的文档 <http://www.postgresql.org/docs/9.5/static/sql-copy.html>`_\ 。


表格查询
-------------

..
    To retrieve data from a table, 
    the table is queried. 

    An SQL SELECT statement is used to do this. 

    The statement is divided into a select list 
    (the part that lists the columns to be returned),
    a table list 
    (the part that lists the tables from which to retrieve the data), 
    and an optional qualification 
    (the part that specifies any restrictions). 

    For example, 
    to retrieve all the rows of table weather, 
    type:

为了从表格里面获取数据，
我们需要对表格进行\ *查询*\ 。
查询一般由 SQL 的 ``SELECT`` 语句负责执行，
一条 ``SELECT`` 语句通常由一个选择列表（select list）、一个表格列表（table list）以及一个可选的条件构成：
其中选择列表用于指定需要返回的列，
表格列表用于指定 ``SELECT`` 语句需要从哪个表格里面获取数据，
而可选的条件则用于指定查询时的限制条件。

举个例子，
要从 ``weather`` 表格里面获取所有行，
我们可以输入以下语句：

::

    SELECT * FROM weather;

..
    Here * is a shorthand for "all columns". [#f1]_ 
    So the same result would be had with:

上面这个查询语句中的 ``*`` 号代表“所有列”，
因此这个语句的查询结果和以下这个语句完全一样：

::

    SELECT city, temp_lo, temp_hi, prcp, date FROM weather;

..
    The output should be:

这两条 ``SELECT`` 语句的结果为：

::

         city      | temp_lo | temp_hi | prcp |    date
    ---------------+---------+---------+------+------------
     San Francisco |      46 |      50 | 0.25 | 1994-11-27
     San Francisco |      43 |      57 |    0 | 1994-11-29
     Hayward       |      37 |      54 |      | 1994-11-29
    (3 rows)

..
    You can write expressions, 
    not just simple column references, 
    in the select list. 
    For example, 
    you can do:

除了编写简单的列引用之外，
用户还可以在选择列表里面编写表达式，
就像这样：

::

    SELECT city, (temp_hi+temp_lo)/2 AS temp_avg, date FROM weather;

..
    This should give:

上面的这个语句将产生以下结果：

::

         city      | temp_avg |    date
    ---------------+----------+------------
     San Francisco |       48 | 1994-11-27
     San Francisco |       50 | 1994-11-29
     Hayward       |       45 | 1994-11-29
    (3 rows)

..
    Notice how the AS clause is used to relabel the output column. 
    (The AS clause is optional.)

注意 ``AS`` 语句是如何对输出的列进行重命名的。（\ ``AS`` 语句是可选的。）

..
    A query can be "qualified" by adding a WHERE clause 
    that specifies which rows are wanted. 

    The WHERE clause contains a Boolean (truth value) expression, 
    and only rows for which the Boolean expression is true are returned. 
    
    The usual Boolean operators (AND, OR, and NOT) are allowed in the qualification. 
    
    For example, 
    the following retrieves the weather of San Francisco on rainy days:

查询可以通过添加 ``WHERE`` 语句来指定哪些行能够被返回。

``WHERE`` 语句包含一个布尔（真值）表达式，
只有哪些能够让布尔表达式返回真值的行会被返回。
``WHERE`` 语句的表达式可以包含常用的布尔操作符，
比如 ``AND`` 、 ``OR`` 和 ``NOT`` 。
举个例子，
以下这个语句只会返回 San Francisco 在雨天时的天气：

::

    SELECT * FROM weather
        WHERE city = 'San Francisco' AND prcp > 0.0;
..
    Result:

执行上面这个语句将得到以下结果：

::

         city      | temp_lo | temp_hi | prcp |    date
    ---------------+---------+---------+------+------------
     San Francisco |      46 |      50 | 0.25 | 1994-11-27
    (1 row)

..
    You can request that the results of a query be returned in sorted order:

通过使用可选的 ``ORDER BY`` 语句，
用户可以让查询结果以有序的方式返回：

::

    SELECT * FROM weather
        ORDER BY city;

..
    Result:

执行上面这个语句将得到以下结果：

::

         city      | temp_lo | temp_hi | prcp |    date
    ---------------+---------+---------+------+------------
     Hayward       |      37 |      54 |      | 1994-11-29
     San Francisco |      43 |      57 |    0 | 1994-11-29
     San Francisco |      46 |      50 | 0.25 | 1994-11-27

..
    In this example, 
    the sort order isn't fully specified, 
    and so you might get the San Francisco rows in either order. 
    But you'd always get the results shown above if you do:

在这个例子中，
排序结构并未完全被指定，
因此 San Francisco 的两个行可能会以不同的顺序被返回。
但通过执行以下语句，
我们可以让查询结果总是以相同的顺序返回：

::

    SELECT * FROM weather
        ORDER BY city, temp_lo;

..
    You can request that duplicate rows be removed from the result of a query:

通过执行以下查询，
我们可以移除查询结果中重复出现的行：

::

    SELECT DISTINCT city
        FROM weather;

..
    Result:

执行上面这个语句将得到以下结果：

::

        city
    ---------------
    Hayward
    San Francisco
    (2 rows)

..
    Here again, 
    the result row ordering might vary. 
    You can ensure consistent results by using DISTINCT and ORDER BY together: [#f2]_

跟前面的情况类似，
上面的语句产生的结果可能会以不同的顺序被返回。
但通过同时使用 ``DISTINCT`` 和 ``ORDER BY`` ，
我们可以确保查询结果总是有序的、并且不会出现重复的结果：

::

    SELECT DISTINCT city
        FROM weather
            ORDER BY city;

..  脚注
    [f1] 
    While SELECT * is useful for off-the-cuff queries, it is widely considered bad style in production code, since adding a column to the table would change the results.
    [2] 
    In some database systems, including older versions of PostgreSQL, the implementation of DISTINCT automatically orders the rows and so ORDER BY is unnecessary. But this is not required by the SQL standard, and current PostgreSQL does not guarantee that DISTINCT causes the rows to be ordered.


连接多个表格
-------------------------

..
    Thus far, 
    our queries have only accessed one table at a time. 

    Queries can access multiple tables at once, 
    or access the same table in such a way that 
    multiple rows of the table are being processed at the same time. 

    A query that accesses multiple rows of the same or different tables at one time 
    is called a *join* query. 

    As an example, 
    say you wish to list all the weather records 
    together with the location of the associated city. 

    To do that, 
    we need to compare the city column of each row of the ``weather`` table 
    with the name column of all rows in the ``cities`` table, 
    and select the pairs of rows where these values match.

到目前为止，
本章展示的所有查询每次都只会访问一个表格。
但实际上，
查询既可以在同一时间里面访问多个表格，
又可以在同一时间里面，
对同一个表格中的多个行进行处理。
这种在同一时间里面访问相同或者不同表格中的多个行的查询被称为\ *连接*\ 查询。

举个例子，
假设我们想要在获取所有天气记录的同时，
将这些记录相关联的城市的地理位置也一并返回。
为了做到这一点，
我们需要将 ``weather`` 表格中每个行的 ``city`` 一列的值与 ``cities`` 表格中每个行的 ``name`` 一列的值进行比较，
并从这两个表格里面选出哪些比较结果相同的配对行（pairs of rows）。

..
    .. note::

        This is only a conceptual model. 
        The join is usually performed in a more efficient manner 
        than actually comparing each possible pair of rows, 
        but this is invisible to the user.

.. note::

    上面描述的只是一个概念模型。
    比起一个接一个地对比每个可能被配对的行，
    连接通常会以一种更为高效的方式执行，
    不过这对于用户来说是不可见的（invisible）。

..
    This would be accomplished by the following query:

以下查询可以将天气记录和相关城市的地理位置一并返回：

::

    SELECT *
        FROM weather, cities
            WHERE city = name;

::

         city      | temp_lo | temp_hi | prcp |    date    |     name      | location
    ---------------+---------+---------+------+------------+---------------+-----------
     San Francisco |      46 |      50 | 0.25 | 1994-11-27 | San Francisco | (-194,53)
     San Francisco |      43 |      57 |    0 | 1994-11-29 | San Francisco | (-194,53)
    (2 rows)

..
    Observe two things about the result set:

查询的执行结果里面有两个需要注意的地方：

..
    - There is no result row for the city of Hayward. 
      This is because there is no matching entry in the cities table for Hayward, 
      so the join ignores the unmatched rows in the weather table. 
      We will see shortly how this can be fixed.

- 查询结果里面并未包含与 Hayward 城市有关的信息。
  这是因为 ``cities`` 表格里面并没有记载 Hayward 城市的地理位置，
  所以连接操作就略去了 ``weather`` 表格中关于 Hayward 城市的未匹配行。
  我们稍后就会看到解决这个问题的方法。

..
    - There are two columns containing the city name. 
      This is correct because the lists of columns from the weather 
      and cities tables are concatenated. 
      In practice this is undesirable, 
      though, 
      so you will probably want to list the output columns explicitly rather than using *:

- 结果里面有两个列包含了城市的名字，
  这是因为我们将 ``weather`` 表格和 ``cities`` 表格包含的所有列都连接在了一起。
  虽然这种行为是正确的，
  但我们实际上并不想将城市名显示两次，
  为此，
  我们可以直接在查询语句的选择列表里面显式地列出所有想要返回的列，
  而不再使用 ``*`` ：

  ::

      SELECT city, temp_lo, temp_hi, prcp, date, location
          FROM weather, cities
              WHERE city = name;

.. **\fix editor highlight

..
    **Exercise:** Attempt to determine the semantics of this query when the WHERE clause is omitted.

**练习：**\ 思考一下，如果去掉上面的查询语句中的 ``WHERE`` 子句，查询的结果将会变成怎样？

..
    Since the columns all had different names, 
    the parser automatically found which table they belong to. 
    If there were duplicate column names in the two tables 
    you'd need to qualify the column names to show which one you meant, 
    as in:

因为 ``weather`` 表格和 ``cities`` 表格的列都有不同的名字，
所以语法分析器会自动地判断查询语句中的各个列分别属于哪个表格。
但是，
如果两个表格里面包含有相同的列名，
那么用户在输入查询语句的时候，
就需要在列名的前面加上具体的表格名，
以此来说明自己想要使用的是哪个列：

::

    SELECT weather.city, weather.temp_lo, weather.temp_hi,
           weather.prcp, weather.date, cities.location
        FROM weather, cities
        WHERE cities.name = weather.city;

..
    It is widely considered good style to qualify all column names in a join query, 
    so that the query won't fail 
    if a duplicate column name is later added to one of the tables.

在执行连接查询的时候显式地标识各个列所属的表格通常被认为是一种良好的风格，
因为这种做法即使在多个表格拥有相同列名的时候也可以正确地执行。

..
    Join queries of the kind seen thus far can also be written in this alternative form:

目前展示过的连接查询也可以改写为以下这种形式：

::

    SELECT *
        FROM weather INNER JOIN cities ON (weather.city = cities.name);

..
    This syntax is not as commonly used as the one above, 
    but we show it here to help you understand the following topics.

这种语法并没有前一种语法常见，
但展示这种语法有助于我们理解接下来要介绍的内容。

..
    Now we will figure out how we can get the Hayward records back in. 

    What we want the query to do is to scan the weather table
    and for each row to find the matching cities row(s).

    If no matching row is found 
    we want some "empty values" to be substituted for the cities table's columns. 

    This kind of query is called an outer join. 
    (The joins we have seen so far are inner joins.)
    The command looks like this:

现在让我们来考虑一下，
如何才能让 Hayward 的天气记录出现在查询结果里面。
我们想要让查询去扫描 ``weather`` 表格的每个行，
并寻找其中与 ``cities`` 城市相匹配的行，
并在没有找到相匹配的行时，
使用一些“空值”（empty value）去代替 ``cities`` 表格中的列。
这种查询被称为\ *外连接*\ （outer join），
而本节前面展示的连接则为\ *内连接*\ （inner join）。
以下查询可以实现上述的要求：

::

    SELECT *
        FROM weather LEFT OUTER JOIN cities ON (weather.city = cities.name);

::

         city      | temp_lo | temp_hi | prcp |    date    |     name      | location
    ---------------+---------+---------+------+------------+---------------+-----------
     Hayward       |      37 |      54 |      | 1994-11-29 |               |
     San Francisco |      46 |      50 | 0.25 | 1994-11-27 | San Francisco | (-194,53)
     San Francisco |      43 |      57 |    0 | 1994-11-29 | San Francisco | (-194,53)
    (3 rows)

..
    This query is called a left outer join 
    because the table mentioned on the left of the join operator 
    will have each of its rows in the output at least once, 
    whereas the table on the right will only have those rows output 
    that match some row of the left table. 

    When outputting a left-table row for which there is no right-table match, 
    empty (null) values are substituted for the right-table columns.

上面的这个查询被称为\ *左外连接*\ （left outer join），
因为位于连接操作符左边的表格的每个行至少会在查询结果里面出现一次，
而位于连接操作符右边的表格的行只会在它与左边表格中的某个行相匹配时出现。
在返回一个来自于左边表格的行时，
如果那个行在右边表格里面没有与之相匹配的行，
那么空缺的那个右边表格的列就会用空值（null）代替。

..
    **Exercise:** There are also right outer joins and full outer joins. Try to find out what those do.

**练习：**\ 除了左外连接之外，还有右外连接（right outer join）和全外连接（full outer join），请尝试执行这些连接。

..
    We can also join a table against itself. 
    This is called a self join. 

    As an example, 
    suppose we wish to find all the weather records 
    that are in the temperature range of other weather records. 

    So we need to compare the temp_lo and temp_hi columns of each weather row 
    to the temp_lo and temp_hi columns of all other weather rows. 

    We can do this with the following query:

除了以上提到的连接方式之外，
用户还可以将一个表格与它自身进行连接，
这种连接被称为\ *自连接*\ （self join）。

作为例子，
假设我们想要根据某个指定的天气记录的温度区间，
找出位于这个温度区间内的所有天气记录，
那么我们就需要将 ``weather`` 表格中某个指定行的 ``temp_lo`` 一列和 ``temp_hi`` 一列，
与 ``weather`` 表格中每个行的 ``temp_lo`` 一列和 ``temp_hi`` 一列进行对比。
具体的查询语句如下：

::

    SELECT W1.city, W1.temp_lo AS low, W1.temp_hi AS high,
        W2.city, W2.temp_lo AS low, W2.temp_hi AS high
        FROM weather W1, weather W2
        WHERE W1.temp_lo < W2.temp_lo
        AND W1.temp_hi > W2.temp_hi;

         city      | low | high |     city      | low | high
    ---------------+-----+------+---------------+-----+------
     San Francisco |  43 |   57 | San Francisco |  46 |   50
     Hayward       |  37 |   54 | San Francisco |  46 |   50
    (2 rows)

..
    Here we have relabeled the weather table as W1 and W2 
    to be able to distinguish the left and right side of the join. 
    
    You can also use these kinds of aliases in other queries 
    to save some typing, e.g.:

为了区分位于连接操作左边的表格和右边的表格，
上面的查询将 ``weather`` 表格重新命名为了 ``W1`` 和 ``W2`` 。
用户在执行其他查询的时候，
也可以通过这种方法去减少需要输入的字符，
比如这样：

::

    SELECT *
        FROM weather w, cities c
            WHERE w.city = c.name;

..
    You will encounter this style of abbreviating quite frequently.

我们将会经常看到这种类型的简化。


聚合函数
-----------------

Like most other relational database products, PostgreSQL supports aggregate functions. An aggregate function computes a single result from multiple input rows. For example, there are aggregates to compute the count, sum, avg (average), max (maximum) and min (minimum) over a set of rows.

As an example, we can find the highest low-temperature reading anywhere with:

::

    SELECT max(temp_lo) FROM weather;

::

      max
     -----
       46
     (1 row)

If we wanted to know what city (or cities) that reading occurred in, 
we might try:

::

    SELECT city FROM weather WHERE temp_lo = max(temp_lo);     WRONG

but this will not work 
since the aggregate max cannot be used in the WHERE clause. 
(This restriction exists because the WHERE clause determines which rows will be included in the aggregate calculation; so obviously it has to be evaluated before aggregate functions are computed.) 
However, 
as is often the case the query can be restated to accomplish the desired result, 
here by using a subquery:

::

    SELECT city FROM weather
        WHERE temp_lo = (SELECT max(temp_lo) FROM weather);

::

         city
    ---------------
     San Francisco
    (1 row)

This is OK because the subquery is an independent computation that computes its own aggregate separately from what is happening in the outer query.

Aggregates are also very useful in combination with GROUP BY clauses. For example, we can get the maximum low temperature observed in each city with:

::

    SELECT city, max(temp_lo)
        FROM weather
        GROUP BY city;

::

         city      | max
    ---------------+-----
     Hayward       |  37
     San Francisco |  46
    (2 rows)

which gives us one output row per city. Each aggregate result is computed over the table rows matching that city. We can filter these grouped rows using HAVING:

::

    SELECT city, max(temp_lo)
        FROM weather
        GROUP BY city
        HAVING max(temp_lo) < 40;

::

      city   | max
    ---------+-----
     Hayward |  37
    (1 row)

which gives us the same results for only the cities that have all temp_lo values below 40. Finally, if we only care about cities whose names begin with "S", we might do:

::

    SELECT city, max(temp_lo)
        FROM weather
        WHERE city LIKE 'S%'(1)
        GROUP BY city
        HAVING max(temp_lo) < 40;

.. note::

    The LIKE operator does pattern matching and is explained in Section 9.7.

It is important to understand the interaction between aggregates and SQL's WHERE and HAVING clauses. The fundamental difference between WHERE and HAVING is this: WHERE selects input rows before groups and aggregates are computed (thus, it controls which rows go into the aggregate computation), whereas HAVING selects group rows after groups and aggregates are computed. Thus, the WHERE clause must not contain aggregate functions; it makes no sense to try to use an aggregate to determine which rows will be inputs to the aggregates. On the other hand, the HAVING clause always contains aggregate functions. (Strictly speaking, you are allowed to write a HAVING clause that doesn't use aggregates, but it's seldom useful. The same condition could be used more efficiently at the WHERE stage.)

In the previous example, we can apply the city name restriction in WHERE, since it needs no aggregate. This is more efficient than adding the restriction to HAVING, because we avoid doing the grouping and aggregate calculations for all rows that fail the WHERE check.


更新
---------

..
    You can update existing rows using the UPDATE command. 
    Suppose you discover the temperature readings are all off by 2 degrees after November 28. 
    You can correct the data as follows:

用户可以通过 ``UPDATE`` 命令对已存在的行进行更新。
假设你想将 11 月 28 日之后的所有天气记录的温度都降低 2 度，
那么可以执行以下语句：

::

    UPDATE weather
        SET temp_hi = temp_hi - 2,  temp_lo = temp_lo - 2
        WHERE date > '1994-11-28';

..
    Look at the new state of the data:

以下展示的是更新之后的数据：

::

    SELECT * FROM weather;

         city      | temp_lo | temp_hi | prcp |    date
    ---------------+---------+---------+------+------------
     San Francisco |      46 |      50 | 0.25 | 1994-11-27
     San Francisco |      41 |      55 |    0 | 1994-11-29
     Hayward       |      35 |      52 |      | 1994-11-29
    (3 rows)


删除
--------------

Rows can be removed from a table using the DELETE command. Suppose you are no longer interested in the weather of Hayward. Then you can do the following to delete those rows from the table:

::

    DELETE FROM weather WHERE city = 'Hayward';

All weather records belonging to Hayward are removed.

::

    SELECT * FROM weather;

::

         city      | temp_lo | temp_hi | prcp |    date
    ---------------+---------+---------+------+------------
     San Francisco |      46 |      50 | 0.25 | 1994-11-27
     San Francisco |      41 |      55 |    0 | 1994-11-29
    (2 rows)

One should be wary of statements of the form

::

    DELETE FROM tablename;

Without a qualification, DELETE will remove all rows from the given table, leaving it empty. The system will not request confirmation before doing this!
