# SQL常用函数

# 1聚合函数:ocean:

- count(col): 表示求指定列的总行数
- max(col): 表示求指定列的最大值
- min(col): 表示求指定列的最小值
- sum(col): 表示求指定列的和
- avg(col): 表示求指定列的平均值

## 1.1group by

``` 
group by有一个原则：就是select 后面的所有列中，没有使用聚合函数的列，必须出现在group by 后面。
```

 注意：
（1）在group by子句中不能使用select子句中定义的列的别名。由于group by子句的执行优先级高于select，因此如果在select中定义的列的别名，group by子句并不知道。
（2）group by子句的结果是随机的

1.2aving和where的区别

### 1.1.1where子句

 作用是这对查询结果进行分组前，将不符合where条件的行去掉，即在分组之前过滤数据，where条件中不能包含聚组函数，使用where条件过滤出特定的行。

### 1.1.2having和where的区别

 作用是筛选满足条件的组，即在分组之后过滤数据，条件中经常包含聚组函数，使用having条件过滤出特定的组，也可以使用多个分组标准进行分组。

例子1：
select 类别,sum(数量） as 数量只和 from A
group by 类别 having sum（数量）>18

例子2:
select 类别,sum(数量）from A
where 数量 gt；8
group by 类别 having sum（数量）gt；10

# 2字符串函数

## 2.1LEN() 函数

LEN() 函数返回文本字段中值的长度。

```
SELECT LEN(column_name) FROM table_name;
```

MySQL 中函数为 LENGTH():

```
SELECT LENGTH(column_name) FROM table_name;
```

## 2.2MID() 函数

MID() 函数用于从文本字段中提取字符。

SELECT MID(column_name,start[,length]) FROM table_name;

| 参数        | 描述                                                        |
| :---------- | :---------------------------------------------------------- |
| column_name | 必需。要提取字符的字段。                                    |
| start       | 必需。规定开始位置（起始值是 1）。                          |
| length      | 可选。要返回的字符数。如果省略，则 MID() 函数返回剩余文本。 |

## 2.3INSTR（）函数 （oracle）

INSTR(源[字符串](https://so.csdn.net/so/search?q=字符串&spm=1001.2101.3001.7020), 要查找的字符串, 从第几个字符开始, 要找到第几个匹配的序号)
返回找到的位置，如果找不到则返回0.
例如：INSTR('CORPORATE FLOOR','OR', 3, 2)中，源字符串为'CORPORATE FLOOR',在字符串中查找'OR'，从第三个字符位置开始查找"OR"，取第三个字后第2个匹配项的位置。

默认查找顺序为从左到右。当起始位置为负数的时候，从右边开始查找。

所以SELECT INSTR('CORPORATE FLOOR', 'OR', -1, 1) "aaa" FROMDUAL的显示结果是 14

## 2.4SUBSTR()函数 (oracle)

oracle的substr函数的用法：
 取得字符串中指定起始位置和长度的字符串 substr( string, start_position, [ length ] )
 如:
  substr('This is a test', 6,2)  would return 'is'
  substr('This is a test',6)  would return 'is a test'
  substr('TechOnTheNet', -3,3)  would return 'Net'
  substr('TechOnTheNet', -6,3)  would return 'The'

# 3普通函数

## 3.1ROUND() 函数

ROUND() 函数用于把数值字段舍入为指定的小数位数。

SQL ROUND() 语法

**SELECT** ROUND(column_name,decimals) **FROM** **TABLE_NAME**;

| 参数        | 描述                         |
| :---------- | :--------------------------- |
| column_name | 必需。要舍入的字段。         |
| decimals    | 可选。规定要返回的小数位数。 |

```
mysql> **SELECT** ROUND(1.298, 1);
    -> 1.3
mysql> **SELECT** ROUND(1.298, 0);
    -> 1
```

## 3.2SQL NOW() 函数

NOW() 函数返回当前系统的日期和时间。

SQL NOW() 语法

SELECT NOW() FROM table_name;

