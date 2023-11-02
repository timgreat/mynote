## SQL

### 基础语法

**SELECT**用于从数据库中选取数据。

SELECT选出的列可能会包含重复值，此时可以使用**DISTINCT**关键字。**DISTINCT**表示对后面所有的参数进行拼接，取不重复的记录。

**WHERE**用于提取满足指定条件的记录，**WHERE**子句中也可以使用如下运算符：

| 运算符  | 描述                                                       |
| :------ | :--------------------------------------------------------- |
| =       | 等于                                                       |
| <>      | 不等于。**注释：**在 SQL 的一些版本中，该操作符可被写成 != |
| >       | 大于                                                       |
| <       | 小于                                                       |
| >=      | 大于等于                                                   |
| <=      | 小于等于                                                   |
| BETWEEN | 在某个范围内                                               |
| LIKE    | 搜索某种模式                                               |
| IN      | 指定针对某个列的多个可能值                                 |

**AND**与**OR**在**WHERE**中用于组合过滤条件，对于复杂的条件可以使用`()`组合起来。

**ORDER BY**关键字用于对结果及按照一个或多个列排序，在列名后加**ASC**关键字表示升序，加**DESC**关键字表示降序。

**INSERT INTO**语句用于向表中插入新记录：

```sql
INSERT INTO table_name (column1,column2,column3,...)
VALUES (value1,value2,value3,...);
```

如果插入时不指定列名的话，VALUES中要列出全部列的值。

**UPDATE**用于更新表中已存在的记录：

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

如果没有WHERE条件，UPDATE会将每一条记录更新。

*注：在 MySQL 中可以通过设置 sql_safe_updates 这个自带的参数来解决，当该参数开启的情况下，你必须在update 语句后携带 where 条件，否则就会报错。set sql_safe_updates=1; 表示开启该参数。*

**DELETE**用于删除表中的行：

```sql
DELETE FROM table_name
WHERE condition;
```

如果没有WHERE条件，DELETE会将所有记录删除。

### 高级SQL

**SELECT TOP**及**LIMIT**子句用于规定要返回记录的数目。

```sql
SELECT column_name(s)
FROM table_name
LIMIT number;
```

**LIKE**用于在**WHERE**子句中搜索列的指定模式：

```sql
SELECT column1, column2, ...
FROM table_name
WHERE column LIKE pattern;
```

**SQL通配符**：

| 通配符                         | 描述                       |
| :----------------------------- | :------------------------- |
| %                              | 替代 0 个或多个字符        |
| _                              | 替代一个字符               |
| [*charlist*]                   | 字符列中的任何单一字符     |
| [^*charlist*] 或 [!*charlist*] | 不在字符列中的任何单一字符 |

**IN**操作符用于在WHERE子句中规定多个值，还可以使用**NOT INT**取不在规定内的值。

```sql
#选择列值在(value1,value2,...)中的记录
SELECT column1, column2, ...
FROM table_name
WHERE column IN (value1, value2, ...);
```

**BETWEEN**操作符用于选取介于两个值之间的数据范围内的值，**NOT BETWEEN**选取不在范围内的值。*值可以是数值、文本和日期*

```sql
SELECT column1, column2, ...
FROM table_name
WHERE column BETWEEN value1 AND value2;
```

为了增强可读性，可以为表和列指定别名：使用**AS**关键字。进一步的，还可以把多个列结合在一起，给一个别名：

```sql
SELECT name, CONCAT(url, ', ', alexa, ', ', country) AS site_info
FROM Websites;
```

#### SQL连接（JOIN）

**JOIN**（默认为INNER JOIN）用于将多个表的行结合起来：

```sql
SELECT column1, column2, ...
FROM table1
JOIN table2 ON condition;
```

- **INNER JOIN**：如果表中有至少一个匹配，则返回行
- **LEFT JOIN**：即使右表中没有匹配，也从左表返回所有的行
- **RIGHT JOIN**：即使左表中没有匹配，也从右表返回所有的行
- **FULL JOIN**：只要其中一个表中存在匹配，则返回行







