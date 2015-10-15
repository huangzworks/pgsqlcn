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

..
    Recall the weather and cities tables from Chapter 2. 
    
    Consider the following problem: 
    You want to make sure that no one can insert rows in the weather table 
    that do not have a matching entry in the cities table. 
    
    This is called maintaining the referential integrity of your data. 
    
    In simplistic database systems 
    this would be implemented (if at all) 
    by first looking at the cities table 
    to check if a matching record exists, 
    and then inserting or rejecting the new weather records. 
    
    This approach has a number of problems and is very inconvenient, 
    so PostgreSQL can do this for you.

回忆一下本文档第 2 章介绍过的 ``weather`` 表格和 ``cities`` 表格。
假设我们现在想要保证，
在将一个新行插入到 ``weather`` 表格的时候，
这个新行中提到的城市必须已经存在于 ``cities`` 表格，
这种机制通常被称为“数据的引用完整性管理机制”。
在一个非常简单的数据库系统中，
引用完整性可以通过以下方法来确保：
在尝试向 ``weather`` 表格插入新记录之前，
先在 ``cities`` 表格里面进行查找，
检查是否存在匹配的城市记录，
然后再决定是否将新的记录插入到 ``weather`` 表格里面。
这种简单的实现方法有非常多的问题，
并且这种方法也无法满足数据的一致性要求，
因此 PostgreSQL 选择了提供内置的引用完整性管理机制，
这种机制就是外键。

..
    The new declaration of the tables would look like this:

在定义了外键之后，
表格的新定义如下：

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

..
    Now try inserting an invalid record:

现在，
尝试插入一条不合法的记录将引发一个错误：

::

    INSERT INTO weather VALUES ('Berkeley', 45, 53, 0.0, '1994-11-28');

::

    ERROR:  insert or update on table "weather" violates foreign key constraint "weather_city_fkey"
    DETAIL:  Key (city)=(Berkeley) is not present in table "cities".

..
    The behavior of foreign keys can be finely tuned to your application. 
    
    We will not go beyond this simple example in this tutorial, 
    but just refer you to Chapter 5 for more information. 

    Making correct use of foreign keys 
    will definitely improve the quality of your database applications, 
    so you are strongly encouraged to learn about them.

外键的行为可以很好地集成到应用程序里面。
除了这个简单的示例以外，
这个教程并不会介绍更多关于外键的信息，
但这些信息可以在本文档的第 5 章找到。
正确地使用外键可以有效地提升数据库程序的质量，
因此读者应该仔细地学习与外键有关的知识。


事务
------------

..
    *Transactions* are a fundamental concept of all database systems. 

    The essential point of a transaction is that 
    it bundles multiple steps into a single, 
    all-or-nothing operation. 

    The intermediate states between the steps 
    are not visible to other concurrent transactions, 
    and if some failure occurs that prevents the transaction from completing, 
    then none of the steps affect the database at all.

*事务*\ 是所有数据库系统的一个基本概念。
事务本质上就是将多个步骤绑定为一个操作，
这个操作要么就执行所有被绑定的步骤，
要么就一个步骤都不执行。
各个步骤之间的中间状态对于其他并发事务是不可见的，
如果事务中的某个步骤在执行时出现了错误，
导致事务无法完成，
那么所有步骤的效果都会被撤销。

..
    For example, 
    consider a bank database that contains balances for various customer accounts, 
    as well as total deposit balances for branches. 
    Suppose that we want to record a payment of $100.00 from Alice's account to Bob's account. 
    Simplifying outrageously, the SQL commands for this might look like:

举个例子，
假设一个银行数据库保存着各个客户账号的余额，
并为每个分行都设置了一个总存款余额。
如果我们想要将爱丽丝账号上的 $100.00 转移到鲍勃的账号，
那么一个简陋的程序可能会执行以下 SQL 命令：

::

    UPDATE accounts SET balance = balance - 100.00
        WHERE name = 'Alice';

    UPDATE branches SET balance = balance - 100.00
        WHERE name = (SELECT branch_name FROM accounts WHERE name = 'Alice');

    UPDATE accounts SET balance = balance + 100.00
        WHERE name = 'Bob';

    UPDATE branches SET balance = balance + 100.00
        WHERE name = (SELECT branch_name FROM accounts WHERE name = 'Bob');

..
    The details of these commands are not important here; 
    the important point is that 
    there are several separate updates 
    involved to accomplish this rather simple operation. 
    Our bank's officers will want to be assured that either all these updates happen, 
    or none of them happen. 
    
    It would certainly not do for a system failure 
    to result in Bob receiving $100.00 that was not debited from Alice. 
    
    Nor would Alice long remain a happy customer 
    if she was debited without Bob being credited. 
        
    We need a guarantee that 
    if something goes wrong partway through the operation, 
    none of the steps executed so far will take effect. 
    
    Grouping the updates into a transaction gives us this guarantee. 
    A transaction is said to be *atomic* : 
    from the point of view of other transactions, 
    it either happens completely or not at all.

这些命令的细节并不重要，
重要的是，
要完成这个简单的转账操作，
程序需要进行好几个独立的更新。
银行的程序员需要保证这些更新要么全都都发生，
要么就一个都不发生。
即使系统出现故障，
也绝不能发生鲍勃收到了 $100.00 ，
但是爱丽丝的转账并没有成功的事情；
也不能发生爱丽丝已经转账了，
但是鲍勃没有收到钱的事情。
系统必须保证，
如果在操作执行的过程中，
有某个地方出错了，
那么已执行的所有步骤的效果都会被撤销。
通过将多个更新打包为一个事务可以获得以上所需的保证：
事务的\ *原子性*\ 可以保证事务包含的所有操作要么全部都执行，
要么就一个都不执行。

..
    We also want a guarantee that 
    once a transaction is completed and acknowledged by the database system, 
    it has indeed been permanently recorded 
    and won't be lost even if a crash ensues shortly thereafter. 
    
    For example, 
    if we are recording a cash withdrawal by Bob, 
    we do not want any chance that the debit to his account will disappear in a crash 
    just after he walks out the bank door. 

    A transactional database guarantees that 
    all the updates made by a transaction are logged in permanent storage (i.e., on disk) 
    before the transaction is reported complete.

我们还想要保证，
一旦事务完成并且得到了数据库系统的确认，
它将成为永久性的记录，
即使系统在事务完成之后崩溃，
事务的效果也不会丢失。
举个例子，
如果鲍勃刚刚在银行完成了一次提款操作，
那么我们希望，
即使系统在执行完提款操作之后崩溃，
鲍勃的提款记录也不会丢失。
支持事务功能的数据库可以保证，
数据库在报告一个事务执行完毕之前，
会先将那个事务执行的所有更新都记录到永久性存储器（比如硬盘）上面。

..
    Another important property of transactional databases 
    is closely related to the notion of atomic updates: 
    when multiple transactions are running concurrently, 
    each one should not be able to see the incomplete changes made by others. 

    For example, 
    if one transaction is busy totalling all the branch balances, 
    it would not do for it to include the debit from Alice's branch but not the credit to Bob's branch, 
    nor vice versa. 
    
    So transactions must be all-or-nothing 
    not only in terms of their permanent effect on the database, 
    but also in terms of their visibility as they happen. 
    
    The updates made so far by an open transaction are invisible to other transactions 
    until the transaction completes, 
    whereupon all the updates become visible simultaneously.

事务的另一个非常重要的属性和原子更新的概念密切相关：
当多个事务并发运行时，
每个事务都不应该看见其他事务正在进行的、尚不完整的修改。

举个例子，
如果一个事务正在计算所有分行的余额总和，
它会将爱丽丝所在分行进行的扣帐操作和鲍勃所在分行的入账操作都考虑在内，
而不会说只考虑了爱丽丝所在分行的扣帐操作，
但并没有考虑鲍勃所在分行的入账操作；
反之亦然。

因此事务不仅要在事务效果的持久性方面做到“要么全部执行，要么全部都不执行”，
它在事务效果的可见性方面也要做到这一点。

直到一个事务执行完毕为止，
这个事务进行的所有更新对于其他事务都是不可见的，
而当事务完成的时候，
事务进行的所有更新都会同时变为可见。

..
    In PostgreSQL, 
    a transaction is set up by surrounding the SQL commands of the transaction with ``BEGIN`` and ``COMMIT`` commands. 
    So our banking transaction would actually look like:

在 PostgreSQL 里面，
用户可以通过使用 ``BEGIN`` 和 ``COMMIT`` 这两个命令，
将那些需要在事务里面执行的事务包围起来，
从而创建一个事务。
上面例子中的银行转账操作可以用以下事务来实现：

::

    BEGIN;
    UPDATE accounts SET balance = balance - 100.00
        WHERE name = 'Alice';
    -- etc etc
    COMMIT;

..
    If, partway through the transaction, 
    we decide we do not want to commit (perhaps we just noticed that Alice's balance went negative), 
    we can issue the command ``ROLLBACK`` instead of ``COMMIT`` , 
    and all our updates so far will be canceled.

如果在创建事务的过程中，
我们临时决定放弃这个事务，
那么可以使用 ``ROLLBACK`` 命令去代替 ``COMMIT`` ，
从而撤销未执行的事务。

..
    PostgreSQL actually treats every SQL statement as being executed within a transaction. 
    If you do not issue a ``BEGIN`` command, 
    then each individual statement has an implicit ``BEGIN`` and (if successful) COMMIT wrapped around it. 
    A group of statements surrounded by ``BEGIN`` and ``COMMIT`` is sometimes called a transaction block.

PostgreSQL 实际上会把每条被执行的 SQL 语句当做是一个事务。
如果用户没有显式地执行 ``BEGIN`` 命令，
那么 PostgreSQL 将为每条单独的命令隐式地加上一个 ``BEGIN`` ，
并在 SQL 命令入队成功之后，
再追加一条 ``COMMIT`` 命令，
将被执行的 SQL 命令包围起来。

..
    .. note::

        Some client libraries issue ``BEGIN`` and ``COMMIT`` commands automatically, 
        so that you might get the effect of transaction blocks without asking. 
        Check the documentation for the interface you are using.

..
    It's possible to control the statements in a transaction in a more granular fashion 
    through the use of *savepoints* . 

    Savepoints allow you to selectively discard parts of the transaction, 
    while committing the rest. 

    After defining a savepoint with ``SAVEPOINT`` , 
    you can if needed roll back to the savepoint with ``ROLLBACK`` TO. 
    All the transaction's database changes between defining the savepoint and rolling back to it are discarded, 
    but changes earlier than the savepoint are kept.

通过使用\ *保存点*\ ，
用户可以以一种更为粒状（granular fashion）的方式去控制事务中的语句。
保存点允许用户在提交事务的过程中，
选择性地放弃事务中的特定部分。
在使用 ``SAVEPOINT`` 命令定义一个保存点之后，
用户可以在有需要的时候，
通过 ``ROLLBACK`` 命令回滚至那个保存点。
当保存点被回滚时，
位于保存点之内的所有数据库修改都会被撤销，
但定义保存点之前所做的那些修改则会继续存在。

..
    After rolling back to a savepoint, 
    it continues to be defined, 
    so you can roll back to it several times. 
    Conversely, 
    if you are sure you won't need to roll back to a particular savepoint again, it can be released, 
    so the system can free some resources. 
    Keep in mind that either releasing or rolling back to a savepoint 
    will automatically release all savepoints that were defined after it.

在回滚至一个保存点之后，
那个保存点还会继续存在，
以便用户在有需要的时候再次进行回滚。
与此相反，
如果用户确定他们不再需要某个保存点，
那么他们也可以释放那个保存点，
使得系统可以回收某些与保存点相关的资源。
需要注意的一点是，
当一个保存点被释放/回滚时，
在那个保存点之后定义的所有保存点也会自动被释放/回滚。

..
    All this is happening within the transaction block, 
    so none of it is visible to other database sessions. 
    When and if you commit the transaction block, 
    the committed actions become visible as a unit to other sessions, 
    while the rolled-back actions never become visible at all.

因为所有这些动作都发生在事务块（transaction block）内部，
所以这些动作对于其他数据库会话来说将是不可见的。
当用户提交事务块之后，
已提交的动作将作为一个整体（unit）变得对外部会话可见；
至于那些被回滚了的操作，
它们对于外部会话来说永远都是不可见的。

..
    Remembering the bank database, 
    suppose we debit $100.00 from Alice's account, 
    and credit Bob's account, 
    only to find later that we should have credited Wally's account. 
    We could do it using savepoints like this:

回到之前银行数据库的例子。
假设程序原本打算从爱丽丝的账户里面扣除 $100.00 ，
并将这些钱转账到鲍勃的账户里面，
但之后却发现原来应该转账到沃利的账号里面才对。
如果程序使用了 PostgreSQL 的保存点特性，
那么它可能会执行以下命令：

::

    BEGIN;
    UPDATE accounts SET balance = balance - 100.00
        WHERE name = 'Alice';
    SAVEPOINT my_savepoint;
    UPDATE accounts SET balance = balance + 100.00
        WHERE name = 'Bob';
    -- oops ... forget that and use Wally's account
    ROLLBACK TO my_savepoint;
    UPDATE accounts SET balance = balance + 100.00
        WHERE name = 'Wally';
    COMMIT;

..
    This example is, of course, oversimplified, 
    but there's a lot of control possible in a transaction block through the use of savepoints. 
    Moreover, 
    ``ROLLBACK`` TO is the only way to regain control of a transaction block 
    that was put in aborted state by the system due to an error, 
    short of rolling it back completely and starting again.

这个示例虽然非常简单，
但它有效地向我们展示了如何通过保存点去对事务块进行控制。
此外，
当事务块因为错误而被系统标识为终止状态（aborted state）时，
重新取得事务块控制权的唯一手段就是使用 ``ROLLBACK`` 。


窗口函数
------------

A *window function* performs a calculation across a set of table rows 
that are somehow related to the current row. 
This is comparable to the type of calculation 
that can be done with an aggregate function. 
But unlike regular aggregate functions, 
use of a window function does not cause rows to become grouped into a single output row 
— the rows retain their separate identities. 
Behind the scenes, 
the window function is able to access more than just the current row of the query result.

Here is an example that shows how to compare each employee's salary 
with the average salary in his or her department:

::

    SELECT depname, empno, salary, avg(salary) OVER (PARTITION BY depname) FROM empsalary;

::

      depname  | empno | salary |          avg          
    -----------+-------+--------+-----------------------
     develop   |    11 |   5200 | 5020.0000000000000000
     develop   |     7 |   4200 | 5020.0000000000000000
     develop   |     9 |   4500 | 5020.0000000000000000
     develop   |     8 |   6000 | 5020.0000000000000000
     develop   |    10 |   5200 | 5020.0000000000000000
     personnel |     5 |   3500 | 3700.0000000000000000
     personnel |     2 |   3900 | 3700.0000000000000000
     sales     |     3 |   4800 | 4866.6666666666666667
     sales     |     1 |   5000 | 4866.6666666666666667
     sales     |     4 |   4800 | 4866.6666666666666667
    (10 rows)

The first three output columns come directly from the table ``empsalary`` , 
and there is one output row for each row in the table. 
The fourth column represents an average taken across all the table rows that have the same ``depname`` value as the current row. 
(This actually is the same function as the regular avg aggregate function, 
but the ``OVER`` clause causes it to be treated as a window function 
and computed across an appropriate set of rows.)

A window function call always contains an ``OVER`` clause directly following the window function's name and argument(s). 
This is what syntactically distinguishes it from a regular function or aggregate function. 
The ``OVER`` clause determines exactly how the rows of the query are split up for processing by the window function. 
The ``PARTITION BY`` list within ``OVER`` specifies dividing the rows into groups, or partitions, 
that share the same values of the ``PARTITION BY`` expression(s). 
For each row, 
the window function is computed across the rows that fall into the same partition as the current row.

You can also control the order in which rows are processed by window functions using ``ORDER BY`` within ``OVER`` . 
(The window ``ORDER BY`` does not even have to match the order in which the rows are output.) 
Here is an example:

::

    SELECT depname, empno, salary,
           rank() OVER (PARTITION BY depname ORDER BY salary DESC)
    FROM empsalary;

::

      depname  | empno | salary | rank 
    -----------+-------+--------+------
     develop   |     8 |   6000 |    1
     develop   |    10 |   5200 |    2
     develop   |    11 |   5200 |    2
     develop   |     9 |   4500 |    4
     develop   |     7 |   4200 |    5
     personnel |     2 |   3900 |    1
     personnel |     5 |   3500 |    2
     sales     |     1 |   5000 |    1
     sales     |     4 |   4800 |    2
     sales     |     3 |   4800 |    2
    (10 rows)

As shown here, 
the ``rank`` function produces a numerical rank within the current row's partition for each distinct ``ORDER BY`` value, 
in the order defined by the ``ORDER BY`` clause. 
``rank`` needs no explicit parameter, 
because its behavior is entirely determined by the ``OVER`` clause.

The rows considered by a window function are those of the "virtual table" produced by the query's ``FROM`` clause as filtered by its ``WHERE`` , ``GROUP BY`` , and ``HAVING`` clauses if any. 
For example, 
a row removed because it does not meet the ``WHERE`` condition is not seen by any window function. 
A query can contain multiple window functions that slice up the data in different ways by means of different ``OVER`` clauses, 
but they all act on the same collection of rows defined by this virtual table.

We already saw that ORDER BY can be omitted if the ordering of rows is not important. 
It is also possible to omit ``PARTITION BY`` , 
in which case there is just one partition containing all the rows.

There is another important concept associated with window functions: 
for each row, there is a set of rows within its partition called its *window frame* . 
Many (but not all) window functions act only on the rows of the window frame, 
rather than of the whole partition. 
By default, if ``ORDER BY`` is supplied then the frame consists of all rows from the start of the partition up through the current row, 
plus any following rows that are equal to the current row according to the ``ORDER BY`` clause. 
When ``ORDER BY`` is omitted the default frame consists of all rows in the partition. [#f1]_ 
Here is an example using ``sum`` :

::

    SELECT salary, sum(salary) OVER () FROM empsalary;

::

     salary |  sum  
    --------+-------
       5200 | 47100
       5000 | 47100
       3500 | 47100
       4800 | 47100
       3900 | 47100
       4200 | 47100
       4500 | 47100
       4800 | 47100
       6000 | 47100
       5200 | 47100
    (10 rows)

Above, since there is no ``ORDER BY`` in the ``OVER`` clause, 
the window frame is the same as the partition, 
which for lack of ``PARTITION BY`` is the whole table; 
in other words each sum is taken over the whole table 
and so we get the same result for each output row. 
But if we add an ``ORDER BY`` clause, we get very different results:

::

    SELECT salary, sum(salary) OVER (ORDER BY salary) FROM empsalary;

::

     salary |  sum  
    --------+-------
       3500 |  3500
       3900 |  7400
       4200 | 11600
       4500 | 16100
       4800 | 25700
       4800 | 25700
       5000 | 30700
       5200 | 41100
       5200 | 41100
       6000 | 47100
    (10 rows)

Here the sum is taken from the first (lowest) salary up through the current one, 
including any duplicates of the current one (notice the results for the duplicated salaries).

Window functions are permitted only in the ``SELECT`` list and the ``ORDER BY`` clause of the query. 
They are forbidden elsewhere, such as in ``GROUP BY`` , ``HAVING`` and ``WHERE`` clauses. 
This is because they logically execute after the processing of those clauses. 
Also, window functions execute after regular aggregate functions. 
This means it is valid to include an aggregate function call in the arguments of a window function, 
but not vice versa.

If there is a need to filter or group rows after the window calculations are performed, 
you can use a sub-select. For example:

::

    SELECT depname, empno, salary, enroll_date
    FROM
      (SELECT depname, empno, salary, enroll_date,
              rank() OVER (PARTITION BY depname ORDER BY salary DESC, empno) AS pos
         FROM empsalary
      ) AS ss
    WHERE pos < 3;

The above query only shows the rows from the inner query having ``rank`` less than 3.

When a query involves multiple window functions, 
it is possible to write out each one with a separate ``OVER`` clause, 
but this is duplicative and error-prone if the same windowing behavior is wanted for several functions. 
Instead, each windowing behavior can be named in a ``WINDOW`` clause and then referenced in ``OVER`` . 
For example:

::

    SELECT sum(salary) OVER w, avg(salary) OVER w
      FROM empsalary
      WINDOW w AS (PARTITION BY depname ORDER BY salary DESC);

More details about window functions can be found in `Section 4.2.8 <http://www.postgresql.org/docs/devel/static/sql-expressions.html#SYNTAX-WINDOW-FUNCTIONS>`_\ , `Section 9.21 <http://www.postgresql.org/docs/devel/static/functions-window.html>`_\ , `Section 7.2.5 <http://www.postgresql.org/docs/devel/static/queries-table-expressions.html#QUERIES-WINDOW>`_\ , and the `SELECT <http://www.postgresql.org/docs/devel/static/sql-select.html>`_ reference page.

.. [#f1] There are options to define the window frame in other ways, but this tutorial does not cover them. See `Section 4.2.8 <http://www.postgresql.org/docs/devel/static/sql-expressions.html#SYNTAX-WINDOW-FUNCTIONS>`_ for details.


继承
------------

..
    Inheritance is a concept from object-oriented databases. 
    It opens up interesting new possibilities of database design.

继承（inheritance）是一个来自于面向对象数据库的概念，
它为数据库设计带来了新的可能性。

..
    Let's create two tables: 
    A table ``cities`` and a table ``capitals`` . 

    Naturally, 
    capitals are also cities, 
    so you want some way to show the capitals implicitly 
    when you list all cities. 

    If you're really clever 
    you might invent some scheme like this:

假设我们现在要创建两个表格，
一个表格储存城市的相关信息，
而另一个表格则储存首都的相关信息。
因为首都也属于城市，
所以你可能会想要在展示所有城市的时候，
将首都也一并展示出来。
为此，
你可能会写出以下代码：

::

    CREATE TABLE capitals (
      name       text,
      population real,
      altitude   int,    -- (in ft)
      state      char(2)
    );

    CREATE TABLE non_capitals (
      name       text,
      population real,
      altitude   int     -- (in ft)
    );

    CREATE VIEW cities AS
      SELECT name, population, altitude FROM capitals
        UNION
      SELECT name, population, altitude FROM non_capitals;

..
    This works OK as far as querying goes, 
    but it gets ugly when you need to update several rows, 
    for one thing.

这种做法虽然可以实现我们想要的查询效果，
但是当我们因为某个原因需要对表格中的某些行进行更新的时候，
这种做法就会显得异常丑陋。

..
    A better solution is this:

以下是一个更好的解决方法：

::

    CREATE TABLE cities (
      name       text,
      population real,
      altitude   int     -- (in ft)
    );

    CREATE TABLE capitals (
      state      char(2)
    ) INHERITS (cities);

..
    In this case, 
    a row of capitals inherits all columns (name, population, and altitude) from its parent, 
    cities. 

    The type of the column name is text, 
    a native PostgreSQL type for variable length character strings. 

    State capitals have an extra column, state, that shows their state. 

    In PostgreSQL, 
    a table can inherit from zero or more other tables.

在这个示例中，
一个 ``capitals`` 表格的行将从它的父表格（parent） ``cities`` 表格那里继承它的所有列（\ ``name`` 、 ``population`` 和 ``altitude``\ ）。
其中，
``name`` 列的类型为 ``text`` ，
这是 PostgreSQL 内置的一种类型，
用于储存长度可变的文字字符串。
与 ``cities`` 表格相比，
``capitals`` 表格拥有额外的 ``state`` 列，
这个列用于储存首都所属的国家。
在 PostgreSQL 中，
一个表格可以继承自任意多个其他表格。

..
    For example, 
    the following query finds the names of all cities, 
    including state capitals, 
    that are located at an altitude over 500 feet:

作为例子，
下面这个查询可以找出海拔超过 500 英尺的城市和首都，
并返回它们的名字和海拔：

::

    SELECT name, altitude
      FROM cities
        WHERE altitude > 500;

..
    which returns:

以下是这个查询的执行结果：

::

       name    | altitude
    -----------+----------
     Las Vegas |     2174
     Mariposa  |     1953
     Madison   |      845
    (3 rows)

..
    On the other hand, 
    the following query finds all the cities that are not state capitals 
    and are situated at an altitude over 500 feet:

另一方面，
以下这个查询可以找出那些不是首都，
但海拔都超过 500 英尺的城市：

::

    SELECT name, altitude
        FROM ONLY cities
        WHERE altitude > 500;

::

       name    | altitude
    -----------+----------
     Las Vegas |     2174
     Mariposa  |     1953
    (2 rows)

..
    Here the ``ONLY`` before cities indicates that 
    the query should be run over only the ``cities`` table, 
    and not tables below ``cities`` in the inheritance hierarchy. 
    
    Many of the commands that we have already discussed — 
    SELECT, UPDATE, and DELETE — support this ONLY notation.

位于 ``cities`` 前面的 ``ONLY`` 表示查询只需要对 ``cities`` 表格进行查询，
至于那些在继承层次里面低于 ``cities`` 表格的其他表格，
则不需要进行查询。
这个教程里面介绍过的很多命令，
比如 ``SELECT`` 、 ``UPDATE`` 和 ``DELETE`` ，
都支持这个 ``ONLY`` 选项。

..
    .. note::

        Although inheritance is frequently useful, it has not been integrated with unique constraints or foreign keys, which limits its usefulness. See Section 5.9 for more detail.


结语
--------

..
    PostgreSQL has many features not touched upon in this tutorial introduction, 
    which has been oriented toward newer users of SQL. 
    These features are discussed in more detail in the remainder of this book.

因为这个入门教程面向的是刚刚接触 SQL 的用户，
所以 PostgreSQL 的很多特性都没有在这个教程里面提到，
不过本文档的后续章节将陆续对这些特性进行介绍。

..
    If you feel you need more introductory material, 
    please visit the PostgreSQL web site for links to more resources.

如果你需要更多入门方面的学习材料，
请访问 `PostgreSQL 的主页 <http://www.postgresql.org/>`_\ 以查看更多相关资源的链接。
