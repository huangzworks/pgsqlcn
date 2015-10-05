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
