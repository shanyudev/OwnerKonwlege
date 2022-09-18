# **Oracle数据库的应用**

# **1.** **名词解释**

**数据：**当今世界是一个充满着数据的互联网世界，充斥着大量的数据。这个互联网世界就是数据世界。数据的来源有很多，比如出行记录、消费记录、浏览的网页、发送的消息等等。**文本，图像、音乐、声音都是数据**。

**数据库：存放数据的仓库**，电子化的文件柜，存储和管理数据的软件。

它的存储空间很大，可以存放百万条、千万条、上亿条数据。但是数据库并不是随意地将数据进行存放，是有一定的规则的，否则查询的效率会很低。

**登录：**数据有安全保密性，不能随便一个人都能看到，查看数据的前提是通过安全认证，这个安全认证就是用户名和密码，如果你用户名和密码输对了，说明这个数据库属于你。**鉴定用户名和密码是否正确的过程称为登录鉴定**。

  总结来说就是：你是谁？

**权限：**通过了登录鉴定并不是说你就可以访问数据了，未必！举个例子：小明的公司有一个数据库，这个数据库存储了客户信息，员工信息，工资信息……这个公司有很多员工，那么无论是谁只要通过了登录鉴定就都能查看员工的工资信息了？员工的工资可是公司的绝密敏感信息啊。答案显然是：不能！

所以要做数据的**权限**控制，**仅让特定的人访问特定的信息**。你拥有这个数据的权限就可以访问这笔数据，否则会遭到拒绝。

总结来说就是：你是能做什么？

**角色：权限的集合称为角色。**或者换种说法：**一组权限叫角色**。

为什么要有角色? 能看到员工工资的人是一群人，而不是一个人，这一群人通常叫做“出纳”。出纳就是一个角色，出纳通常还具有：报销单，对账单，工资表…..数据的操作权限，那么，我们说拥有这一组权限的人就是“出纳”，“出纳”就是角色。

设置角色的目的是为了更方便的把这一组权限赋给某一群人，因为一个一个的为用户赋权限太累了，权限太多，用户也太多。

**存储过程：存储在数据库中供用户调用的程序叫存储过程（数据库中的程序）。**

**游标：**直接的理解是可以（在结果集中）上下游走的光标。

游标的作用就是用于临时存储从数据库中提取的数据块（结果集）。**游标提供了在结果集中一次一行或者多行前进或向后浏览数据的能力**。游标实际就是JDBC当中的ResultSet。

# **2.** **Oracle的安装与配置**

## **2.1** **版本**

Oracle11g，PLSQL_Developer_9.0.5.1648

## **2.2** **安装**

先安装Oracle11g，再安装PLSQL_Developer。

注：Oracle11g要严格按照视频中的步骤安装，新手安装Oracle失败的机率很大！如果Oracle的安装程序和安装目标路径中有汉语，失败的机率是100%。

  如果安装失败则是一件非常麻烦的事，尽可能一次安装成功。

## **2.3** **配置**

启动两个服务（默认自动启动）：

OracleServiceORCL

OracleOraDb11g_home1TNSListener

使用配置助手配置本地连接（默认自动配置好的）。这两项操作不出问题就不做。

# **3.** **用户创建与授权(DCL)**

## **3.1** **用户类型**

登录时可以选择的三种账户类型:sysdba,sysoper,normal

它们之间的区别：

| **账户**    | **描述**     | **功能**                                                     |
| ----------- | ------------ | ------------------------------------------------------------ |
| **sysdba**  | 数据库管理员 | 打开数据库服务器，关闭数据库服务器，备份数据库，恢复数据库，日志归档，会话限制，管理功能，创建数据库 |
| **sysoper** | 数据库操作员 | 打开数据库服务器，关闭数据库服务器， 备份数据库，恢复数据库，日志归档，会话限制 |
| **normal**  | 普通用户     | 无任何权限（只有通过被授权之后才可以对数据库进行操作）       |

## **3.2** **用户**

### **3.2.1** **默认解锁账户**

| **用户名** | **密码**   | **类型**        | **描述**                      |
| ---------- | ---------- | --------------- | ----------------------------- |
| sys        | 安装时设置 | sysdba, sysoper | 级别最高的账户                |
| sysdba     | 安装时设置 | sysdba          | 级别最高的账户                |
| system     | 安装时设置 | normal          | 虽然是普通用户，但拥有dba角色 |

sysdba，sysoper，dba的区别：sysdba，sysoper不依赖于数据库系统的启动，因为它俩是可以启动和关闭数据库系统的。dba必须依赖于数据库系统，数据库工作正常了dba角色才有了存在的基础。

### **3.2.2** **创建账户**

### **3.2.2.1** **创建用户**

sql格式：CREATE USER 用户名 IDENTIFIED BY 密码;例如：CREATE USER nowashing IDENTIFIED BY yujinxiang; 创建了一个用户，用户名是nowashing密码是yujinxiang。

### **3.2.2.2** **修改任意用户的密码**

sql格式：ALTER USER 用户名 IDENTIFIED BY 密码;例如：ALERT USER scott IDENTIFIED BY tiger;将scott的密码修改为tiger。

### **3.2.2.3** **修改当前用户的密码**

sql格式：ALTER USER 用户名 IDENTIFIED BY 新密码 REPLACE 旧密码;

### **3.2.2.4** **删除用户**

sql格式：DROP USER 用户名（CASCADE级联删除拥有对象）例如：DROP USER nowashing;删除掉nowashing这个用户，以及这个用户所拥有的所有的数据对象(权限，表，视图，存储过程……)。

### **3.2.2.5** **查询所有用户**

SELECT * FROM all_users;

## **3.3** **授权**

### **3.3.1** **授权**

sql格式：GRANT 权限/角色列表 TO 用户名 [WITH ADMIN OPTION];例如1：GRANT dba TO nowashing;授权dba角色给nowashing这个用户。后续篇章将权限，角色统称为权限。例如2：权限或角色有多项用逗号分隔：GRANT connect,resource,dba TO nowashing;例如3：被授权用户拥有此权限后是否可以再将此权限授予别人：GRANT dba TO nowashing WITH ADMIN OPTION;不加WITH ADMIN OPTION是不允许授予别人的。例如4：将某授权授予给所有用户：GRANT 权限 TO PUBLIC;

### **3.3.2** **撤消授权**

sql格式：REVOKE 权限/角色列表 FROM 用户例如：REVOKE dba TO nowashing;收回(撤消)dba角色从nowashing这个用户。权限或角色有多项用逗号分隔：REVOKE  connect,resource,dba TO nowashing

### **3.3.3** **查询已拥有的权限**

SELECT * FROM dba_sys_privs; --查询所有用户和角色被授予的系统权限SELECT * FROM user_sys_privs; --查询当前登录用户被授予的系统权限SELECT * FROM user_role_privs—查询当前登录用户拥有的角色

### **3.3.4** **常见系统权限**

系统权限: 允许用户执行特定的数据库动作，如创建表、创建索引、连接实例等。

对象权限: 允许用户操纵一些特定的对象，如读取视图，可更新某些列、执行存储过程等。

Oracle的系统权限有206种（SELECT * FROM system_privilege_map），以下仅列出常用的一些：

| **权限名称**              | **描述**                 |
| ------------------------- | ------------------------ |
| **create session**        | 连接到数据库             |
| **create sequence**       | 创建序列                 |
| **create synonym**        | 创建同义词               |
| **create public synonym** | 创建公用的同义词         |
| **create table**          | 创建表                   |
| **create any table**      | 在任何模式中创建表       |
| **drop table**            | 删除表                   |
| **drop any table**        | 删除任何模式中的表       |
| **create procedure**      | 创建存储过程             |
| **execute any procedure** | 运行任务模式中的存储过程 |
| **create user**           | 创建用户                 |
| **alter** **user**        | 修改用户                 |
| **drop user**             | 删除用户                 |
| **create view**           | 创建视图                 |

模式(schema)：是某个用户拥有所有对象的集合。

### **3.3.5** **常见系统角色**

| **角色名称** | **拥有权限**                                                 |
| ------------ | ------------------------------------------------------------ |
| **dba**      | 所有权限                                                     |
| **connect**  | create session 连接到数据库alter session --修改会话create sequence --建立序列create synonym --建立同义词create view --建立视图create cluster --建立聚簇create database link --建立数据库链接 |
| **resource** | create trigger--建立触发器create sequence  --建立序列create procedure --建立存储过程create cluster --建立聚簇create operator  --建立操作员create index --建立索引create indextype --建立索引类型create table --建立表 |

### **3.3.6** **对象权限**

所谓的对象指的是：表，视图，索引，同义词，存储过程，触发器，序列……

Ø 不同的对象具有不同的对象权限

Ø 对象的拥有者拥有所有权限

Ø 对象的拥有者可以向外分配权限

| **对象权限**         | **表** | **视图** | **序列** | **过程** |
| -------------------- | ------ | -------- | -------- | -------- |
| **查询(select)**     | √      | √        | √        |          |
| **插入(insert)**     | √      | √        |          |          |
| **更新(update)**     | √      | √        |          |          |
| **删除(delete)**     | √      | √        |          |          |
| **修改(alter)**      | √      |          | √        |          |
| **索引(index)**      | √      |          |          |          |
| **关联(references)** | √      |          |          |          |
| **执行(execute)**    |        |          |          | √        |

**3.3.6.1** **授予权限**

sql格式：GRANT 权限列表 ON 对象名 TO 用户名 [WITH GRANT OPTION]例1，授予**郁金香**这个用户，向**学生表****查询，插值，修改，删除**的权限：GRANT **select ,insert,update,delete,execute** ON **t_student** TO yujinxiang;例2，向数据库中所有用户分配权限：GRANT select ON dept TO PUBLIC;

**3.3.6.2** **授予部分字段权限（insert,update）**

sql格式：GRANT 权限名称 (字段列表)  ON 对象名 TO 用户名 [WITH GRANT OPTION]例如，授予**郁金香**这个用户，**修改****学生表**的**学生姓名，学生年龄**的权限：GRANT **update** (**stu_name，student_age**)  ON **t_student** TO **yujinxiang**;

WITH GRANT OPTION 可在授于权限（对象）的同时，允许此用户将此权限授权给别的用户

**3.3.6.3** **查询已拥有权限**

sql格式：SELECT * FROM user_tab_privs_made --查询授出去的对象权限(通常是属主自己查）SELECT * FROM user_tab_privs_recd，SELECT * FROM user_tab_privs --查询当前用户拥有的对象权限

**3.3.6.4** **撤消授权**

sql格式：REVOKE 权限列表 ON 对象名 FROM 用户名;例1，撤消授予**郁金香**这个用户，向**学生表****查询，插值，修改，删除**的权限：REVOKE **select ,insert,update,delete,execute** ON **t_student** FROM yujinxiang;例2，撤消授予**郁金香**这个用户，**修改****学生表**的**学生姓名，学生年龄**的权限：REVOKE **update** ON **t_student** FROM **yujinxiang**;

# **4.** **DDL,DML,DQL,DCL**

## **DDL(Data Definition Language)：**

数据定义语言,用于创建对象 如:CREATE TABLE ,ALTER TABLE,DROP TABLE,CREATE VIEW……

## **DML (Data Manipulation Language)：**

数据操纵语言，如：INSERT INTO,UPDATE,DELETE……

## **DQL (Data Query Language)：**

数据查询语言，如：SELECT……

## **DCL(Data Control Language)：**

数据控制语言，控制特定用户对数据表、存储过程、函数等数据库对象的控制权，由 GRANT 和 REVOKE 两个指令组成。

# **5.** **命名规范**

表名和列名:

Ø 必须以字母开头

Ø 必须在 1–30 个字符之间

Ø 必须只能包含 A–Z, a–z, 0–9, _, $, 和 #

Ø 必须不能和用户定义的其他对象重名

Ø 必须不能是Oracle 的保留字(所有的保留字：select * from v$reserved_words order by keyword asc;)

Ø Oracle默认存储是都存为大写

SQL：

为了构建易读易编的有效语句，其规则和准则如下：

Ø SQL语句是不区分大小写的

Ø SQL语句可以是一行，也可以是多行

Ø 关键字不能在两行之间一分为二或缩写

Ø 子句通常放在单独的行中，这样可以增强可读性并且易于编辑

Ø 合理使用缩进 ( 为了增强可读性)

# **6.** **数据类型**

| **字段类型**     | **中文说明**                   | **限制条件**                     | **其它**                                                     |
| ---------------- | ------------------------------ | -------------------------------- | ------------------------------------------------------------ |
| **CHAR**         | 固定长度字符串                 | 最大长度2000 bytes               |                                                              |
| **VARCHAR2**     | 可变长度的字符串               | 最大长度4000 bytes               | varchar2中文占2字节，英文占1字节。                           |
| **NVARCHAR2**    | 根据字符集而定的可变长度字符串 | 最大长度4000 bytes               | Nvarchar2中英文占一样的字节，具体占多少字节因字符集而定。    |
| **DATE**         | 日期（日-月-年），时间可有可无 | YYYY-MM-DD（HH24:MI:SS）         |                                                              |
| **TIMESTAMP(6)** | 时间戳 （年月日时分秒毫秒）    | YYYY-MM-DD HH24:MI:SS.ff         | 与DATE数据类型相比，TIMESTAMP类型可以精确到微秒，微秒的精确范围为0-9，默认为6 |
| **LONG**         | 超长字符串                     | 最大长度2G                       | 足够存储大部分著作                                           |
| **RAW**          | 固定长度的二进制数据           | 最大长度2000 bytes               | 可存放多媒体图象声音等                                       |
| **LONG RAW**     | 可变长度的二进制数据           | 最大长度2G                       | 同上                                                         |
| **BLOB**         | 二进制数据                     | 最大长度4G                       |                                                              |
| **CLOB**         | 大字符数据                     | 最大长度4G                       | 保存单字节或多字节字符数据，最大值为4G                       |
| **NCLOB**        | 根据字符集而定的字符数据       | 最大长度4G                       | 保存Unicode编码字符数据,最大值为4G。                         |
| **BFILE**        | 存放在数据库外的二进制数据     | 最大长度4G                       |                                                              |
| **NUMBER(P,S)**  | 数字类型                       | P为所有有效数字的位数，S为小数位 | oracle底层只有number为类型并没有decimal和integer这两个类型这两个类型只为oracle和其它数据库之间方便迁移的。 |
| **DECIMAL(P,S)** | 数字类型                       | P为所有有效数字的位数，S为小数位 |                                                              |
| **INTEGER**      | 整数类型                       | 小的整数                         |                                                              |
| **FLOAT**        | 浮点数类型                     | NUMBER(38)，双精度               | 存储近似值                                                   |
| **REAL**         | 实数类型                       | NUMBER(63)，精度更高             | 存储近似值                                                   |

# **7.** **DDL**

## **7.1** **创建表**

基本格式:

CREATE TABLE  表名 ( 列名 类型, 列名 类型, 列名 类型, ……)例如：CREATE TABLE  t_student ( stu_id number(9), stu_name varchar2(50), stu_age number(2), stu_date date ……)

从已有的数据创建表：

CREATE TABLE stud2 AS SELECT * FROM stud

## **7.2** **修改表**

修改表名称:

RENAME  表名 to 新表名

给表添加备注:

COMMENT ON TABLE 表名 IS  '表的说明'

给列添加备注:

COMMENT ON COLUMN 表名.列名 IS '列的说明'

修改表结构：

ALTER TABLE 表名 ……(后面要跟的语句取决写下面的新增/修改/删除列的应用场景)

给表增加列: 

ALTER TABLE 表名 ADD 列名 列类型

删除列：

ALTER TABLE 表名 DROP COLUMN 列名

修改列：

修改列名和列类型ALTER TABLE 表名  MODIFY 列名 列类型更改列名 ALTER TABLE 表名 RENAME COLUMN 旧列名 to 新列名删除列  ALTER TABLE 表名 DROP COLUMN 列名

## **7.3** **删除表**

DROP 表名

## **7.4** **查询用户所有表**

SELECT * FROM user_tables

## **7.5** **查看表结构**

SELECT table_name,column_name, data_type, data_length, data_precision FROM user_tab_columns;

## **7.6** **查看表备注**

SELECT * FROM user_tab_comments

## **7.7** **查看列备注**

SELECT * FROM user_col_comments

# **8.** **DML**

## **8.1** **向表中写入新数据行(insert)**

指定列名插值：

INSERT INTO 表名 (列名1，列名2，列名3...) VLAUES (列1的值，列2的值，列3的值...)

向所有列插值：

INSERT INTO 表名 VLAUES (列1的值，列2的值，列3的值…)

插入的结果从其它表查出来：

INSERT INTO temp2  SELECT * FROM temp2

插值时使用序列：

INSERT INTO 表名 (id，列名2，列名3...) VLAUES (序列名称.NEXTVAL，列2的值，列3的值...)

注：序列要先创建，创建和使用序列参考序列章节

## **8.2** **修改数据行(update)**

基本格式：

UPDATE 表名 SET 字段1=字段1的值，字段2=字段2的值…… WHERE 字段1=字段1的值 [and/or] 字段2=字段2的值……例1：UPDATE t_student  SET age=20,name=’张三’ WHERE id=1更新学生表当中id为1的学生信息为：年龄20岁，姓名张三例2：UPDATE t_student  SET age=20,sex=’女’ WHERE name=’张三’ AND phone=’13012345678’更新学生表当中姓名为张三，并且手机号为13012345678为学生信息为：年龄20岁，姓名张三

## **8.3** **删除数据行(delete)**

基本格式：

DELETE FROM 表名 WHERE字段1=字段1的值 [AND/OR] 字段2=字段2的值……例如：DELETE FROM t_student WHERE id=1删除id为1的这个学生的信息

## **8.4** **截断表(****truncate****)**

基本格式：

TRUNCATE TABLE表名

截断表指的是将表恢复到初始新建状态，从表中删除所有的行和重置表的存储区域。

## **8.5** **drop,delete,truncate的区别**

 1.delete和truncate都是只删除表的数据，而不删除表的结构（定义），drop删除数据和定义。

 2.delete语句是dml,事务提交之后才生效。truncate和drop是ddl，操作立即生效不需提交事务,不能回滚。

 3.delete如果有相应的trigger,执行的时候将被触发。truncate,drop操作不触发trigger。

 4.truncate 将表重置回到最开始。而delete只删除数据，不释放存储空间，等待写入新的数据来覆盖旧的数据。 

 5.速度一般来说: drop> truncate > delete。

 6.truncate只能删除全部的数据，而delete可以通过where条件来筛选要删除的数据。

# **9.** **DQL**

## **9.1** **简单查询**

格式：

select 子句 from 子句 [where 子句 [order by cloumns [asc/desc]]]

◆select子句选择要查询的信息（特定列，列所有 * 。使用逗号隔开，可以调整列的先后显示顺序）

例1,查询所有学生信息：SELECT * FROM t_student例2,查询所有学生的id,name,age信息：SELECT id,name,age FROM t_student

 ◆from子句选择要查询的表，视图，结果集，dual

例1，from作用在表上：SELECT * FROM t_student例2，from作用在视图上(视图后面讲)：SELECT * FROM v_student例3，from作用在结果集上：SELECT * FROM (SELECT * FROM t_student)例4，from作用在dual上：SELECT sysdate FROM DUALSELECT 1+1 FROM DUAL解释：DUAL用在没有查询目标的SELECT语句块中

◆where子句限定限制返回的行,where子句中的多个条件使用 and\or连接。

例1，查询id为1的学生信息：SELECT * FROM t_student WHERE id=1例2，查询id为1并且name为张三的学生信息：SELECT * FROM t_student WHERE id=1 and name=’张三’例3，查询name为张三或年龄大于18岁的学生信息：SELECT * FROM t_student WHERE id=1 or age>18

★ 字符类型要用’’括起来，date类型要用to_date函数，时间戳类型要用to_datestamp。字符值区分大小写。

 ★ 当需要使用’时，需要使用’来转义，例如一个单引号’’，两个单引号 ‘’’’

## **9.2** **查询中使用运算符**

sql当中可以使用算术表达式，表达式一般出现在select子句和where子句中。算术表达式可以包含列名、常数值和算术运算符。

1.算术运算符 + - * / 。

  算术运算符的优先顺序：

 ◆乘和除的运算优先级高于加和减

 ◆优先级相同的运算符是从左到右进行运算的

 ◆可以使用小括号来强制语句做出优先运算，并使语句运算顺序更为清晰

 ◆日期计算 也可以使用+ -，单位为天（如:select sysdate + 1 from  dual）

2.连接符 || 或 concat(arg1,arg2)

3.比较运算符 < ,>  ,<= ,>= ,=,!=,<>,is,is not ,in ,not in,any,all,between,like 

 ◆LIKE：匹配字符串

 ◆IN：匹配列表值

 ◆BETWEEN：匹配范围值

 ◆IS NULL：匹配空值

 ◆IS NAN：匹配非数字值

◆NOT 取反

 ◆ANY 任意一个  (select 1 from dual  where 1 > any(1,2,3,4) ) 

 ◆ALL 所有的  (select 1 from dual  where 1 > all(1,2,3,4) ) 

4.通配符

下划线(_)：表示匹配1个字符

百分号(%)：表示匹配任意个字符

5.转义

如果要查询实际的下划线或百分号就需要使用ESCAPE选项区分通配符

ESCAPE选项告知数据库如何区分通配符和要匹配的字符，例如：

SELECT ename  FROM  emp  WHERE ename  LIKE  '%\%%'  ESCAPE  '\'

6.处理NULL值

NULL值表示未知的值。它是一个特殊的值，但并不是空字符串，NULL值表示该列是未知的。

NVL(列名, 0)，当时列为NULL时，返回0 

7.as 可以给列名起一个别名，并且在返回记录时使用别名返回。默认oracle将别名转为大写。如果想自定义别名可以在用""将别名括起来。as可以省略。

8.DISTINCT 删除重复的行记录，union ,union all

9.ORDER BY，排序（基本语句: ORDER BY 表达式1 ASC/DESC,表达式2 ASC/DESC......），多个表达式之间用逗号隔开

 ◆ASC 升序，可省略

 ◆DESC 降序

10.使用用户输入的参数

语法：&参数名

\11. case 列名 

​      when 值1 then 显示结果1

​      when 值2 then 显示结果2

​      ……

​      else 结果

​      end 

## **9.3** **分页查询**

设：page_no为当前页号，count每页记录数

select s2.* from 

(

  select s1.*,rownum rn from (

​     select s.* from student s  order by s.id

  ) s1 

) s2  where rn between 1 + (&page_no - 1) * &count  and  + &page_no * &count

## **9.4** **多表查询**

例题：查询出所有学生信息以及此学生的导员姓名 

### **9.4.1** **交叉连接（笛卡尔积）**

select * from a,b

或

select * from a cross join b

### **9.4.2** **内连接**

select * from a,b where a.fk = b.pk

或

select * from a [inner] join b on a.fk = b.pk

内连接又称等值连接

### **9.4.3** **自然连接**

select * from a natural join b

自然连接是内连接当中极其特殊的一种，它会自动关联两表当中字段名称和字段类型一模一样的值，也就是说两表当中字段名称和字段类型一模一样的情况下，它会自动加上 on a.fk = b.pk。

自然连接会去除重复的字段

### **9.4.4** **外连接**

**9.4.4.1** **左外连接**

select * from a left [outer] join b on a.pk = b.fk

**9.4.4.2** **右外连接**

select * from a right[outer] join b on a.pk = b.fk

**9.4.4.3** **全外连接**

select * from a full [outer] join b on a.pk = b.fk

**9.4.5** **子查询**

select a.*,(select b.x from b where b.fk=a.pk)  from a

**9.4.6** **exists运算符**

 select 1 from dual  where exists (select 1 from dual where 1> any(0,2,3,4))

 查询有挂科的学生: select s.* from student s where exists (select 1 from subject su where su.score < 60 and su.stu_id = s.id)

# **10.** **函数**

## **10.1** **单行函数（单行函数只处理单个行，并且为每行返回一个结果）**

 1.1字符函数

 ◆CONCAT(x,y),将x和y拼接起来，并返回新字符串

 ◆INITCAP(x)将字符串转换为每个词首字母为大写，其他字母为小写

 ◆INSTR(str,  find_string  [,  start]  [,  count])

  返回指定字符串find_string在str中的位置。可以指定开始搜索的位置start，也可以提供该字符串出现的次数(第)count（次）。

  start和count默认为1，表示从字符串开始的位置开始搜索，并返回第一次出现的位置

 ◆LENGTH(x) 返回表达式中的字符数

 ◆LOWER(x) ,UPPER(x) 转换大小写

 ◆LPAD(x,  width  [,  pad_string])，RPAD(x,  width  [,  pad_string]) 在字符串左/右侧填充pad_string字符，以使总字符宽度为width，默认填充空格,汉字占2位

 ◆LTRIM(x [,  trim_string]),RTRIM(x [,  trim_string]),TRIM(trim_string FROM x)，从x字符串左/右侧/两端去除所有的trim_string字符串，默认去除空格

 ◆NVL(x,  value)  将一个NULL值转换为另外一个值。如果x是NULL值的话返回value值，否则返回x值本身

 ◆NVL2(x,  value1,  value2) 如果x不为NULL值，返回value1，否则返回value2

 ◆REPLACE(x,  search_string,  replace_string) 从字符串x中搜索search_string字符串，并使用replace_string字符串替换

 ◆SUBSTR(x,  start  [,  length]) 返回字符串中的指定的字符，

  ★从start个位置开始

  ★长度为length个字符

  ★如果length省略,则将返回一直到字符串末尾的所有字符

★如果start是负数，则从末尾开始算起

1.2数字函数

 ◆ABS(value) 返回value的绝对值

 ◆CEIL(value) 返回大于或等于value的最小整数 如: SELECT CEIL(5.8),CEIL(-5.2) FROM dual;  返回：6 和 -5

 ◆FLOOR(value) 返回小于或等于value的最大整数  如: SELECT FLOOR(5.8),FLOOR(-5.2) FROM dual; 返回：5 和 -6

 ◆POWER(value,n) 返回value的n次幂

 ◆MOD(m,n) 返回m和n取余数的结果

 ◆SQRT(value) 对value进行开平方

 ◆TRUNC(value,n) 对value进行截断

 ★ n>0，保留n位小数； TRUNC(5.75，1) ==> 5.7

 ★ n<0，则保留-n位整数位(小数点左边指定位数后面的部分截去，均以0记)； TRUNC(5.75，-1) ==> 0  截整数部分，只保留整数，自右往左截-n位

 ★ n=0，则去掉小数部分 TRUNC(5.75，0) ==> 5 

 ◆ROUND(value[,n]) 对value进行四舍五入，保存小数点右侧的n位。

 ★ n>0，保留n位小数；ROUND(5.75,1)==> 5.8

 ★ n<0，则保留-n位整数位(小数点左边指定位数后面的部分四舍五入)； ROUND(5.75,-1) ==> 10

 ★ n=0，则去掉小数部分 ROUND(5.75)==> 5

   如果n不填，则n默认为0.

1.3转换函数 （将值从一种类型转换成另外一种类型，或者从一种格式转换为另外一种格式）

 ◆TO_CHAR(x [,  format])  将x转化为字符串。 format为转换的格式，可以为数字格式或日期格式

 ★数字格式  SELECT TO_CHAR(12345.67, '99,999.99') FROM dual;

  ☆ 9 确定宽度，前导补空，后导补0

  ☆ 0 确定宽度，前后导都补0

  ☆ $ 美元

  ☆ L 当地的货币符号

  ☆ . 小数点

  ☆ ,分隔符

 ★日期格式  select to_char(sysdate,'yyyy-mm-dd hh24:mi:ss') from dual

  ☆ q 季度1-4 

  ☆ ww 年里面的第几周

  ☆ 当前时间减去7分钟的时间  sysdate - interval '7' MINUTE 

  ☆ 当前时间减去7小时的时间  sysdate - interval '7' hour

  ☆ 当前时间减去7天的时间   sysdate - interval '7' day

  ☆ 当前时间减去7月的时间   sysdate - interval '7' month

  ☆ 当前时间减去7年的时间   sysdate - interval '7' year

  ☆ 时间间隔乘以一个数字    sysdate - 8*interval '7' hour

◆TO_DATE(x [,format]) 将x字符串转换为日期

  规则同TO_CHAR

  ☆  TO_DATE('2012-3-15','YYYY-MM-DD‘) 

◆TO_NUMBER(x [,  format]) 将x转换为数字。可以指定format格式

  ☆ TO_NUMBER('970.13') + 25.5 

  ☆ TO_NUMBER('$12,345.67', '$99,999.99') ,  TO_NUMBER('￥12,345.67', 'L99,999.99')

☆ TO_NUMBER(to_char(sysdate, 'yyyy'))

◆CAST(x  AS  type) 将x转换为指定的兼容的数据库类型

☆ CAST(12345.67 AS VARCHAR2(10) )

 ☆ CAST('05-7月-07' AS DATE)

☆ CAST(12345.678 AS NUMBER(10,2))	

## **10.2** **聚集函数（亦称分组函数、聚合函数。聚集函数可以对行集进行操作，并且为每组给出一个结果）**

格式：select 组函数（表达式）/表达式 from 子句 group by 表达式

注意：非group by 中出现的表达式 如果要出现在select当中，必须使用组函数

 ★ 聚集函数可以使用任何有效的表达式

 ★ NULL值在聚集函数中将被忽略

 ★ 可以在聚集函数中使用DISTINCT关键字，排除重复值

★ 筛选条件如果使用了组函数，则应使用having筛选

 ◆AVG(x)：返回x的平均值

 ◆COUNT(x)：返回统计的行数

 ◆MAX(x)：返回x的最大值

 ◆MIN(x)：返回x的最小值

 ◆SUM(x)：返回x的总计值

 ◆MEDIAN(x)：返回中间值

 ◆STDDEV(x)：返回标准偏差

 ◆VARIANCE(x)：返回x的方差

  聚集函数的分组应用，有时需要对表中的行进行分组，然后统计每组的信息：

 例如：

1.统计学生的人数，按性别进行统计，可以使用GROUP BY进行分组  SELECT sex  FROM student GROUP BY sex

\2. 统计每个导师带的所有学生的每个科目的平均/最高/最低/总成绩，并按对应成绩排序,并且只要平均成绩大于60分的记录

\3. 统计每个商品分类下的所有商品的每个月份的平均/最高/最低/总销量，并按对应销量排序,并且只要平均销量低于10000件的记录

\4. 统计每个员工的每年里哪个一月工资最低（如果有不同的月都是最低工资，则取离当前时间较近的一个月），并且显示此月工资是多少（每月工资不固定）

# **11.** **数据完整性(约束)**

数据完整性(Data Integrity)是指数据的精确性(Accuracy)和可靠性(Reliability)，它是防止数据库中存在不符合语义规定的数据和因错误信息的输入造成无效操作或错误信息而提出的。

例如：

1.数据类型 年龄使用什么类型？

2.格式是否正确 身份证号，可以是15位也可以是18位

3.范围 性别可选择“男”、“女”

4.是否允许重复 银行卡号不允许重复

数据完整性分为三类：

## **11.1** **实体完整性（Entity Integrity）**

 实体完整性要求每一个表中的主键字段都不能为空（非空）或者重复的值（唯一）。

 ★ 唯一约束(UNIQUE)

 ★ 主键约束(PRIMARY KEY)

隐含非空约束和唯一约束

## **11.2** **域完整性（Domain Integrity）**

 域完整性指列的值域的完整性。如数据类型、格式、值域范围、是否允许空值等

 ★限制数据类型（检查约束 CHECK） 

示例:

1）check(列名 in(值1,值2......))  

2）check (列名>=0 and 列名<=100) 

3）check(name like 'M%')

 ★默认值 (DEFAULT)  没有约束名称，也不能用CONSTRAINT关键字

 ★非空约束(NOT NULL) 没有约束名称，也不能用CONSTRAINT关键字

## **11.3** **参照完整性(Referential Integrity)**

 外键约束(FOREIGN KEY)  

完整语句：

CONSTRAINT 约束名称 FOREIGN KEY (列名)  REFERENCES 引用表名 (引用列名)

建表语句：

Create table 表名(列 类型 REFERENCES 引用表名 (引用主键)) 注意，没有 CONSTRAINT 约束名称 FOREIGN KEY,也就是说：不支持对约束命名，也不用写出FOREIGN KEY

★当更新、删除、插入一个表中的数据时，通过参照引用相互关联的另一个表中的数据，来检查对表的数据操作是否正确。

 ★引用完整性要求关系中不允许引用不存在的实体。

 ★引用完整性与实体完整性是关系模型必须满足的完整性约束条件。

## **11.4** **添加约束**

\1. 先建表，后创建约束

ALTER TABLE 表名 ADD CONSTRAINT 约束名称 约束类型  (列名) 

   \2. 建表时直接加约束(不适用外键约束，外键有它自已的格式):

​     CREATE TABLE表名(				列名 类型 [CONSTRAINT 约束名称] 约束类型)     CREATE TABLE表名(				列名 类型 ,      [CONSTRAINT 约束名称] 约束类型(列名))     CREATE TABLE表名(				列名 类型 约束类型  --这种格式仅适用于非空和默认值,非空和默认值可以同时存在，例如：sex number(1) not null defalut 1)

## **11.5** **删除约束**

alter table 表名 drop CONSTRAINT 约束名称

## **11.6** **启用约束**

alter table 表名  MODIFY CONSTRAINT 约束名称 enablealter table 表名  MODIFY 列名  not nullalter table 表名  MODIFY 列名  default 值

## **11.7** **禁用约束**

alter table 表名  MODIFY CONSTRAINT 约束名称 disablealter table 表名  MODIFY 列名  nullalter table 表名  MODIFY 列名  default null

## **11.8** **查询约束**

SELECT TABLE_NAME , CONSTRAINT_NAME，CONSTRAINT_TYPE,STATUS FROM USER_CONSTRAINTSSELECT * FROM USER_CONS_COLUMNS

# **12.** **级联**

级联删除

在创建外键约束时可以添加ON DELETE CASCADE选项，那么当主表的数据被删除时，子表对应的行同样也自动被删除。

级联更新

在创建外键约束时可以添加ON DELETE SET NULL选项，那么当主表的数据被删除时，子表匹配的相关行的列会被设置为NULL值，而不是被删除。

# **13.** **范式**

第一范式：

对于表中的每一行，必须且仅仅有唯一的行值。在一行中的每一列仅有唯一的值并且具有原子性。

◆**有主键（必须且仅仅有唯一的行值）**

◆**字段不可以再拆分（每一列仅有唯一的值并且具有原子性）**

  比如，联系方式：24040486@qq.com，18502999999 不符哈第一范式。因为联系方式可以拆分为Email和电话。并且具有两个值。

第二范式：

非主键列是主键的子集，非主键列活动必须完全依赖整个主键。

◆**表不可以再拆分(表的原子性)**

非主键列是主键的子集不相关的信息不要放到一张表，确保表中的每列都和主键相关。 错误示例：

| STU_ID | STU_NAME | STU_AGE | TEA_NAME | TEA_SEX |
| ------ | -------- | ------- | -------- | ------- |
| 1      | 张三     | 20      | 张老师   | 男      |
| 2      | 李四     | 20      | 张老师   | 男      |
| 3      | 王五     | 20      | 李老师   | 女      |
| 4      | 赵六     | 20      | 李老师   | 女      |

两处不合理：

 1.教师跟学生是不相关的两种信息，不应放在一张表里 

 2.教师姓名和教师性别和学生存在传递关系，导致教师信息重复

拆分成两张表，满足第二范式后，区别一目了然：

| STU_ID | STU_NAME | STU_AGE | TEA_ID |
| ------ | -------- | ------- | ------ |
| 1      | 张三     | 20      | 1      |
| 2      | 李四     | 20      | 1      |
| 3      | 王五     | 20      | 2      |
| 4      | 赵六     | 20      | 2      |

| TEA_ID | TEA_NAME | TEA_SEX |
| ------ | -------- | ------- |
| 1      | 张老师   | 男      |
| 2      | 李老师   | 女      |

第三范式：

◆非主键列互不依赖

例如：

| STU_ID | STU_NAME | STU_SCORE | SCORE_LEVEL |
| ------ | -------- | --------- | ----------- |
| 1      | 张三     | 100       | 优秀        |
| 2      | 李四     | 80        | 良好        |
| 3      | 王五     | 80        | 良好        |
| 4      | 赵六     | 60        | 及格        |

分数和等级都是非主属性，但确产生了依赖。

消除依赖：

| STU_ID | STU_NAME | STU_SCORE | SCORE_LEVEL |
| ------ | -------- | --------- | ----------- |
| 1      | 张三     | 100       | 1           |
| 2      | 李四     | 80        | 2           |
| 3      | 王五     | 80        | 2           |
| 4      | 赵六     | 60        | 3           |

| LEVEL_ID | SCORE_LEVEL |
| -------- | ----------- |
| 1        | 优秀        |
| 2        | 良好        |
| 3        | 及格        |
| 4        | 不及格      |

**14.** **表关系设计**

★一对多

多的一方引用一的一方

★一对一

两表共用主键

★多对多

通过第三张表实现，建立两个一对多的关系

# **15.** **PLSQL**

PL/SQL(Procedural Language/Structured Query Language，过程语言/结构化查询语言)

## **15.1** **PL/SQL基本格式**

PL/SQL的块由变量声明、程序代码和异常处理代码3部分组成：

DECLARE (声明)

 声明一些变量、常量、用户定义的数据类型及游标:

 name varchar(30); --声明时不设置值

 name varchar(30):=‘Jack’;--声明带有默认值

 name preson.name%type; --直接引用一个表的数据类型

BEGIN （主体）

 主程序体，在这里可以加入各种合法语句

EXCEPTION （异常处理）

 异常处理程序，当程序中出现错误时执行这一部分

END （结束）

## **15.2** **PL/SQL有效字符**

 1.大小写英文字母

 2.0~9的阿拉伯数字

 3.下划线

 4.操作符，包括+、-、*、/、<、>、!、=、@、%等

 5.最大长度为30个字符，不区分大小写，但建议适当使用大小写，增加程序的可读性

## **15.3** **变/常量声明**

Declare 声明变量（char, varchar2, date, number, boolean, long,integer,type,rowtype)

 varl       char(15);

 married      boolean := true;

 psal       number(7,2);

 my_name      emp.ename%type;  引用型变量，即my_name的类型与emp表中ename列的类型一样

 emp_rec      emp%row type;   记录集型变量

 记录变量分量的引用： emp_rec.ename:='ADAMS';

声明常量:  常量名 constant 数据类型 := 值; 

 常量一旦定义，在以后的使用中其值不再改变，一些固定的大小为了防止有人改变，定义成常量。

 例如 pass_score constant INTEGER := 60 ;

## **15.4** **输出**

set severoutput on --SQL*Plus需要, PL/SQL Developer不需要

declare

  name varchar(10):='HelloWorld';

 begin

   dbms_output.put('test:'); --不换行输出，不能单独工作

   dbms_output.put_line(name); --换行输出

 end;

## **15.5** **接收用户的输入赋值 &变量名称**

  declare

  begin

​      dbms_output.put_line('你输入的值是:' || '&word');

end;

## **15.6** **into赋值**

用into关键字可以将查询结果的值，赋给变量:

declare

  name varchar2(100);

  id number;

 begin

   select id,name into id,name from student where id = 1;

   dbms_output.put_line('id的值:'||id ||',name的值:'|| name);

 end;

## **15.7** **流程控制**

### **15.7.1** **if****…****then****…**

if...then... 

elsif... then... 

else...

end if;

示例1：

declare

  score number := &score;--从控制台输入一个分数

begin

  if score < 60  then

   dbms_output.put_line('不及格');

​    elsif score <= 70 then  /*注意:是elsif不是elseif！*/    

​     dbms_output.put_line('及格');

   elsif score <= 80 then

   dbms_output.put_line('良');

  else  /*注意:else没有then！*/  

​    dbms_output.put_line('优秀');

  end if;  /*注意:所有的成块的代码 都要end！*/  

end;

示例2：

 declare

  score number;

begin

  select score into score from student where id = 2;//从学生表里查分数

  if score < 60  then

   dbms_output.put_line('不及格');

  elsif score <= 70 then

   dbms_output.put_line('及格');

  elsif score <= 80 then

   dbms_output.put_line('良');

  

  else 

​    dbms_output.put_line('优秀');

  end if;

end;

### **15.7.2** **case when****…****then****…**

case  var  

when … then   

when … then  

else 

end (类似于三元表达式，只能赋值，不能有动作)

示例1: sql语句当中

  select  

  case  sex  

   when 0 then '男'

   when 1 then  '女'

   else '未知'

   end

   from student where id = 1;

示例2:PL/SQL当中

declare

  sex number;

  res varchar2(10);

begin

  select sex into sex from student where id = 1;

res := (

  case   

   when sex = 0 then '男'

   when sex = 1 then  '女'

   when sex is null then '没有值'

   else '未知'

   end

);

   dbms_output.put_line(res);

end;

### **15.7.3** **for循环**

for 变量 in 起始值..结束值

loop

end loop

示例1：输出一列*:

declare

begin

for i in 1..10

loop

  dbms_output.put_line('*');

end loop;

end;

### **15.7.4** **loop循环(自动退出)**

loop 

exit when ...; 

end loop;

示例1：输出1-9

declare

i number := 1; 

begin

  

loop

 dbms_output.put_line(i);

 i := i + 1;

 exit when i > 9;

end loop;   

end;

### **15.7.5** **loop循环(手动退出)**

loop  

//exit;  执行exit则退出

end loop  

示例1：输出1-9

declare

y number := 1;  

begin

loop

 dbms_output.put_line(y);

 y := y + 1;

 

 if y > 9 then 

  exit;

 end if;

end loop;   

end;

### **15.7.6** **while循环**

WHILE...

LOOP...

END LOOP

示例1：输出1-9

declare

y number := 1;  

begin

WHILE y <= 9

LOOP  

  dbms_output.put_line(y);

   y := y + 1; 

END LOOP;

end;

## **15.8** **异常处理**

### **15.8.1** **预定义异常**

DECLARE

BEGIN

......

EXCEPTION

​    WHEN 异常名称 THEN

​       --异常处理语句

--异常可使用SQLCODE输出错误编码，以及使用SQLERRM来输出错误信息：

dbms_output.put_line('值类型错误' || SQLCODE || SQLERRM);

​    WHEN 异常名称 THEN

​       异常处理语句

​    ………

​    WHEN OTHERS THEN

​       异常处理语句

END;

所有异常种类：

ACCESS_INTO_NULL  在未初始化对象时出现

CASE_NOT_FOUNF  在CASE语句中的选项与用户输入的数据不匹配时出现

COLLECTION_IS_NULL 在给尚未初始化的表或数组赋值时出现

CURSOR_ALREADY_OPEN 用户试图重新打开已经打开的游标时出现。在重新打开游标前必须先将其关闭

INVALID_CURSOR 在执行非法游标运算(如fetch一个尚未打开的游标)时出现

LOGIN_DENIED 输入的用户名或密码无效时出现

SOTRAGE_ERROR 在内存损坏或PL/SQL耗尽内存时出现

**DUP_VAL_ON_INDEX** 用户试图将重复的(duplicate)值存储在使用唯一索引的数据库列中时出现(insert into student (id) values(1) 1为已存在的id)

**INVALID_NUMBER** 将字符串转换为数字时出现(select id into i from student where name = 3)

**NO_DATA_FOUND** 在表中不存在请求的行时出现(select id into i  from student where id = -1)

**TOO_MANY_ROWS** 在执行SELECT INTO语句后返回多行时出现(select id into i  from student)

**VALUE_ERROR** 变量中的列值超出变量的大小或类型(i number(1); i:='a';)

**ZERO_DIVIDE** 以零做除数时出现(i number; i:=1/0)

示例：

DECLARE

i number;

str varchar2(10) := '1';

BEGIN

 select id into i from student; 

EXCEPTION

​    WHEN VALUE_ERROR THEN

​       dbms_output.put_line('值类型错误' || SQLCODE || SQLERRM);

​    WHEN ZERO_DIVIDE THEN

​       dbms_output.put_line('0不能做被除数' || SQLCODE || SQLERRM);

​    WHEN TOO_MANY_ROWS THEN

​       dbms_output.put_line('返回了太多行，i只能接收一个' || SQLCODE || SQLERRM);

​    WHEN NO_DATA_FOUND THEN

​       dbms_output.put_line('没有返回行，i无法完成赋值' || SQLCODE || SQLERRM);  

​    WHEN  ACCESS_INTO_NULL THEN

​       dbms_output.put_line('变量没有初始化' || SQLCODE || SQLERRM);

​     WHEN  INVALID_NUMBER THEN

​       dbms_output.put_line('数字不能转为字符串' || SQLCODE || SQLERRM);  

​         

​    WHEN  DUP_VAL_ON_INDEX THEN

​       dbms_output.put_line('索引重复' || SQLCODE || SQLERRM);

   WHEN OTHERS THEN

​     dbms_output.put_line('未知错误' || SQLCODE || SQLERRM);    

END;

**15.8.2** **非预定义异常(自定义异常)**

Oracle允许自定义的错误代码的范围为-20000 -- -20999

示例1：

DECLARE

BEGIN

​     RAISE_APPLICATION_ERROR(-20001,'I am sorry to see you');

EXCEPTION

​     WHEN OTHERS THEN

​     dbms_output.put_line( SQLCODE ||'    '|| SQLERRM);   

END;

示例2：

DECLARE

myexception exception;

BEGIN

RAISE myException;

EXCEPTION

​     WHEN myexception THEN

​        RAISE_APPLICATION_ERROR(-20001,'I am sorry to see you');

END;

# **16.** **存储过程**

**存储过程**：存储在数据库中供用户程序调用的程序叫**存储过程**（**数据库中的程序**）

## **16.1** **创建存储过程**

用CREATE PROCEDURE命令建立存储过程

语法：

​    CREATE [OR REPLACE] PROCEDURE 过程名 [(参数名称1 in/out 参数类型1,参数名称2 in/out 参数类型2...)] AS PLSQL子程序体(begin……end)

## **16.2** **形参声明**

基本格式：参数名称 in/out 参数类型

Ø in类型为输入类型的参数，out类型为输出类型的参数

Ø 过程没有返回值,in类型的参数，只可以接收值，不能再给in类型的参数设置新的值

Ø 利用out参数在过程中实现返回多个值

Ø 声明接收参数的只声明类型，不声明大小

## **16.3** **实参声明**

实参声明跟在as和begin之间即可，不需要declare关键字

## **16.4** **存储过程的调用**

1． exec 过程名称[(参数1，参数2.....)]; PLSQL不支持这种调用

  2． begin 过程名称[(参数1，参数2.....)] end;

## **16.5** **存储过程示例**

\1. 批量向学生表写入100条数据

CREATE OR REPLACE PROCEDURE student_batch_insertASbegin   for i in 1..100   loop     insert into t_student(id,name,sex,age)values(SEQ_STUDENT_ID.Nextval,'学生姓名',0,20);   end loop;end;

\2. 银行转账

create or replace procedure transfer(out_no in varchar2,in_no in varchar2,amont in number)asbegin   *--1.从扣款方账户当中减掉要转账的金额*   update t_bank_no set bank_amont = bank_amont - amont where bank_no = out_no;   *--2.给收款方账户当中加要转账的金额*   update t_bank_no set bank_amont = bank_amont + amont where bank_no = in_no;   commit;*--提交*end;

\3. 银行转账的异常处理+事务

事务指的是：一系列操作，要么全部成功，要么全部失败。

create or replace procedure transfer(out_no in varchar2,in_no in varchar2,amont in number)asi number;begin   *--1.从扣款方账户当中减掉要转账的金额*   update t_bank_no set bank_amont = bank_amont - amont where bank_no = out_no;   *--i := 10 / 0;*   *--2.给收款方账户当中加要转账的金额*   update t_bank_no set bank_amont = bank_amont + amont where bank_no = in_no;   commit; exception       when others then       rollback;end;

# **17.** **游标(Cursor)**

 游标是SQL的一个内存工作区，由系统或用户以变量的形式定义。游标的作用就是用于临时存储从数据库中提取的数据块。 

游标的三种类型：隐式Cursor,显式Cursor,Ref Cursor(动态Cursor) 。

## **17.1** **隐式Cursor**  

隐式Cursor是系统自动打开和关闭Cursor。

   可以通过隐式Cusor的属性来了解DML操作的状态和结果(行数和是否成功)，从而达到流程的控制。

​     隐式和显式Cursor都具有的属性:

​     SQL%ROWCOUNT 整型，代表DML语句成功执行的数据行数

​     SQL%FOUND   布尔型，值为TRUE代表操作成功

​     SQL%NOTFOUND 布尔型，与SQL%FOUND相反

​     SQL%ISOPEN  布尔型，DML执行过程中为真，结束后为假

示例:

declarebegin  update student set remark = null;   --输出操作成功的条数  dbms_output.put_line( SQL%rowcount );  --判断操作是否成功  if SQL%FOUND  then     dbms_output.put_line('success');  else      dbms_output.put_line('failed' );  end if;  --判断游标是否打开（是否正在执行中）  if SQL%ISOPEN  then     dbms_output.put_line('running');  else      dbms_output.put_line('done' );  end if;  end;

## **17.2** **静态Cursor**

​    从数据库中提取多行数据，使用显式Cursor

​    显式游标的运用分为四个步骤： 

​    定义游标---Cursor [Cursor Name] IS; 必须要使用is. 

​    打开游标---Open [Cursor Name]; 

​    操作数据---Fetch [Cursor name]  into 行对象;

​    关闭游标---Close [Cursor Name],这个步骤绝对不可以遗漏

示例1 单行游标:

declare

 cursor student_c is select * from student where id = 1;

 rs student%rowType;

begin

  open student_c;

  fetch student_c into rs;

  dbms_output.put_line(rs.id || ' ' || rs.name); 

   close student_c;

end;

示例2 多行游标：

declare

 cursor student_c is select * from student order by id;

begin

​    --无需打开和关闭，自动打开和关闭

  for rs in student_c loop

   dbms_output.put_line(rs.id || ' ' || rs.name); 

  end loop;

 end;

## **17.3** **动态Cursor（Ref Cursor）** 

运行时才产生的游标，是动态游标。

   Ref cursor的使用步骤: 

   Type 自定义游标类型 is ref cursor; --自定义了一个游标类型，并声明它是一个动态游标

   游标名称 自定义游标类型;       --声明一个游标 它是自定义类型

   Open 游标名称 for sql语句;

   Fetch 游标名称  into 行对象;

   Close 游标名称

示例:

create or replace procedure sigleRowCoursor(id_p in varchar2) is

 cursor student_c is select * from student where id = id_p;

 rs student%rowType;

 type ref_cursor_type  is ref cursor;--自定义了一个游标类型，并声明它是一个动态游标

 college_c ref_cursor_type;

 rs_college college%rowType;

 v_sql varchar2(100);

 begin

  open student_c;

   fetch student_c into rs;

   dbms_output.put_line(rs.id || ' ' || rs.name);

   v_sql := 'select * from college where id = ' || rs.col_id;

   close student_c;

   open college_c for v_sql;

   fetch college_c into rs_college;

   dbms_output.put_line(rs_college.id || ' ' || rs_college.name);

   close college_c;

end;

存储过程返回游标:

create or replace procedure backParamter2 (res out sys_refcursor) is

begin

 open res for select * from student;

end;

运行:

declare

--声明一个系统游标类型的变量

student_cursor sys_refcursor;

--声明一个自定义类型（这个类型是学生表里的行对应的表）

type student_table_type is table of student%rowtype;

--声明一个变量为自定义类型

student_table student_table_type;

begin

  --执行存储过程，此过程返回系统游标

 backParamter2(student_cursor);

 --抓取系统游标（集合）里的数据到学生表里

 fetch student_cursor bulk collect into  student_table;

 --迭代学生表

 for i  in 1..student_table.count 

 loop

   --取学生表里的行对应的列的值

   dbms_output.put_line(student_table(i).id || ',' || student_table(i).name);

   

 end loop;

 --关闭游标

 close student_ref_cursor;

end;

# **18.** **视图**

视图包含行和列，就像一个真实的表。视图中的字段就是来自一个或多个数据库中的真实的表中的字段。我们可以向视图添加 SQL 函数、WHERE 以及 JOIN 语句，我们也可以提交数据，就像这些来自于某个单一的表。

**注释：**数据库的设计和结构不会受到视图中的函数、where 或 join 语句的影响。

创建格式：

CREATE VIEW AS SELECT ……

# **19.** **同义词**

同义词是Oracle对象的别名，使用同义词访问相同的对象,可以为表、视图、存储过程、函数或另一同义词等对象创建同义词,方便访问其它用户的对象，隐藏了对象的身份。

创建格式：

CREATE SYNONYM synonym_name FOR object

# **20.** **序列**

序列就是**有顺序**的**数列**。例如：123456789……

创建序列：

CREATE SEQUENCE 序列名称[ MINVALUE 1 MAXVALUE 999999999999999999999999999 INCREMENT BY 1 START WITH  1]

解释：创建序列，最小值是1，最大值是999999999999999999999999999，增量为1，开始于1。序列调用nextval将触发增量条件。**序列只能向前走，不能向后退！**

使用序列：

SELECT 序列名称.NEXTVAL FROM DUAL

查询序列当前值：

SELECT 序列名称. CURRVAL FROM DUAL

解释：

NEXTVAL就是Next Value，下一个值。

CURRVAL就是Current Value，当前值。

DUAL可以认为是系统的虚表，不存在的表，之所以FROM 后面跟DUAL是因为这条SQL语句的FROM不知该作用在哪张表，当不知道该作用在哪张表时就写DUAL。
