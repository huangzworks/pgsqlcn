数据定义
==============


表格基础知识
----------------------

..
    A table in a relational database is much like a table on paper: 
    It consists of rows and columns. 

    The number and order of the columns is fixed, 
    and each column has a name. 

    The number of rows is variable — 
    it reflects how much data is stored at a given moment. 

    SQL does not make any guarantees about the order of the rows in a table. 

    When a table is read, 
    the rows will appear in an unspecified order, 
    unless sorting is explicitly requested. 
    This is covered in Chapter 7. 

    Furthermore, 
    SQL does not assign unique identifiers to rows, 
    so it is possible to have several completely identical rows in a table. 

    This is a consequence of the mathematical model that underlies SQL 
    but is usually not desirable. 

    Later in this chapter we will see how to deal with this issue.

关系式数据库中的表格和纸上的表格非常相似：
它们都包含着行和列。
其中列的数量和排列顺序都是固定的，
并且每个列都有自己的名字；
而行的数量则是可变动的，
它取决于给定时刻上被储存的数据数量。
SQL 并不保证表格中行的排列顺序。
当一个表格被读取的时候，
除非用户显式地使用了排序操作，
否则行将以未经指定的顺序出现，
本文档的第 7 章将对这方面做更多的说明。
此外，
因为 SQL 并不会向行分配唯一标识符，
所以同一个表格里面可能会出现完全相同的行。
这个问题是由 SQL 底层的数学模型造成的，
但我们通常并不想表格里面出现完全相同的行，
本章稍后的内容将介绍如何解决这个问题。

..
    Each column has a data type. 

    The data type constrains the set of possible values 
    that can be assigned to a column 
    and assigns semantics to the data stored in the column 
    so that it can be used for computations. 

    For instance, 
    a column declared to be of a numerical type 
    will not accept arbitrary text strings, 
    and the data stored in such a column 
    can be used for mathematical computations. 

    By contrast, 
    a column declared to be of a character string type 
    will accept almost any kind of data 
    but it does not lend itself to mathematical calculations, 
    although other operations such as string concatenation are available.

每个列都有自己的数据类型。
数据类型限制了可以赋予给列的可用值，
并为储存在列里面的数据赋予了语义，
使得这些数据可以被用于执行计算。
比如说，
一个被声明为由数字类型组成的列是无法储存任何文本字符串（text strings）的，
并且储存在这个列里面的数据可以用于进行数学计算。
与此相反，
如果一个列声明自己是由文字字符串（character string）类型组成的，
那么它将接受几乎任何类型的数据，
不过这个列将无法执行数学计算，
但是诸如字符串拼接这样的操作倒是没有问题的。

..
    PostgreSQL includes a sizable set of built-in data types 
    that fit many applications. 

    Users can also define their own data types. 

    Most built-in data types have obvious names and semantics,
    so we defer a detailed explanation to Chapter 8. 

    Some of the frequently used data types are integer for whole numbers, 
    numeric for possibly fractional numbers, 
    text for character strings, 
    date for dates, 
    time for time-of-day values, 
    and timestamp for values containing both date and time.

PostgreSQL 提供了数量可观的一系列内置数据类型，
这些数据类型可以适用于非常多应用程序。
除此之外，
用户也可以定义他们自己的数据类型。
大多数内置数据类型都有明显的名字和语义，
本文档将在第 8 章对这些内置类型进行详细的介绍。
其中最常用一些数据类型包括表示数字的 ``integer`` 类型，
表示分数的 ``numeric`` 类型，
表示文字字符串的的 ``text`` 类型，
表示日期的 ``date`` 类型，
表示一天中的时间的 ``time`` 类型，
以及包含着日期和时间两个值的 ``timestamp`` 类型。

..
    To create a table, 
    you use the aptly named CREATE TABLE command. 
    In this command you specify at least a name for the new table, 
    the names of the columns and the data type of each column. 
    For example:

通过使用 ``CREATE TABLE`` 命令，
用户可以创建一个新的表格。
在调用 ``CREATE TABLE`` 命令创建一个新表格时，
用户至少需要指定新表格的名字、各个列的名字以及各个列的数据类型，
就像这样：

::

    CREATE TABLE my_first_table (
        first_column text,
        second_column integer
    );

..
    This creates a table named my_first_table with two columns. 
    The first column is named first_column and has a data type of text; 
    the second column has the name second_column and the type integer. 
    The table and column names follow the identifier syntax explained in Section 4.1.1. 
    The type names are usually also identifiers, 
    but there are some exceptions. 
    Note that the column list is comma-separated 
    and surrounded by parentheses.

这个命令将创建一个名为 ``my_first_table`` 的表格，
它包含两个列：
第一个列名为 ``first_column`` ，
数据类型为 ``text`` ；
而第二个列则名为 ``second_column`` ，
数据类型为 ``integer`` 。

表格的名字和列的名字需要遵循 4.1.1 节介绍的标识符语法。
类型名字通常也是标识符，
但并非所有类型名字也是如此。
最后需要注意的是，
列的定义由括号包围，
并且各个列之间需要用逗号隔开。

..
    Of course, 
    the previous example was heavily contrived. 

    Normally, 
    you would give names to your tables and columns 
    that convey what kind of data they store. 

    So let's look at a more realistic example:

上面的表格定义示例有非常浓重的虚构成分。
在一般情况下，
我们都会根据表格和列储存的数据的种类来对它们进行命名。
以下是一个看上去更为真实的表格定义示例：

::

    CREATE TABLE products (
        product_no integer,
        name text,
        price numeric
    );

..
    (The numeric type can store fractional components, 
    as would be typical of monetary amounts.)

（\ ``numeric`` 类型可以储存分数，
这种数字类型通常被用于表示金钱的数量。）

..
    .. tip:: When you create many interrelated tables it is wise to choose a consistent naming pattern for the tables and columns. For instance, there is a choice of using singular or plural nouns for table names, both of which are favored by some theorist or other.

..
    There is a limit on how many columns a table can contain. 
    Depending on the column types, 
    it is between 250 and 1600. 
    However, 
    defining a table with anywhere near this many columns is highly unusual 
    and often a questionable design.

一个表格能够包含的列是有限的：
根据列的类型不同，
一个表格最多能够包含的列将介于 250 个至 1600 个之间。
不过，
定义一个包含这么多列的表格并不常见，
而且这通常意味着一个有问题的设计。

..
    If you no longer need a table, 
    you can remove it using the DROP TABLE command. 
    For example:

当用户不在需要某个表格时，
他可以使用 ``DROP TABLE`` 命令去移除它。
就像这样：

::

    DROP TABLE my_first_table;
    DROP TABLE products;

..
    Attempting to drop a table that does not exist is an error. 

    Nevertheless, 
    it is common in SQL script files to unconditionally try to drop each table before creating it, 
    ignoring any error messages, 
    so that the script works whether or not the table exists. 

    (If you like, 
    you can use the DROP TABLE IF EXISTS variant to avoid the error messages, 
    but this is not standard SQL.)

尝试删除一个不存在的表格将引发一个错误。
不过 SQL 脚本文件常常会在创建表格之前，
先删除可能存在的同名表格，
并忽略由此产生的所有错误信息，
从而保证脚本无论在表格是否存在的情况下都能够正常运作。
（如果有需要的话，
读者可以使用 ``DROP TABLE`` 的 ``DROP TABLE IF EXISTS`` 变种去避免引发错误，
但这个变种并非标准的 SQL 命令。）

..
    If you need to modify a table that already exists, 
    see Section 5.5 later in this chapter.

如果你需要修改一个已经存在的表格，
那么请看本章稍后 5.5 节介绍的内容。

..
    With the tools discussed so far 
    you can create fully functional tables. 

    The remainder of this chapter 
    is concerned with adding features to the table definition 
    to ensure data integrity, security, or convenience. 

    If you are eager to fill your tables with data now 
    you can skip ahead to Chapter 6 
    and read the rest of this chapter later.

通过使用上面介绍的工具，
读者现在已经可以创建出能够正常运作的表格了。
本章余下的内容将要介绍的是如何通过给表格定义添加额外的特性，
从而确保数据的完整性和安全性，
又或者仅仅为了让表格变得更加易于使用。
如果你现在就急切地想要学习使用数据去填充表格的方法，
那么可以先去阅读本文档的第 6 章，
之后再转过头来阅读本章剩余的内容。


默认值
-------------------

..
    A column can be assigned a default value. 

    When a new row is created 
    and no values are specified for some of the columns, 
    those columns will be filled with their respective default values. 

    A data manipulation command can also request explicitly 
    that a column be set to its default value, 
    without having to know what that value is. 

    (Details about data manipulation commands are in Chapter 6.)

在 PostgreSQL 中，
用户可以为列赋予一个默认值。
当用户创建一个新行，
但是并没有为其中的某些列指定值的时候，
那些列就会被赋予它们相应的默认值。
用户也可以通过数据处理命令（data manipulation command），
明确地将一个列设置为它的默认值，
并且不需要知道哪个默认值是什么。
用户还可以通过数据处理命令（data manipulation command），
明确地为一个列设置默认值，
并且不需要知道这个默认值到底是什么。
（本文档的第 6 章将对数据处理命令做更详细的介绍。）

..
    If no default value is declared explicitly, 
    the default value is the null value. 
    This usually makes sense 
    because a null value can be considered to represent unknown data.

如果一个列没有设置默认值，
那么它的默认值就是控制。
这种行为一般都是有意义的，
因为一个空值可以用于表示未知的数据。

..
    In a table definition, 
    default values are listed after the column data type. 
    For example:

在一个表格定义里面，
默认值可以在列的数据类型之后列出。
就像这样：

::

    CREATE TABLE products (
        product_no integer,
        name text,
        price numeric DEFAULT 9.99
    );

..
    The default value can be an expression, 
    which will be evaluated whenever the default value is inserted 
    (not when the table is created). 

    A common example is for a timestamp column to have a default of ``CURRENT_TIMESTAMP`` , 
    so that it gets set to the time of row insertion. 
    Another common example is generating a "serial number" for each row. 

    In PostgreSQL this is typically done by something like:

默认值除了可以是常量之外，
还可以是一个表达式。
表达式默认值将在行插入到表格时被求值（而不是在创建表格时被求值）。
一个比较常见的例子就是，
通过将 ``CURRENT_TIMESTAMP`` 设置为 ``timestamp`` 列的默认值，
我们可以让行在插入到表格时，
将 ``timestamp`` 列的值设置为当时的时间戳。
使用表达式作为默认值的另一个常见例子，
是使用默认值去为每个行创建序列号（serial number），
这种行为可以通过以下代码来实现：

::

    CREATE TABLE products (
        product_no integer DEFAULT nextval('products_product_no_seq'),
        ...
    );

..
    where the ``nextval()`` function supplies successive values from a sequence object (see Section 9.16). 
    This arrangement is sufficiently common 
    that there's a special shorthand for it:

其中 ``nextval()`` 函数负责从序列对象（参见 9.16 节）中获取下一个序列号。
因为这种做法非常常见，
所以 PostgreSQL 为其提供了一种等效的快捷方式：

::

    CREATE TABLE products (
        product_no SERIAL,
        ...
    );

..
    The ``SERIAL`` shorthand is discussed further in Section 8.1.4.

本文档的 8.1.4 节将介绍更多关于 ``SERIAL`` 的信息。
