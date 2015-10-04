入门须知
===============

安装 PostgreSQL
------------------------

..
    Before you can use PostgreSQL 
    you need to install it, 
    of course. 
    It is possible that PostgreSQL is already installed at your site, 
    either because it was included in your operating system distribution 
    or because the system administrator already installed it. 
    If that is the case, 
    you should obtain information 
    from the operating system documentation or your system administrator 
    about how to access PostgreSQL.

在使用 PostgreSQL 之前，
我们必须确保 PostgreSQL 已经被安装到了电脑上面。
在有些情况下，
某些操作系统发行版可能已经自带了 PostgreSQL ，
又或者你的系统管理员可能已经帮你安装好了 PostgreSQL ，
这时你就需要从操作系统手册或者系统管理员那里获取访问 PostgreSQL 所需的信息。

..
    If you are not sure whether PostgreSQL is already available 
    or whether you can use it for your experimentation 
    then you can install it yourself. 
    Doing so is not hard and it can be a good exercise. 
    PostgreSQL can be installed by any unprivileged user; 
    no superuser (root) access is required.

如果你无法确认 PostgreSQL 是否已经安装，
又或者不确定已有的 PostgreSQL 能否满足你的实验要求，
那么你可以自己动手安装 PostgreSQL 。
安装 PostgreSQL 并不困难，
并且将是一个非常好的练习。
安装 PostgreSQL 并不需要超级用户（superuser，root）访问权限，
所以任何非特权（unprivileged）用户都可以自行安装 PostgreSQL 。

..
    If you are installing PostgreSQL yourself, 
    then refer to Chapter 15 for instructions on installation, 
    and return to this guide when the installation is complete. 
    Be sure to follow closely the section about setting up the appropriate environment variables.

如果你需要自行安装 PostgreSQL ，
那么请查看本文档第 15 章中介绍的安装方法，
并在安装完成之后再转过头来阅读这一章。
在阅读安装方法的过程中，
请特别注意文中关于设置正确的环境变量的那一节。

..
    If your site administrator has not set things up in the default way, 
    you might have some more work to do. 
    For example, 
    if the database server machine is a remote machine, 
    you will need to set the PGHOST environment variable to the name of the database server machine. 
    The environment variable PGPORT might also have to be set. 

    The bottom line is this: 
    if you try to start an application program 
    and it complains that it cannot connect to the database, 
    you should consult your site administrator or, 
    if that is you, 
    the documentation to make sure that your environment is properly set up. 
    If you did not understand the preceding paragraph then read the next section.

如果系统管理员没有按照默认的方法对 PostgreSQL 进行设置，
那么你可能就需要做一些额外的工作。
比如说，
如果数据库服务器是一个远程服务器，
那么你就需要将 ``PGHOST`` 环境变量设置为那个数据库服务器的名字，
并将 ``PGHOST`` 环境变量设置为那个服务器的端口号。

关于服务器设置的一个底线是：
如果你在尝试启动一个应用程序的时候，
该程序向你抱怨它无法连接到服务器，
那么你就应该联系你的管理员；
如果你的服务器是由你自己管理的，
那么你就应该去阅读相关的文档，
确保相关的环境已经被正确地设置。


基本架构
----------------------

..
    Before we proceed, 
    you should understand the basic PostgreSQL system architecture. 
    Understanding how the parts of PostgreSQL interact will make this chapter somewhat clearer.

在学习如何操作 PostgreSQL 之前，
我们需要先了解一下 PostgreSQL 系统的基本架构。
理解 PostgreSQL 的各个部分是如何进行交互的，
将有助于我们学习本章接下来要介绍的知识。

..
    In database jargon, 
    PostgreSQL uses a client/server model. 
    A PostgreSQL session consists of the following cooperating processes (programs):

用数据库的行话来讲，
PostgreSQL 使用的是客户端/服务器模型。
一个 PostgreSQL 会话（session）由以下协作进程（程序）组成：

..
    - A server process, which manages the database files, accepts connections to the database from client applications, and performs database actions on behalf of the clients. The database server program is called postgres.

- 一个服务器进程，
  它负责管理数据库文件、接受客户端程序对数据库的连接请求，
  并对数据库执行客户端指定的动作。
  这个数据库服务器程序的名字叫做 ``postgres`` 。

..
    - The user's client (frontend) application 
    that wants to perform database operations. 
    Client applications can be very diverse in nature: 
    a client could be a text-oriented tool, 
    a graphical application, 
    a web server that accesses the database to display web pages, 
    or a specialized database maintenance tool. 
    Some client applications are supplied with the PostgreSQL distribution; 
    most are developed by users.

- 客户端（又称“前端”）程序，
  用户使用这些程序来指定对数据库的操作。
  客户端程序在本质上可以是非常多样的：
  一个客户端可以是一个文本工具、
  一个图形程序、
  一个通过访问数据库来显示网页的 web 服务器，
  又或者是一个专门的数据库维护工具。
  某些客户端程序是跟着 PostgreSQL 一起发行的，
  而其他大部分客户端则是由用户自己开发的。

..
    As is typical of client/server applications, 
    the client and the server can be on different hosts. 
    In that case they communicate over a TCP/IP network connection. 
    You should keep this in mind, 
    because the files that can be accessed on a client machine might not be accessible 
    (or might only be accessible using a different file name) 
    on the database server machine.

跟典型的客户端/服务器程序一样，
PostgreSQL 的客户端和服务器双方可以运行在不同的地址（host）上面。
在这种情况下，
客户端和服务器将通过 TCP/IP 网络连接进行通讯。
读者应该牢牢记住这一点，
因为在客户端机器上面能够访问的文件，
在服务器机器上面并不一定能够访问
（又或者需要以不同的名字来访问）。

..
    The PostgreSQL server can handle multiple concurrent connections from clients. 
    To achieve this it starts ("forks") a new process for each connection. 
    From that point on, 
    the client and the new server process communicate without intervention by the original postgres process. 
    Thus, 
    the master server process is always running, 
    waiting for client connections, 
    whereas client and associated server processes come and go. 
    (All of this is of course invisible to the user. We only mention it here for completeness.)

PostgreSQL 服务器能够并发地处理来自多个客户端的多个连接。
为了做到这一点，
服务器会为每个连接都启动（或者说，fork）一个新的进程。
在此之后，
客户端和新进程就可以直接进行通讯，
而不必担心被原来的 ``postgres`` 进程打扰。
因此，
主服务器进程将一直运行，
等待客户端的连接请求；
与此相反，
客户端和与之相关联的服务器进程则会不断地出现和消逝。
（当然，
以上这些操作对于用户来说都是透明的，
在这里提及这些知识只是为了完整性考虑。）


创建数据库
------------------

..
    The first test to see whether you can access the database server 
    is to try to create a database. 
    A running PostgreSQL server can manage many databases. 
    Typically, 
    a separate database is used for each project 
    or for each user.

为了判断我们能否访问数据库服务器，
我们首先要做的一个测试就是尝试去创建一个数据库。
一个运行中的 PostgreSQL 服务器可以管理多个数据库。
一般来说，
每个用户都会为自己的每个项目独立使用一个数据库。

..
    Possibly, 
    your site administrator has already created a database for your use. 
    He should have told you what the name of your database is. 
    In that case you can omit this step and skip ahead to the next section.

如果你的系统管理员已经帮你创建好了数据库，
并且他已经把属于你的数据库的名字告诉了你，
那么你就可以跳过本节介绍的步骤，
直接阅读下一节。

..
    To create a new database, 
    in this example named mydb, 
    you use the following command:

举个例子，
要创建一个名为 ``mydb`` 的新数据库，
你可以使用以下命令：

.. code-block:: bash

    $ createdb mydb

..
    If this produces no response then this step was successful 
    and you can skip over the remainder of this section.

如果这个操作没有返回任何响应信息，
那么说明操作成功，
请跳过本节余下的部分，
直接阅读下一节。

..
    If you see a message similar to:

如果你看见了一条类似这样的信息：

.. code-block:: bash

    createdb: command not found

..
    then PostgreSQL was not installed properly. 
    Either it was not installed at all 
    or your shell's search path was not set to include it. 
    Try calling the command with an absolute path instead:

那么说明 PostgreSQL 并未被正确地安装。
一个可能是 PostgreSQL 并未被安装，
而另一个可能则是终端没有在搜索路径里面包含 PostgreSQL 所在的位置。
你可以尝试通过绝对路径直接执行命令：

.. code-block:: bash

    $ /usr/local/pgsql/bin/createdb mydb

..
    The path at your site might be different. 
    Contact your site administrator 
    or check the installation instructions to correct the situation.

在不同的机器上面，
PostgreSQL 所在的路径可能会有所不同。
请联系你的系统管理员，
又或者根据安装手册的指示改正执行命令时的路径。

..
    Another response could be this:

执行 ``createdb`` 命令可能得到的另一个回复是：

.. code-block:: bash

    createdb: could not connect to database postgres: could not connect to server: No such file or directory
            Is the server running locally and accepting
            connections on Unix domain socket "/tmp/.s.PGSQL.5432"?

..
    This means that the server was not started, 
    or it was not started where createdb expected it. 
    Again, 
    check the installation instructions or consult the administrator.

这个回复表示服务器并未启动，
又或者它并未以 ``createdb`` 期待的方式启动。
同样地，
在遇到这个问题时，
请检查安装手册，
或者联系你的系统管理员。

..
    Another response could be this:

除了以上两种情况之外，
``createdb`` 还可能会回复一条包含了你的 PostgreSQL 用户名的错误信息：

.. code-block:: bash

    createdb: could not connect to database postgres: FATAL:  role "joe" does not exist

..
    where your own login name is mentioned. 
    This will happen if the administrator has not created a PostgreSQL user account for you. 
    (PostgreSQL user accounts are distinct from operating system user accounts.) 

    If you are the administrator, 
    see Chapter 20 for help creating accounts. 

    You will need to become the operating system user 
    under which PostgreSQL was installed (usually postgres)
    to create the first user account. 

    It could also be that 
    you were assigned a PostgreSQL user name 
    that is different from your operating system user name; 
    in that case 
    you need to use the -U switch or set the PGUSER environment variable 
    to specify your PostgreSQL user name.

当你的系统管理员没有为你创建 PostgreSQL 用户账号的时候，
这种回复就会出现。
（注意，
PostgreSQL 的用户账号和操作系统的用户账号并不是一回事。）

如果你就是系统管理员，
那么请查看本文档的第 20 章，
学习创建用户的具体步骤。
为了创建第一个 PostgreSQL 用户账号，
你将需要成为安装了 PostgreSQL 的那个操作系统用户组属下的用户（这个用户组通常被命名为 ``postgres`` ）。

还有一种可能出现的情况，
就是分配给你的 PostgreSQL 用户名和你的操作系统用户名并不相同；
在这种情况下，
你就需要使用 ``-U`` 去切换或者设置 ``PGUSER`` 环境变量，
从而指定你的 PostgreSQL 用户名。

..
    If you have a user account 
    but it does not have the privileges required to create a database, 
    you will see the following:

如果你拥有了一个账号，
但该账号并不具备创建数据库所需的权限，
那么你将看见以下信息：

.. code-block:: bash

    createdb: database creation failed: ERROR:  permission denied to create database

..
    Not every user has authorization to create new databases. 
    If PostgreSQL refuses to create databases for you 
    then the site administrator needs to grant you permission to create databases. 
    Consult your site administrator if this occurs. 
    If you installed PostgreSQL yourself 
    then you should log in for the purposes of this tutorial 
    under the user account that you started the server as.\ [#f1]_

如果你遇到了这种情况，
那么说明你并不具备创建新数据库所需的权限。
为此，
你需要联系系统管理员，
让他授予（grant）你创建数据库的权力。
如果你是自己安装 PostgreSQL 的，
那么为了执行这个教程里面展示的操作，
你需要登录启动 PostgreSQL 服务器时使用的那个账号。

..
    You can also create databases with other names. 
    PostgreSQL allows you to create any number of databases at a given site. 
    Database names must have an alphabetic first character 
    and are limited to 63 bytes in length. 
    A convenient choice is to create a database with the same name as your current user name. 
    Many tools assume that database name as the default, 
    so it can save you some typing. 
    To create that database, 
    simply type:

PostgreSQL 允许用户在同一台机器上面创建任意数量的数据库，
因此除了上面提到的 ``mydb`` 之外，
用户还可以创建其他不同名字的数据库。
数据库名字必须以英文字母开头，
并且长度不能超过 63 个字节。
一种常见的做法是创建一个与当前用户名字相同的数据库。
很多工具都会假设用户的名字就是数据库的名字，
并通过这一假设来减少用户需要输入的字符数量。
要创建一个与用户名同名的数据库，
用户只需要直接执行以下命令就可以了：

.. code-block:: bash

    $ createdb

..
    If you do not want to use your database anymore you can remove it.
    For example, 
    if you are the owner (creator) of the database mydb, 
    you can destroy it using the following command:

当你不再想要使用数据库的时候，
你可以考虑移除它。
比如说，
如果你是 ``mydb`` 数据库的拥有者（创建者），
那么你可以使用以下命令来销毁它：

.. code-block:: bash

    $ dropdb mydb

..
    (For this command, 
    the database name does not default to the user account name. 
    You always need to specify it.)
    This action physically removes all files associated with the database and cannot be undone, 
    so this should only be done with a great deal of forethought.

``dropdb`` 命令将从物理上移除与 ``mydb`` 数据库有关的所有文件，
并且这个操作是无法撤销的，
因此你在执行 ``dropdb`` 的时候一定要三思而后行。
跟 ``createdb`` 命令一样，
如果被移除的数据库跟用户名同名，
那么用户只需要执行 ``dropdb`` 就可以直接删除与用户名同名的那个数据库。

..
    More about createdb and dropdb can be found in createdb and dropdb respectively.

要了解关于 ``createdb`` 命令和 ``dropdb`` 命令的更多信息，
请参考 `createdb <http://www.postgresql.org/docs/9.5/static/app-createdb.html>`_ 的文档和 `dropdb <http://www.postgresql.org/docs/9.5/static/app-dropdb.html>`_ 的文档。

.. 注释 1
    .. [#f1] As an explanation for why this works: PostgreSQL user names are separate from operating system user accounts. When you connect to a database, you can choose what PostgreSQL user name to connect as; if you don't, it will default to the same name as your current operating system account. As it happens, there will always be a PostgreSQL user account that has the same name as the operating system user that started the server, and it also happens that that user always has permission to create databases. Instead of logging in as that user you can also specify the -U option everywhere to select a PostgreSQL user name to connect as.


访问数据库
-------------

..
    Once you have created a database, you can access it by:

    - Running the PostgreSQL interactive terminal program, called **psql** , 
      which allows you to interactively enter, edit, and execute SQL commands.

    - Using an existing graphical frontend tool like pgAdmin 
      or an office suite with ODBC or JDBC support to create and manipulate a database. 
      These possibilities are not covered in this tutorial.

    - Writing a custom application, 
      using one of the several available language bindings. 
      These possibilities are discussed further in Part IV.

    You probably want to start up psql to try the examples in this tutorial. 
    It can be activated for the mydb database by typing the command:

在创建了数据库之后，
我们就可以通过以下几种方法去访问数据库：

- 运行名为 **psql** 的 PostgreSQL 交互终端程序，
  这个程序允许用户交互式地输入、编辑和执行 SQL 命令。

- 使用类似 pgAdmin 这样的图形前端工具，
  又或者带有 ODBC 或 JDBC 支持的 office 套件来创建和操作数据库。

- 在多种可用的语言绑定里面，
  选择一种去编写一个定制的程序。
  本文档的第 4 章列出了所有可选的语言绑定。

本教程将使用 ``psql`` 程序来展示如何执行数据库命令。
要通过 ``psql`` 程序来访问 ``mydb`` 数据库，
我们可以执行以下命令：

.. code-block:: bash

    $ psql mydb

..
    If you do not supply the database name then it will default to your user account name. 
    You already discovered this scheme in the previous section using createdb.

跟之前介绍的 ``createdb`` 和 ``dropdb`` 一样，
如果用户在执行 ``psql`` 程序的时候，
没有指定要访问的数据库，
那么 ``psql`` 将访问与用户的用户名同名的数据库。

..
    In ``psql`` , you will be greeted with the following message:

在进入 ``psql`` 程序之后，
你将看到以下信息：

.. code-block:: psql

    psql (9.5alpha2)
    Type "help" for help.

    mydb=>

..
    The last line could also be:

在某些情况下，
消息的最后一行也可能是这样的：

.. code-block:: psql

    mydb=#

..
    That would mean you are a database superuser, 
    which is most likely the case 
    if you installed PostgreSQL yourself. 
    Being a superuser means that you are not subject to access controls. 
    For the purposes of this tutorial that is not important.

这意味着你是数据库的超级用户。
如果你是自己亲自安装 PostgreSQL 的话，
那么你通常就会是这种超级用户。

成为超级管理员意味着你将不受制于访问控制规则，
但对于本教程要展示的内容来说，
用户是否超级管理员并不重要。

..
    If you encounter problems starting psql then go back to the previous section. 
    The diagnostics of createdb and psql are similar, 
    and if the former worked the latter should work as well.

如果你在启动 ``psql`` 程序的时候遇上了问题，
那么请阅读前一个小节。
因为 ``createdb`` 和 ``psql`` 可能遇上的问题都是相似的，
所以如果 ``createdb`` 能够成功运作，
那么 ``psql`` 一般也不会出现什么问题。

..
    The last line printed out by psql is the prompt, 
    and it indicates that psql is listening to you 
    and that you can type SQL queries into a work space maintained by psql. 
    Try out these commands:

``psql`` 打印的最后一行为提示符（prompt），
它意味着 ``psql`` 正在等候你的指示，
这时你就可以将 SQL 查询语句键入到 ``psql`` 维护的工作空间（work space）当中。
你可以尝试键入以下语句：

.. code-block:: psql

    mydb=> SELECT version();
    version
    -----------------------------------------------------------------------
    PostgreSQL 9.5alpha2 on i586-pc-linux-gnu, compiled by GCC 2.96, 32-bit
    (1 row)

    mydb=> SELECT current_date;
    date
    ------------
    2002-08-31
    (1 row)

    mydb=> SELECT 2 + 2;
    ?column?
    ----------
    4
    (1 row)

..
    The psql program has a number of internal commands that are not SQL commands. 
    They begin with the backslash character, "\". 
    For example, 
    you can get help on the syntax of various PostgreSQL SQL commands by typing:

``psql`` 程序有几个不属于 SQL 命令的内部命令，
这些命令都以反斜线符号 ``"\"`` 开头。
举个例子，
你可以通过输入以下命令来获得关于不同 PostgreSQL SQL 命令的语法帮助信息：

.. code-block:: psql

    mydb=> \h

..
    To get out of psql, type:

要退出 ``psql`` ，
你可以输入：

.. code-block:: psql

    mydb=> \q

..
    and psql will quit and return you to your command shell. 
    (For more internal commands, type \? at the psql prompt.)
    The full capabilities of psql are documented in psql. 
    In this tutorial we will not use these features explicitly, 
    but you can use them yourself when it is helpful.

然后 ``psql`` 就会退出并返回至你原来的命令行。
（要了解更多内部命令，
请在 ``psql`` 提示符中输入 ``\?`` 。）
`psql 的文档 <http://www.postgresql.org/docs/9.5/static/app-psql.html>`_ 记录了 ``psql`` 的各项特性，
这个文档并不会显式地使用这些特性，
但你可以在需要的时候自行使用它们。
