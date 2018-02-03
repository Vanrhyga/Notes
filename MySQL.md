[TOC]

### SQL简介

**SQL，指结构化查询语言，用于访问和处理数据库。**

RDBMS，指关系型数据库管理系统，其数据被存储在表（tables）中。

表是相关的数据项的集合，由行和列组成。



### SQL语法

一个数据库通常包含一个或多个表。

每个表由一个名字标识，包含带有数据的记录（行）。

下面的语句从 Persons 表中选取 LastName 列的数据：

~~~sql
SELECT LastName FROM Persons
~~~

**SQL对大小写不敏感！**

可以把SQL分为两个部分：数据操作语言（DML）和数据定义语言（DDL）。

查询和更新指令构成了SQL的DML部分：

> SELECT - 从数据库表中获取数据
>
> UPDATE - 更新数据库表中的数据
>
> DELETE - 从数据库表中删除数据
>
> INSERT INTO - 向数据库表中插入数据

DDL部分使我们有能力创建或删除表格：

> CREATE DATABASE - 创建新数据库
>
> ALTER DATABASE - 修改数据库
>
> CREATE TABLE - 创建新表
>
> ALTER TABLE - 变更数据库表
>
> DROP TABLE - 删除表
>
> CREATE INDEX - 创建索引
>
> DROP INDEX - 删除索引



### SQL SELECT 语句

SELECT语句用于从表中选取数据，结果被存储在一个结果集中。

SQL SELECT 语法：

~~~sql
SELECT 列名称 FROM 表名称
~~~

以及：

~~~sql
SELECT * FROM 表名称
~~~

例如，从 Persons 数据库表中获取 "LastName" 和 "FirstName" 列的内容：

~~~sql
SELECT LastName,FirstName FROM Persons
~~~

现在我们希望从 Persons 表中选取所有的列，可以使用符号 * 取代列的名称：

~~~sql
SELECT * FROM Persons
~~~

**提示**：* 是选取所有列的快捷方式。



### SQL SELECT DISTINCT 语句

在表中，可能会包含重复值。不过，有时希望仅仅列出不同的值，关键词 DISTINCT 用于返回唯一不同的值：

~~~sql
SELECT DISTINCT 列名称 FROM 表名称
~~~



### SQL WHERE 子句

**WHERE 子句用于规定选择的标准**

如需有条件地从表中选取数据，可将 WHERE 子句添加到 SELECT 语句：

~~~sql
SELECT 列名称 FROM 表名称 WHERE 列 运算符 值
~~~

下面的运算符可在 WHERE 子句中使用：

| 操作符     | 描述     |
| ------- | ------ |
| =       | 等于     |
| <>      | 不等于    |
| >       | 大于     |
| <       | 小于     |
| >=      | 大于等于   |
| <=      | 小于等于   |
| BETWEEN | 在某个范围内 |
| LIKE    | 搜索某种模式 |

**注释：**某些版本的SQL中，<> 可写为 != 。

如果只希望选取居住在城市 "Beijing" 中的人：

~~~sql
SELECT * FROM Persons WHERE City='Beijing'
~~~

**SQL使用单引号来环绕文本值（大部分数据库系统也接受双引号）。若是数值，请不要使用引号。**

*文本值：*

> 这是正确的：
>
> SELECT * FROM Persons WHERE FirstName='Bush'

> 这是错误的：
>
> SELECT * FROM Persons WHERE FirstName=Bush

*数值：*

> 这是正确的：
>
> SELECT * FROM Persons WHERE Year>1965

> 这是错误的：
>
> SELECT * FROM Persons WHERE Year>'1965'



### SQL AND&OR 运算符

**AND 和 OR 运算符基于一个以上的条件对记录进行过滤。**

AND 和 OR 可在 WHERE 子语句中把两个或多个条件结合起来。

如果第一个条件和第二个条件都成立，则 AND 运算符显示一条记录。

如果第一个条件和第二个条件只要有一个成立，则 OR 运算符显示一条记录。

使用 AND 来显示所有姓为 "Carter" 并且名为 "Thomas" 的人：

~~~sql
SELECT * FROM Persons WHERE FirstName='Thomas' AND LastName='Carter'
~~~

使用 OR 来显示所有姓为 "Carter" 或者名为 "Thomas" 的人：

~~~sql
SELECT * FROM Persons WHERE FirstName='Thomas' OR LastName='Carter'
~~~

也可以把 AND 和 OR 结合起来：

~~~sql
SELECT * FROM Persons WHERE (FirstName='Thomas' OR FirstName='William') AND LastName='Carter'
~~~



### SQL ORDER BY 子句

**ORDER BY 语句用于对结果集进行排序**

默认按照升序对记录进行排序。如果希望按照降序，可以使用 DESC 关键字。

以字母顺序显示公司名称：

~~~sql
SELECT Company,OrderNumber FROM Orders ORDER BY Company
~~~

以字母顺序显示公司名称，并以数字顺序显示顺序号：

~~~sql
SELECT Company,OrderNumber FROM Orders ORDER BY Company,OrderNumber
~~~

以逆字母顺序显示公司名称：

~~~sql
SELECT Company,OrderNumber FROM Orders ORDER BY Company DESC
~~~

以逆字母顺序显示公司名称，并以数字顺序显示顺序号：

~~~sql
SELECT Company,OrderNumber FROM Orders ORDER BY Company DESC,OrderNumber ASC
~~~



### SQL INSERT INTO 语句

INSERT INTO 语句用于向表格中插入新的行。

语法：

~~~sql
INSERT INTO 表名称 VALUES (值1,值2,....)
~~~

也可以指定所要插入数据的列：

~~~sql
INSERT INTO table_name (列1,列2,...) VALUES (值1,值2,....)
~~~

例如：

~~~sql
INSERT INTO Persons VALUES ('Gates','Bill','Xuanwumen 10','Beijing')

INSERT INTO Persons (LastName,Address) VALUES('Wilson','Champs-Elysees')
~~~



### SQL UPDATE 语句

UPDATE 语句用于修改表中的数据。

语法：

~~~sql
UPDATE 表名称 SET 列名称 = 新值 WHERE 列名称 = 某值
~~~

例如：

~~~sql
UPDATE Persons SET FirstName='Fred' WHERE LastName='Wilson'
~~~

~~~sql
UPDATE Persons SET Address='Zhongshan 23',City='Nanjing' WHERE LastName='Wilson'
~~~



### SQL DELETE 语句

DELETE 语句用于删除表中的行。

语法：

~~~sql
DELETE FROM 表名称 WHERE 列名称 = 值
~~~

例如：

~~~sql
DELETE FROM Persons WHERE LastName='Wilson'
~~~

可以在不删除表的情况下删除所有的行，这意味着表的结构、属性和索引都是完整的：

~~~sql
DELETE FROM table_name
~~~

或者：

~~~sql
DELETE * FROM table_name
~~~



### SQL TOP 子句

TOP 子句用于规定要返回的记录数目。

语法：

~~~sql
SELECT column_name(s)
FROM table_name
LIMIT number
~~~

例如：

~~~sql
SELECT *
FROM Persons
LIMIT 5
~~~

希望从 Persons 表中选取头两条记录，可以使用下面的 SELECT 语句：

~~~sql
SELECT TOP 2 * FROM Persons
~~~

希望从 Persons 表中选取 50％ 的记录，可以使用下面的 SELECT 语句：

~~~sql
SELECT TOP 50 PERCENT * FROM Persons
~~~



### SQL LIKE 操作符

**LIKE 操作符用于在 WHERE 子句中搜索列中的指定模式。**

语法：

~~~sql
SELECT column_name(s)
FROM table_name
WHERE column_name LIKE pattern
~~~

希望从 Persons 表中选取居住在以 "N" 开始的城市里的人，可以使用下面的 SELECT 语句：

~~~sql
SELECT * FROM Persons
WHERE City LIKE 'N%'
~~~

**提示：**"%"可用于定义通配符（模式中缺少的字母）。

希望从 Persons 表中选取居住在包含 "lon" 的城市里的人，可以使用下面的 SELECT 语句：

~~~sql
SELECT * FROM Persons
WHERE City LIKE '%lon%'
~~~

通过使用 NOT 关键字，可以从 Persons 表中选取居住在**不包含** "lon" 的城市里的人，可以使用下面的 SELECT 语句：

~~~sql
SELECT * FROM Persons
WHERE City NOT LIKE '%lon%'
~~~



### SQL 通配符

在搜索数据库中的数据时，SQL 通配符可以替代一个或多个字符。

SQL 通配符必须与 LIKE 运算符一起使用。

在 SQL 中，可使用以下通配符：

| 通配符         | 描述            |
| ----------- | ------------- |
| %           | 替代一个或多个字符     |
| _           | 仅替代一个字符       |
| [charlist]  | 字符列中的任何单一字符   |
| [!charlist] | 不在字符列中的任何单一字符 |

希望从 Persons 表中选取居住在以 "Ne" 开始的城市里的人，可以使用下面的 SELECT 语句：

~~~sql
SELECT * FROM Persons 
WHERE City LIKE 'Ne%'
~~~

希望从 Persons 表中选取居住在包含 "lond" 的城市里的人，可以使用下面的 SELECT 语句：

~~~sql
SELECT * FROM Persons
WHERE City LIKE '%lond%'
~~~

希望从 Persons 表中选取名字的第一个字符之后是 "eorge"的人，可以使用下面的 SELECT 语句：

~~~sql
SELECT * FROM Persons
WHERE FirstName LIKE '_eorge'
~~~

希望从 Persons 表中选取的这条记录的姓氏以 "C" 开头，然后是一个任意字符，然后是 "r"，然后是任意字符，然后是 "er"，可以使用下面的 SELECT 语句：

~~~sql
SELECT * FROM Persons
WHERE LastName LIKE 'C_r_er'
~~~

希望从 Persons 表中选取居住的城市以 "A" 或 "L" 或 "N" 开头的人，可以使用下面的 SELECT 语句：

~~~sql
SELECT * FROM Persons
WHERE City LIKE '[ALN]%'
~~~

希望从 Persons 表中选取居住的城市**不以** "A" 或 "L" 或 "N" 开头的人，可以使用下面的 SELECT 语句：

~~~sql
SELECT * FROM Persons
WHERE City LIKE '[!ALN]%'
~~~



### SQL  IN 操作符

IN 操作符允许在 WHERE 子句中规定多个值。

语法：

~~~sql
SELECT column_name(s)
FROM table_name
WHERE column_name IN (value1,value2,...)
~~~

希望从 Persons 表中选取姓氏为 Adams 和 Carter 的人，可以使用下面的 SELECT 语句:

~~~sql
SELECT * FROM Persons
WHERE LastName IN ('Adams','Carter')
~~~



### SQL BETWEEN 操作符

**BETWEEN 操作符在 WHERE 子句中使用，作用是选取介于两个值之间的数据范围。**

操作符 BETWEEN ... AND 会选取介于两个值之间的数据范围。这些值可以是数值、文本或日期。

语法：

~~~sql
SELECT column_name(s)
FROM table_name
WHERE column_name
BETWEEN value1 AND value2
~~~

以字母顺序显示介于 "Adams" (包括) 和 "Carter"  (不包括) 之间的人，可以使用下面的 SELECT 语句：

~~~sql
SELECT * FROM Persons
WHERE LastName
BETWEEN 'Adams' AND 'Carter'
~~~

**重要事项：**不同的数据库对 BETWEEN ... AND 操作符的处理方式是有差异的。

如需使用上面的例子显示范围之外的人，可以使用 NOT 操作符：

~~~sql
SELECT * FROM Persons
WHERE LastName
NOT BETWEEN 'Adams' AND 'Carter'
~~~



### SQL Alias (别名)

**通过使用 SQL，可以为列名称和表名称指定别名 (Alias)。**

表的语法：

~~~sql
SELECT column_name(s)
FROM table_name
AS alias_name
~~~

列的语法：

~~~sql
SELECT column_name AS alias_name
FROM table_name
~~~

假设有两个表分别是："Persons" 和 "Product_Orders"。分别为它们指定别名 "p" 和 "po"。现在希望列出 "John Adams" 的所有订单，可以使用下面的 SELECT 语句：

~~~sql
SELECT po.OrderID,p.LastName,p.FirstName
FROM Persons AS p,Product_Orders AS po
WHERE p.LastName='Adams' AND p.FirstName='John'
~~~

不使用别名的 SELECT 语句：

~~~sql
SELECT Product_Orders.OrderID,Persons.LastName,Persons.FirstName
FROM Persons,Product_Orders
WHERE Persons.LastName='Adams' AND Persons.FirstName='John'
~~~

使用列名别名，例如：

~~~sql
SELECT LastName AS Family,FirstName AS Name
FROM Persons
~~~



