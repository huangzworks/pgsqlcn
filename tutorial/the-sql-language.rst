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
