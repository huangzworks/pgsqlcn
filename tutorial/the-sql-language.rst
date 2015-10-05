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
