数据操作
==============

..
    The previous chapter discussed how to create tables and other structures to hold your data. 
    Now it is time to fill the tables with data. 
    This chapter covers how to insert, update, and delete table data. 
    The chapter after this will finally explain how to extract your long-lost data from the database.

前面的章节介绍了创建表格和其他结构的方法，
并说明了如何使用这些表格和结构去储存数据，
现在是时候考虑怎样使用数据去填充表格了。
本章将介绍插入、更新和删除表格数据的方法，
而之后的一章将介绍如何从表格里面获取已有的数据。

插入数据
--------------

..
    When a table is created, 
    it contains no data. 

    The first thing to do before a database can be of much use is to insert data. 

    Data is conceptually inserted one row at a time. 
    Of course you can also insert more than one row, 
    but there is no way to insert less than one row. 

    Even if you know only some column values, 
    a complete row must be created.

一个新创建的表格是不包含任何数据的。
为了让一个数据库能够发挥它的实际作用，
我们必须向数据库里面插入数据。
插入操作是以行为单位进行的：
尽管用户可以一次插入多个行，
但是却不能插入少于一个行的数据 ——
即使用户在执行插入操作时只给出了某些列的值，
数据库也会为这个插入操作创建一个完整的行。

..
    To create a new row, 
    use the INSERT command. 
    The command requires the table name and column values. 
    For example, 
    consider the products table from Chapter 5:

``INSERT`` 命令接受表格的名字和列的值作为参数，
并在表格里面创建一个新行。
举个例子，
对于第 5 章介绍的 ``products`` 表格来说：

::

    CREATE TABLE products (
        product_no integer,
        name text,
        price numeric
    );

..
    An example command to insert a row would be:

以下是一个使用 ``INSERT`` 命令去插入新行的例子：

::

    INSERT INTO products VALUES (1, 'Cheese', 9.99);

..
    The data values are listed in the order in which the columns appear in the table, 
    separated by commas. 
    Usually, 
    the data values will be literals (constants), 
    but scalar expressions are also allowed.

用户需要根据列在表格中的次序，
逐一列出每个数据值，
并在每个数据值之间使用逗号进行分割。
通常情况下，
数据值都是字面量（literals，也即是常量），
但使用标量（scalar）作为数据值也是可行的。

..
    The above syntax has the drawback 
    that you need to know the order of the columns in the table. 

    To avoid this 
    you can also list the columns explicitly. 

    For example, 
    both of the following commands have the same effect as the one above:

上面展示的 ``INSERT`` 语法有一个缺点，
它要求用户记住各个列在表格中的次序，
不过这个问题可以通过显式地列出各个列来解决。
举个例子，
以下两个命令将产生完全相同的效果：

::

    INSERT INTO products (product_no, name, price) VALUES (1, 'Cheese', 9.99);
    INSERT INTO products (name, price, product_no) VALUES ('Cheese', 9.99, 1);

..
    Many users consider it good practice to always list the column names.

很多用户都认为总是列出各个列的名字是一种更好的编程风格。

..
    If you don't have values for all the columns, 
    you can omit some of them. 

    In that case, 
    the columns will be filled with their default values. 
    
    For example:

如果你在插入行的时候，
缺少某个列的值，
那么你可以在执行 ``INSERT`` 命令时省略那些没有值的域。
在这种情况下，
那些没有被设置值的列将被默认值填充。
就像这样：

::

    INSERT INTO products (product_no, name) VALUES (1, 'Cheese');
    INSERT INTO products VALUES (1, 'Cheese');

..
    The second form is a PostgreSQL extension. 
    It fills the columns from the left with as many values as are given, 
    and the rest will be defaulted.

第二种语法是 PostgreSQL 的扩展语法：
这种语法会按照给定值的数量，
从左到右地填充行的各个列，
而那些没有给出值的行则使用默认值进行填充。

..
    For clarity, 
    you can also request default values explicitly, 
    for individual columns or for the entire row:

为了保证清晰性，
用户也可以显式地要求使用默认值去填充某个单独的列或者整个行：

::

    INSERT INTO products (product_no, name, price) VALUES (1, 'Cheese', DEFAULT);
    INSERT INTO products DEFAULT VALUES;

..
    You can insert multiple rows in a single command:

用户也可以在一条命令里面插入多个行：

::

    INSERT INTO products (product_no, name, price) VALUES
        (1, 'Cheese', 9.99),
        (2, 'Bread', 1.99),
        (3, 'Milk', 2.99);

..
    .. tip:: When inserting a lot of data at the same time, considering using the COPY command. It is not as flexible as the INSERT command, but is more efficient. Refer to Section 14.4 for more information on improving bulk loading performance.


更新数据
--------------

..
    The modification of data that is already in the database 
    is referred to as updating. 

    You can update individual rows, 
    all the rows in a table, 
    or a subset of all rows. 

    Each column can be updated separately; 
    the other columns are not affected.

对已经存在于数据库中的数据进行修改，
这一操作被称为更新操作。
用户可以更新表格中的某个行，
又或者所有行，
又或者其中一部分行。
每个列可以单独被更新，
其他列不会受到影响。

..
    To update existing rows, 
    use the UPDATE command. 
    This requires three pieces of information:

``UPDATE`` 命令可以对一个已经存在的行进行更新，
这个命令需要三方面的信息：

..
    1. The name of the table and column to update

    2. The new value of the column

    3. Which row(s) to update

1. 需要更新的表格的名字以及列的名字。

2. 设置给列的新值。

3. 指定需要更新的行。

..
    Recall from Chapter 5 that SQL does not, 
    in general, provide a unique identifier for rows. 

    Therefore it is not always possible to directly specify which row to update. 

    Instead, 
    you specify which conditions a row must meet in order to be updated. 

    Only if you have a primary key in the table 
    (independent of whether you declared it or not) 
    can you reliably address individual rows 
    by choosing a condition that matches the primary key. 

    Graphical database access tools rely on this fact 
    to allow you to update rows individually.

本文档的第 5 章曾经说过，
在通常情况下，
SQL 并不会为每个行都提供一个唯一标识符。
由于这个原因，
用户有时候并不能直接指定需要更新的行；
而是要指定一些条件，
并通过这些条件来决定表格中的哪些行将被更新。

只有当用户拥有了表格中某个行的主键时，
他才能够通过选择一个与主键相匹配的条件，
确实地对某个单独的行进行更新。
图形数据库访问工具通常会利用这个原理来实现单独更新某个列的功能。

..
    For example, 
    this command updates all products 
    that have a price of 5 to have a price of 10:

举个例子，
以下这个语句将从 ``products`` 表格里面找出所有 ``price`` 值为 ``5`` 的行，
并把它们的 ``price`` 值修改为 ``10`` ：

::

    UPDATE products SET price = 10 WHERE price = 5;

..
    This might cause zero, one, or many rows to be updated. 
    It is not an error to attempt an update that does not match any rows.

这个命令可能会造成零个、一个或者很多个行被更新。
执行一个没有实际效果的更新操作并不会引起错误。

..
    Let's look at that command in detail. 

    First is the key word ``UPDATE`` followed by the table name. 

    As usual, 
    the table name can be schema-qualified, 
    otherwise it is looked up in the path. 

    Next is the key word ``SET`` followed by the column name, 
    an equal sign, 
    and the new column value. 

    The new column value can be any scalar expression, 
    not just a constant. 

    For example, 
    if you want to raise the price of all products by 10% you could use:

让我们来详细地分析上面的这个命令。
首先，
跟在关键字 ``UPDATE`` 之后的是表格的名字。
As usual, 
the table name can be schema-qualified, 
otherwise it is looked up in the path. 
接着，
跟着关键字 ``SET`` 之后的是列的名字，
一个等号，
以及要设置给列的新值。
列的新值既可以是任何标量表达式（scalar expression），
也可以是一个常量。
比如说，
如果你想要将所有产品的价格都提高 10% ，
那么可以执行以下命令：

::

    UPDATE products SET price = price * 1.10;

..
    As you see, 
    the expression for the new value can refer to the existing value(s) in the row. 

    We also left out the ``WHERE`` clause. 

    If it is omitted, 
    it means that all rows in the table are updated. 

    If it is present, 
    only those rows that match the ``WHERE`` condition are updated. 

    Note that the equals sign in the ``SET`` clause is an assignment 
    while the one in the ``WHERE`` clause is a comparison, 
    but this does not create any ambiguity. 

    Of course, 
    the ``WHERE`` condition does not have to be an equality test. 

    Many other operators are available (see Chapter 9). 

    But the expression needs to evaluate to a Boolean result.

如你所见，
一个表达式可以通过引用行里面已有的值去构建新值。
上面展示的 ``UPDATE`` 命令都没有用到 ``WHERE`` 语句：
如果用户在执行 ``UPDATE`` 命令时没有给定 ``WHERE`` 语句，
那么表格中的所有行都将被更新；
另一方面，
如果用户给定了 ``WHERE`` 语句，
那么只有那些符合 ``WHERE`` 语句中指定条件的语句会被更新。
一个需要注意的地方是，
当等号出现在 ``SET`` 语句里面时，
它表示的是一个赋值操作；
而当等号出现在 ``WHERE`` 语句里面时，
它表示的是一次对比操作；
这两种操作是完全不同的两种操作。
另外需要注意的一点是，
虽然 ``WHERE`` 语句中的表达式需要计算出一个布尔值结果，
但这个表达式并不总是为一个相等测试条件：
除了等号之外，
PostgreSQL 还提供了很多操作符可供使用，
具体的信息将在本文档的第 9 章进行介绍。

..
    You can update more than one column in an UPDATE command 
    by listing more than one assignment in the SET clause. 
    For example:

通过在 ``SET`` 语句里面列出多个赋值等式，
用户可以在一条 ``UPDATE`` 命令里面对多个列进行更新。
就像这样：

::

    UPDATE mytable SET a = 5, b = 3, c = 1 WHERE a > 0;


删除数据
--------------

..
    So far we have explained how to add data to tables 
    and how to change data. 

    What remains is to discuss how to remove data 
    that is no longer needed. 

    Just as adding data is only possible in whole rows, 
    you can only remove entire rows from a table. 

    In the previous section 
    we explained that SQL does not provide a way to directly address individual rows. 

    Therefore, 
    removing rows can only be done 
    by specifying conditions that the rows to be removed have to match. 

    If you have a primary key in the table 
    then you can specify the exact row. 

    But you can also remove groups of rows matching a condition, 
    or you can remove all rows in the table at once.

到目前为止，
我们学习了将数据添加到表格里面的方法，
以及对表格里面的数据进行修改的方法，
那么我们剩下要学习的自然就是移除表格中无用数据的方法了。

就像添加数据时需要将整个行添加到表格里面一样，
要从表格里面移除数据，
我们也必须移除整个行。
上一节曾经提到过，
SQL 并没有提供能够直接访问某个单独的行的方法。
因此，
行的移除操作只能通过指定与被移除行相匹配的条件来完成。

如果你拥有表格中某个行的主键的话，
那么你可以通过那个主键来精确地移除那个行。
除此之外，
你也可以通过指定条件来删除一组与条件相匹配的行，
又或者直接一次性地移除表格中的所有行。

..
    You use the DELETE command to remove rows; 
    the syntax is very similar to the UPDATE command. 
    For instance, 
    to remove all rows from the products table that have a price of 10, use:

删除行需要用到 ``DELETE`` 命令，
这个命令的语法和 ``UPDATE`` 命令非常相似。
举个例子，
通过执行以下命令，
我们可以从 ``products`` 表格里面移除价格等于 ``10`` 的所有行：

::

    DELETE FROM products WHERE price = 10;

..
    If you simply write:

另一方面，
如果我们直接执行以下命令：

::

    DELETE FROM products;

..
    then all rows in the table will be deleted! 
    Caveat programmer.

那么 ``products`` 表格中的所有行都将被删除！
请谨慎地使用这个命令。
