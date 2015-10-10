数据类型
==================

..
    PostgreSQL has a rich set of native data types available to users. 
    Users can add new types to PostgreSQL using the CREATE TYPE command.

PostgreSQL 为用户提供了一集丰富的原生数据类型，
并且也允许用户使用 ``CREATE TYPE`` 命令去创建新的自定义类型。

..
    Table 8-1 shows all the built-in general-purpose data types. 

    Most of the alternative names listed in the "Aliases" column 
    are the names used internally by PostgreSQL for historical reasons. 
    
    In addition, 
    some internally used or deprecated types are available, 
    but are not listed here.

表格 8-1 展示了 PostgreSQL 内置的所有通用数据类型。
“别名”一栏中列出的大多数别名，
都是因为历史原因而在 PostgreSQL 内部使用的别名。
除此之外，
PostgreSQL 还提供了一些别的内部使用类型或者已被废弃的类型，
不过这些类型并没有在表格 8-1 中展示。

----

表格 8-1 数据类型

+-----------------------------------------------+---------------------------+---------------------------------------------------+
| 名字                                          | 别名                      | 作用简介                                          |
+===============================================+===========================+===================================================+
| ``bigint``                                    |  ``int8``                 | 有符号的 8 字节长整数                             |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``bigserial``                                 |   ``serial8``             | 8 字节长的自增整数                                |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``bit [ (n) ]``                               |                           | 固定长度的位串（bit string）                      |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``bit varying [ (n) ]``                       | ``varbit``                | 长度可变的位串（bit string）                      |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``boolean``                                   | ``bool``                  | 逻辑布尔值（真/假）                               |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``box``                                       |                           | 平面上的一个长方体（rectangular box）             |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``bytea``                                     |                           | 二进制数据（“字节数组”）                          |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``character [ (n) ]``                         | ``char [ (n) ]``          | 固定长度的字符串                                  |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``character varying [ (n) ]``                 | ``varchar [ (n) ]``       | 长度可变的字符串                                  |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``cidr``                                      |                           | IPv4 或者 IPv6 格式的网络地址                     |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``circle``                                    |                           | 平面上的一个圈（circle）                          |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``date``                                      |                           | 日历日期（年，月，日）                            |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``double precision``                          | ``float8``                | 8 字节长的双精度浮点数                            |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``inet``                                      |                           | IPv4 或者 IPv6 格式的主机地址（host address）     |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``integer``                                   | ``int`` , ``int4``        | 四字节长的有符号整数                              |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``interval [ fields ] [ (p) ]``               |                           | 时间间隔                                          |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``json``                                      |                           | 文化式形式（textual）的 JSON 数据                 |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``jsonb``                                     |                           | 二进制形式的 JSON 数据，decomposed                |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``line``                                      |                           | 平面上的一条无限长的直线                          |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``lseg``                                      |                           | 平面上的一个线段                                  |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``macaddr``                                   |                           | MAC（Media Access Control，媒体访问控制）地址     |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``money``                                     |                           | 金额数量                                          |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``numeric [ (p, s) ]``                        | ``decimal [ (p, s) ]``    | 精度可选的精确小数                                |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``path``                                      |                           | 平面上的一条几何路径                              |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``pg_lsn``                                    |                           | PostgreSQL 的日志序列数字                         |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``point``                                     |                           | 平上面的一个几何点                                |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``polygon``                                   |                           | 平面上的一条封闭集合路径                          |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``real``                                      | ``float4``                | 4 字节长的单精度浮点数                            |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``smallint``                                  | ``int2``                  | 2 字节长的有符号整数                              |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``smallserial``                               | ``serial2``               | 2 字节长的自增整数                                |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``serial``                                    | ``serial4``               | 4 字节长的自增整数                                |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``text``                                      |                           | 长度可变的字符串                                  |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``time [ (p) ] [ without time zone ]``        |                           | 一天中的时间（不带时区）                          |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``time [ (p) ] with time zone``               | ``timetz``                | 一天中的时间（带时区）                            |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``timestamp [ (p) ] [ without time zone ]``   |                           | 日期和时间（不带时区）                            |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``timestamp [ (p) ] with time zone``          |                           | 日期和时间（带时区）                              |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``tsquery``                                   |                           | 文本搜索查询                                      |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``tsvector``                                  |                           | 文本搜索文档                                      |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``txid_snapshot``                             |                           | 用户级别（user-level）的事务 ID 快照（snapshot）  |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``uuid``                                      |                           | 全局唯一标识符                                    |
+-----------------------------------------------+---------------------------+---------------------------------------------------+
| ``xml``                                       |                           | XML 数据                                          |
+-----------------------------------------------+---------------------------+---------------------------------------------------+

----

..
    .. note::
        
        Compatibility: The following types (or spellings thereof) are specified by SQL: bigint, bit, bit varying, boolean, char, character varying, character, varchar, date, double precision, integer, interval, numeric, decimal, real, smallint, time (with or without time zone), timestamp (with or without time zone), xml.

..
    Each data type has an external representation 
    determined by its input and output functions. 

    Many of the built-in types have obvious external formats. 

    However, 
    several types are either unique to PostgreSQL, 
    such as geometric paths, 
    or have several possible formats, 
    such as the date and time types. 

    Some of the input and output functions are not invertible, i.e., 
    the result of an output function might lose accuracy 
    when compared to the original input.

每种数据类型的外部表示都由它们自己的输入和输出函数决定。
大多数内置类型都有明显的外部格式。
诸如集合路径这样的路径是 PostgreSQL 特有的，
而像是 ``date`` 和 ``time`` 这样的类型则拥有好几种不同的格式。
某些输入和输出函数是不可逆的（invertible），
也即是说，
输入函数的结果与它原来的输入相比，
可能会出现一定程度的精度丢失。

数字类型
------------------

..
    Numeric types consist of two-, four-, and eight-byte integers, 
    four- and eight-byte floating-point numbers, 
    and selectable-precision decimals. 
    Table 8-2 lists the available types.

PostgreSQL 的数字类型包含两字节、四字节和八字节整数，
四字节和八字节浮点数，
以及精度可选的小数构成。
表格 8-2 列出了所有可用的类型。

----

表 8-2 数字类型

+-----------------------+---------------+-------------------------------+-----------------------------------------------------------------------+
| 名字                  | 占用空间      | 作用简介                      | 取值范围                                                              |
+=======================+===============+===============================+=======================================================================+
| ``smallint``          | 2 字节        | 取值范围较小的整数            | ``-32768`` 至 ``+32767``                                              |
+-----------------------+---------------+-------------------------------+-----------------------------------------------------------------------+
| ``integer``           | 4 字节        | 典型的整数                    | ``-2147483648`` 至 ``+2147483647``                                    |
+-----------------------+---------------+-------------------------------+-----------------------------------------------------------------------+
| ``bigint``            | 8 字节        | 取值范围较大的整数            | ``-9223372036854775808`` 至 ``+9223372036854775807``                  |
+-----------------------+---------------+-------------------------------+-----------------------------------------------------------------------+
| ``decimal``           | 长度可变      | 由用户指定精度的精确数字      | 小数点之前最多 ``131072`` 个数字，小数点之后最多 ``16383`` 个数字     |
+-----------------------+---------------+-------------------------------+-----------------------------------------------------------------------+
| ``numeric``           | 长度可变      | 由用户指定精度的精确数字      | 小数点之前最多 ``131072`` 个数字，小数点之后最多 ``16383`` 个数字     |
+-----------------------+---------------+-------------------------------+-----------------------------------------------------------------------+
| ``real``              | 4 字节        | 精度可变的不精确数字          | 精确至小数点后 6 位                                                   |
+-----------------------+---------------+-------------------------------+-----------------------------------------------------------------------+
| ``double precision``  | 8 字节        | 精度可变的不精确数字          | 精确至小数点后 15 位                                                  |
+-----------------------+---------------+-------------------------------+-----------------------------------------------------------------------+
| ``smallserial``       | 2 字节        | 小的自增整数                  | ``1`` 至 ``32767``                                                    |
+-----------------------+---------------+-------------------------------+-----------------------------------------------------------------------+
| ``serial``            | 4 字节        | 自增整数                      | ``1`` 至 ``2147483647``                                               |
+-----------------------+---------------+-------------------------------+-----------------------------------------------------------------------+
| ``bigserial``         | 8 字节        | 大的自增整数                  | ``1`` 至 ``9223372036854775807``                                      |
+-----------------------+---------------+-------------------------------+-----------------------------------------------------------------------+

----

..
    The syntax of constants for the numeric types 
    is described in Section 4.1.2. 
    
    The numeric types have a full set of corresponding arithmetic operators and functions. 
    Refer to Chapter 9 for more information. 
    
    The following sections describe the types in detail.

本文档的 4.1.2 节描述了数字类型常量所使用的语法。
数字类型拥有一整套相应的数字操作和函数，
这些操作和函数将在本文档的第 9 章进行介绍。
本节接下来将对各个数字类型的细节进行描述。

整数类型
^^^^^^^^^^^^^^^^^^^

..
    The types smallint, integer, and bigint store whole numbers, 
    that is, 
    numbers without fractional components, 
    of various ranges. 
    Attempts to store values outside of the allowed range will result in an error.

``smallint`` 、 ``integer`` 和 ``bigint`` 这三种类型都可以用于储存不带小数部分的数字，
它们之间的唯一区别就是各自的取值范围不同。
尝试在这些类型的列里面储存一个超出取值范围的值将引发一个错误。

..
    The type integer is the common choice, 
    as it offers the best balance between range, storage size, and performance. 
    
    The smallint type is generally only used if disk space is at a premium. 
    
    The bigint type is designed to be used 
    when the range of the integer type is insufficient.

``integer`` 类型是最常用的整数类型，
它在取值范围、储存空间和性能上面都提供了最好的平衡性。
``smallint`` 类型通常只在硬盘空间非常昂贵的情况下使用。

..
    SQL only specifies the integer types integer (or int), smallint, and bigint. 
    
    The type names int2, int4, and int8 are extensions, 
    which are also used by some other SQL database systems.

SQL 标准只指定了 ``integer`` 类型（又称 ``int`` 类型）、 ``smallint`` 类型和 ``bigint`` 类型。
类型名 ``int2`` 、 ``int4`` 和 ``int8`` 都是 PostgreSQL 自身的扩展，
某些其他的 SQL 数据库系统也在使用这些扩展。

任意精度数字
^^^^^^^^^^^^^^^^^^^^

..
    The type numeric can store numbers with a very large number of digits. 

    It is especially recommended for storing monetary amounts and other quantities where exactness is required. 

    Calculations with numeric values yield exact results where possible, 
    e.g. addition, subtraction, multiplication. 

    However, 
    calculations on numeric values are very slow compared to the integer types, 
    or to the floating-point types described in the next section.


..
    We use the following terms below: 
    The scale of a numeric is the count of decimal digits in the fractional part, 
    to the right of the decimal point. 
    
    The precision of a numeric is the total count of significant digits in the whole number, 
    that is, the number of digits to both sides of the decimal point. 
    
    So the number 23.5141 has a precision of 6 and a scale of 4. 
    
    Integers can be considered to have a scale of zero.

..
    Both the maximum precision and the maximum scale of a numeric column can be configured. 
    To declare a column of type numeric use the syntax:

::

    NUMERIC(precision, scale)

..
    The precision must be positive, 
    the scale zero or positive. 
    Alternatively:

精度必须是正数，
而大小则可以是 ``0`` 或者正数。
此外，
如果大小为 ``0`` ，
那么也可以使用以下这种声明：

::

    NUMERIC(precision)

..
    selects a scale of 0. 
    Specifying:

::

    NUMERIC

without any precision or scale creates a column in which numeric values of any precision and scale can be stored, 
up to the implementation limit on precision. 
A column of this kind will not coerce input values to any particular scale, 
whereas numeric columns with a declared scale will coerce input values to that scale. 
(The SQL standard requires a default scale of 0, i.e., coercion to integer precision. We find this a bit useless. If you're concerned about portability, always specify the precision and scale explicitly.)

..
    Note: The maximum allowed precision when explicitly specified in the type declaration is 1000; NUMERIC without a specified precision is subject to the limits described in Table 8-2.

If the scale of a value to be stored is greater than the declared scale of the column, the system will round the value to the specified number of fractional digits. Then, if the number of digits to the left of the decimal point exceeds the declared precision minus the declared scale, an error is raised.

Numeric values are physically stored without any extra leading or trailing zeroes. Thus, the declared precision and scale of a column are maximums, not fixed allocations. (In this sense the numeric type is more akin to varchar(n) than to char(n).) The actual storage requirement is two bytes for each group of four decimal digits, plus three to eight bytes overhead.

In addition to ordinary numeric values, the numeric type allows the special value NaN, meaning "not-a-number". Any operation on NaN yields another NaN. When writing this value as a constant in an SQL command, you must put quotes around it, for example UPDATE table SET x = 'NaN'. On input, the string NaN is recognized in a case-insensitive manner.

..
    Note: In most implementations of the "not-a-number" concept, NaN is not considered equal to any other numeric value (including NaN). In order to allow numeric values to be sorted and used in tree-based indexes, PostgreSQL treats NaN values as equal, and greater than all non-NaN values.

The types decimal and numeric are equivalent. Both types are part of the SQL standard.

When rounding values, the numeric type rounds ties away from zero, while (on most machines) the real and double precision types round ties to the nearest even number. For example:

::

    SELECT x,
      round(x::numeric) AS num_round,
      round(x::double precision) AS dbl_round
    FROM generate_series(-3.5, 3.5, 1) as x;
      x   | num_round | dbl_round
    ------+-----------+-----------
     -3.5 |        -4 |        -4
     -2.5 |        -3 |        -2
     -1.5 |        -2 |        -2
     -0.5 |        -1 |        -0
      0.5 |         1 |         0
      1.5 |         2 |         2
      2.5 |         3 |         2
      3.5 |         4 |         4
    (8 rows)

浮点类型
^^^^^^^^^^^^^^^^

The data types real and double precision are inexact, variable-precision numeric types. In practice, these types are usually implementations of IEEE Standard 754 for Binary Floating-Point Arithmetic (single and double precision, respectively), to the extent that the underlying processor, operating system, and compiler support it.

Inexact means that some values cannot be converted exactly to the internal format and are stored as approximations, so that storing and retrieving a value might show slight discrepancies. Managing these errors and how they propagate through calculations is the subject of an entire branch of mathematics and computer science and will not be discussed here, except for the following points:

- If you require exact storage and calculations (such as for monetary amounts), use the numeric type instead.

- If you want to do complicated calculations with these types for anything important, especially if you rely on certain behavior in boundary cases (infinity, underflow), you should evaluate the implementation carefully.

- Comparing two floating-point values for equality might not always work as expected.

On most platforms, the real type has a range of at least 1E-37 to 1E+37 with a precision of at least 6 decimal digits. The double precision type typically has a range of around 1E-307 to 1E+308 with a precision of at least 15 digits. Values that are too large or too small will cause an error. Rounding might take place if the precision of an input number is too high. Numbers too close to zero that are not representable as distinct from zero will cause an underflow error.

..
Note: The extra_float_digits setting controls the number of extra significant digits included when a floating point value is converted to text for output. With the default value of 0, the output is the same on every platform supported by PostgreSQL. Increasing it will produce output that more accurately represents the stored value, but may be unportable.

In addition to ordinary numeric values, the floating-point types have several special values:

- ``Infinity``
- ``-Infinity``
- ``NaN``

These represent the IEEE 754 special values "infinity", "negative infinity", and "not-a-number", respectively. (On a machine whose floating-point arithmetic does not follow IEEE 754, these values will probably not work as expected.) When writing these values as constants in an SQL command, you must put quotes around them, for example UPDATE table SET x = 'Infinity'. On input, these strings are recognized in a case-insensitive manner.

..
Note: IEEE754 specifies that NaN should not compare equal to any other floating-point value (including NaN). In order to allow floating-point values to be sorted and used in tree-based indexes, PostgreSQL treats NaN values as equal, and greater than all non-NaN values.

PostgreSQL also supports the SQL-standard notations float and float(p) for specifying inexact numeric types. Here, p specifies the minimum acceptable precision in binary digits. PostgreSQL accepts float(1) to float(24) as selecting the real type, while float(25) to float(53) select double precision. Values of p outside the allowed range draw an error. float with no precision specified is taken to mean double precision.

..
Note: The assumption that real and double precision have exactly 24 and 53 bits in the mantissa respectively is correct for IEEE-standard floating point implementations. On non-IEEE platforms it might be off a little, but for simplicity the same ranges of p are used on all platforms.

序列号类型
^^^^^^^^^^^^^^^^^

The data types smallserial, serial and bigserial are not true types, but merely a notational convenience for creating unique identifier columns (similar to the AUTO_INCREMENT property supported by some other databases). In the current implementation, specifying:

::

    CREATE TABLE tablename (
        colname SERIAL
    );

is equivalent to specifying:

::

    CREATE SEQUENCE tablename_colname_seq;
    CREATE TABLE tablename (
        colname integer NOT NULL DEFAULT nextval('tablename_colname_seq')
    );
    ALTER SEQUENCE tablename_colname_seq OWNED BY tablename.colname;

Thus, we have created an integer column and arranged for its default values to be assigned from a sequence generator. A NOT NULL constraint is applied to ensure that a null value cannot be inserted. (In most cases you would also want to attach a UNIQUE or PRIMARY KEY constraint to prevent duplicate values from being inserted by accident, but this is not automatic.) Lastly, the sequence is marked as "owned by" the column, so that it will be dropped if the column or table is dropped.

..
Note: Because smallserial, serial and bigserial are implemented using sequences, there may be "holes" or gaps in the sequence of values which appears in the column, even if no rows are ever deleted. A value allocated from the sequence is still "used up" even if a row containing that value is never successfully inserted into the table column. This may happen, for example, if the inserting transaction rolls back. See nextval() in Section 9.16 for details.

To insert the next value of the sequence into the serial column, specify that the serial column should be assigned its default value. This can be done either by excluding the column from the list of columns in the INSERT statement, or through the use of the DEFAULT key word.

The type names serial and serial4 are equivalent: both create integer columns. The type names bigserial and serial8 work the same way, except that they create a bigint column. bigserial should be used if you anticipate the use of more than 2^31 identifiers over the lifetime of the table. The type names smallserial and serial2 also work the same way, except that they create a smallint column.

The sequence created for a serial column is automatically dropped when the owning column is dropped. You can drop the sequence without dropping the column, but this will force removal of the column default expression.
