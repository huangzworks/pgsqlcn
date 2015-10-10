.. PostgreSQL数据库文档（中文版） documentation master file, created by
   sphinx-quickstart on Fri Oct  2 22:11:25 2015.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

PostgreSQL数据库文档（中文版）
==========================================================

This book is the official documentation of PostgreSQL. 
It has been written by the PostgreSQL developers and other volunteers 
in parallel to the development of the PostgreSQL software. 
It describes all the functionality that the current version of PostgreSQL officially supports.

To make the large amount of information about PostgreSQL manageable, 
this book has been organized in several parts. 
Each part is targeted at a different class of users, 
or at users in different stages of their PostgreSQL experience:

- Part I is an informal introduction for new users.

- Part II documents the SQL query language environment, including data types and functions, as well as user-level performance tuning. Every PostgreSQL user should read this.

- Part III describes the installation and administration of the server. Everyone who runs a PostgreSQL server, be it for private use or for others, should read this part.

- Part IV describes the programming interfaces for PostgreSQL client programs.

- Part V contains information for advanced users about the extensibility capabilities of the server. Topics include user-defined data types and functions.

- Part VI contains reference information about SQL commands, client and server programs. This part supports the other parts with structured information sorted by command or program.

- Part VII contains assorted information that might be of use to PostgreSQL developers.

前言
---------------

.. toctree::
   :maxdepth: 1

   preface/what-is-postgresql

第 1 部分：使用教程
----------------------

..
    Welcome to the PostgreSQL Tutorial. 
    The following few chapters are intended to give a simple introduction to PostgreSQL, 
    relational database concepts, 
    and the SQL language 
    to those who are new to any one of these aspects. 

    We only assume some general knowledge about how to use computers. 
    No particular Unix or programming experience is required. 

欢迎阅读 PostgreSQL 使用教程！
本教程包含三个章节，
这些章节旨在帮助读者了解关于 PostgreSQL、关系式数据库概念以及 SQL 语言的一些基本知识。

本教程只要求读者具备基本的电脑使用常识，
但并不要求读者有 Unix 使用经验或者编程经验。

..
    This part is mainly intended to give you some hands-on experience 
    with important aspects of the PostgreSQL system. 
    It makes no attempt to be a complete or thorough treatment of the topics it covers.

因为这个教程的主要目的在于向读者传授一些 PostgreSQL 系统关键部分的使用经验，
所以这个教程并不会对某些主题进行完整和深入的讨论，
这些讨论将留给文档后续的其他部分来进行。

.. toctree::
   :maxdepth: 2

   tutorial/getting-started
   tutorial/the-sql-language
   tutorial/advanced-features

第 2 部分：SQL 语言
------------------------------

This part describes the use of the SQL language in PostgreSQL. We start with describing the general syntax of SQL, then explain how to create the structures to hold data, how to populate the database, and how to query it. The middle part lists the available data types and functions for use in SQL commands. The rest treats several aspects that are important for tuning a database for optimal performance.

The information in this part is arranged so that a novice user can follow it start to end to gain a full understanding of the topics without having to refer forward too many times. The chapters are intended to be self-contained, so that advanced users can read the chapters individually as they choose. The information in this part is presented in a narrative fashion in topical units. Readers looking for a complete description of a particular command should see Part VI.

Readers of this part should know how to connect to a PostgreSQL database and issue SQL commands. Readers that are unfamiliar with these issues are encouraged to read Part I first. SQL commands are typically entered using the PostgreSQL interactive terminal psql, but other programs that have similar functionality can be used as well.

.. toctree::
   :maxdepth: 2

   sql/ddl
   sql/dml
   sql/datatype
