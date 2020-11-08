# MySQL 基础

## 数据库基础

### 1. 三个范式

{% hint style="info" %}
1NF ：⽆重复的列， 表中的每⼀列不能够再拆分成其他⼏列， 强调的是列的原⼦性.。

2NF ：表必须有⼀个主键，非主键列必须完全依赖于主键， ⽽不能只依赖于主键的⼀部分。

3NF ：⾸先需要满⾜2NF， 另外⾮主键列必须直接依赖于主键， 不能存在传递依赖。 即不能存在： ⾮主键列 A 依赖于⾮主键列 B， ⾮主键列 B 依赖于主键的情况。

2NF **VS** 3NF：2NF强调⾮主键列是否依赖主键，表示可以间接依赖。3NF强调⾮主键列是否直接依赖主键， 不能是那种通过传递关系的依赖。
{% endhint %}

### 2. 存储过程与存储函数的区别

{% hint style="info" %}
存储过程：为了完成特定功能的 SQL 语句集。，编译创建后保存在数据库，等待用户调用

特点：

* 简化重复编写复杂的 SQL
* 提高性能，减少数据在数据库和应用服务器之间的传输。
* 没有返回值，但是可以通过 in 、out 、inout 指定输入输出
{% endhint %}

### 3. 什么是视图以及视图作用

{% hint style="info" %}
视图是一张虚拟的表，只存储视图的定义，不存储对应的数据。视图一般用来查询数据，如若修改数据则会影响到所关联记录的表。

作用：

* 重用 SQL，简化重复编写复杂的 SQL
* 数据安全，通过视图给定受限权限的用户数据
* 更改数据格式和表示。
{% endhint %}

### 4. 超键、候选键、主键、外键是什么？

{% hint style="info" %}
超键：在关系中能唯一标识元组的属性集称为关系模式的超键。例如：身份证是一个超键，姓名\(假设不重复\)是一个超键，（身份证，姓名）是一个超键，超键包含候选键和主键

候选键：是最小超键，即没有冗余元素的超键。例如：单独身份证或单独姓名，如果是身份证号加姓名组合，在不需要姓名的情况下仍然是唯一，此时姓名就是冗余元素。

主键：数据库表唯一和完整标识的数据列或属性的组合。一个数据列只能有一个主键，且主键不能为空值（Null）

外键：在一个表中存在的另一个表的主键称此表的外键。
{% endhint %}

### 5. DDL、DML、DQL、DCL、TCL

{% hint style="info" %}
DDL\(Data Definition Language 数据定义语言\)：负责数据结构定义和数据对象定义，常用DDL：CREATE、ALTER、DROP

DML\(Data Manipulation Language 数据操作语言\)：用于操作数据库对象中包含的数据，常用DML：INSERT、DELETE、UPDATE

DQL\(Data Query Language 数据查询语言\)：主要功能是查询数据，核心指令：SELECT

DCL\(Data Control Language 数据控制语言\)：对数据访问权进行控制的指令，常用DCL：GRANT、REVOKE

TCL\(Transaction Control Language 事务控制语言\)：控制和管理事务以维护SQL语句中数据的完整性，常用TCL：BEGIN、COMMIT、ROLLBACK
{% endhint %}

## SQL 

### 1. drop、truncate、delete区别

{% hint style="info" %}
drop：从数据库中删除表，所有的数据行，索引和权限也会被删除，属于DDL，不可回滚

truncate：表结构还在，删除表中的所有数据，属于DDL，不可回滚

delete：删除表中的数据，表结构还在，属于DML，可回滚
{% endhint %}

### 2. int\(10\)、varchar\(10\) 、char\(10\)区别

{% hint style="info" %}
**`char`**：定长字符串，如果插入数据的长度小于 `char` 的固定长度时，则用空格填充。存取速度要比 `varchar` 快，最多存放 `255` 个字符与编码无关。

**`varchar`**：可变长字符串，插入的数据是多度进行存储，最多存放 `65532` 个字符。

**`char VS varchar`**：`char` 占用磁盘空间大，固定长度存取效率高，`varchar` 占用磁盘空间小，长度不等存取效率低

varchar\(50\)和varchar\(100\)存储相同长度的字符串，两者占用空间一样，当是后者在排序过程耗时更长，其中的50、100代表字符个数

**`int(10)`**：10表示显示的数据的长度，不是存储数据的大小，数据长度 9999999999，占32个字节
{% endhint %}

### 3. 各种 join 的区别

{% hint style="info" %}
INNER JOIN：

* 等值连接：`ON A.id = B.id`
* 非等值连接：`ON A.id > B.id`
* 自连接：`SELECT * FROM A T1 INNER JOIN A T2 ON T1.id = T2.pid`

OUTER JOIN：

* LEFT OUTER JOIN：左边表数据全部查询出来，右边表只查询满足条件的数据，条件不满足，用 `null` 补充
* RIGHT OUTER JOIN：右边表数据全部查询出来，左边表只查询满足条件的数据，条件不满足，用 `null`补充

全连接：

* MySQL不支持全连接，可以使用LEFT JOIN 和UNION和RIGHT JOIN联合使用

```sql
SELECT * FROM A LEFT JOIN B ON A.id=B.id 
UNIONSELECT FROM A RIGHT JOIN B 
ON A.id=B.id
```
{% endhint %}

### 4. HAVING 子句 和 WHERE 的异同点?

{% hint style="info" %}
HAVING：可以使用聚合函数，可以对分组后的结果进行筛选

WHERE：不能使用聚合函数，不能对分组后的结果进行筛选
{% endhint %}

### 5. UNION与UNION ALL的区别

{% hint style="info" %}
UNION ALL：不会合并重复的记录行

UNION：合并时去除重复

UNION 效率高于 UNION ALL
{% endhint %}

### 6. SQL约束条件

{% hint style="info" %}
主键约束：PRIMARY KEY

唯一约束：UNIQUE

非空约束：NOT NULL

外键约束：FOREIGN KEY
{% endhint %}

