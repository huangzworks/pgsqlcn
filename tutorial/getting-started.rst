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

基本架构知识
----------------------


