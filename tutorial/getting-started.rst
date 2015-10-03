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
