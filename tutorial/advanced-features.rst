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
