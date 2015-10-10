高级特性
=================

引言
-----------

..
    In the previous chapter 
    we have covered the basics of using SQL 
    to store and access your data in PostgreSQL. 

    We will now discuss some more advanced features of SQL 
    that simplify management and prevent loss or corruption of your data. 

    Finally, 
    we will look at some PostgreSQL extensions.

前面的章节介绍了如何使用基本的 SQL 语句去储存和访问 PostgreSQL 中的数据。
本章将对 SQL 的一些高级特性进行介绍，
这些特性可以简化用户对数据的管理操作，
并防止数据出现丢失或者错误。

..
    This chapter will on occasion refer to examples found in Chapter 2 
    to change or improve them, 
    so it will be useful to have read that chapter. 
    
    Some examples from this chapter can also be found in advanced.sql in the tutorial directory. 
    This file also contains some sample data to load, 
    which is not repeated here. 
    (Refer to Section 2.1 for how to use the file.)

本章偶尔会用到一些在本文档第 2 章中介绍过的例子，
并对它们进行修改，
又或者改进它们，
所以读者最好已经阅读过本文档的第 2 章了。

本章用到的某些例子也可以在 ``tutorial`` 文件夹中的 ``advanced.sql`` 文件里面找到。
除此之外，
``advanced.sql`` 文件还包含了一些示例数据，
读者可以通过载入这个文件来直接使用这些数据，
因此本章将不再展示输入这些数据的过程。
（文件的载入方法可以在本文档的 2.1 节看到。）


视图（view）
--------------------

..
    Refer back to the queries in Section 2.6. 

    Suppose the combined listing of weather records and city location is of particular interest to your application, 
    but you do not want to type the query each time you need it. 

    You can create a *view* over the query, 
    which gives a name to the query 
    that you can refer to like an ordinary table:

让我们重新回顾本文档 2.6 节提到过的例子。
假设对于程序来说，
同时列出天气记录以及相关联的地理位置是一个特别重要的操作，
但你可能并不想在每次用到这个操作的时候都重新输入整个查询语句，
这时你就可以为那个查询创建一个\ *视图*\ 。
视图允许用户为一个查询关联一个名字，
并通过给定名字来引用与之相关联的查询，
就像这样：

::

    CREATE VIEW myview AS
        SELECT city, temp_lo, temp_hi, prcp, date, location
            FROM weather, cities
            WHERE city = name;

    SELECT * FROM myview;

..
    Making liberal use of views 
    is a key aspect of good SQL database design. 

    Views allow you to encapsulate the details of the structure of your tables, 
    which might change as your application evolves, 
    behind consistent interfaces.

大量使用视图是优秀的 SQL 数据库设计的关键部分。
视图允许用户将表格结构的相关细节隐藏起来，
使得应用程序即使在不断演进而导致表格结构不断变更的情况下，
仍然能够提供始终如一的接口。

..
    Views can be used in almost any place a real table can be used. 
    Building views upon other views is not uncommon.

几乎所有可以使用表格地方，
都可以使用视图去代替表格。
基于其他视图去构建新视图的做法并不少见。


外键（foreign key）
------------------------

Recall the weather and cities tables from Chapter 2. Consider the following problem: You want to make sure that no one can insert rows in the weather table that do not have a matching entry in the cities table. This is called maintaining the referential integrity of your data. In simplistic database systems this would be implemented (if at all) by first looking at the cities table to check if a matching record exists, and then inserting or rejecting the new weather records. This approach has a number of problems and is very inconvenient, so PostgreSQL can do this for you.

The new declaration of the tables would look like this:

::

    CREATE TABLE cities (
            city     varchar(80) primary key,
            location point
    );

    CREATE TABLE weather (
            city      varchar(80) references cities(city),
            temp_lo   int,
            temp_hi   int,
            prcp      real,
            date      date
    );

Now try inserting an invalid record:

::

    INSERT INTO weather VALUES ('Berkeley', 45, 53, 0.0, '1994-11-28');

::

    ERROR:  insert or update on table "weather" violates foreign key constraint "weather_city_fkey"
    DETAIL:  Key (city)=(Berkeley) is not present in table "cities".

The behavior of foreign keys can be finely tuned to your application. We will not go beyond this simple example in this tutorial, but just refer you to Chapter 5 for more information. Making correct use of foreign keys will definitely improve the quality of your database applications, so you are strongly encouraged to learn about them.
