**SQL注入简介**

在**OWASP**发布的top10排行榜中**SQL注入漏洞**一直是危害排名极高的漏洞，数据库注入一直是web中一个令人头疼的问题。**一个严重的SQL注入漏洞，可能会直接导致
一家公司破产！**这并不是戏言，其实SQL注入漏洞最主要的形成原因是在进行数据交互中，当前端的数据传入后端进行处理时，由于没有做严格的判断，导致其传入
的“数据”在拼接到SQL语句中之后，由于其特殊性，被当作SQL语句的一部分被执行，从而导致数据库受损（被脱库、被删除、甚至整个服务器权限沦陷）。SQL注入是
一种**非常常见**的数据库攻击手段，SQL注入漏洞也是网络世界中最普遍的漏洞之一。大家也许都听过某某学长通过攻击学校数据库修改自己成绩的事情，这些学长们一般用
的就是SQL注入方法。**SQL注入**其实就是恶意用户通过在表单中填写包含SQL关键字的数据来使数据库执行非常规代码的过程。简单来说，就是数据做了代码才能干的事情。这
个问题的来源是，SQL数据库的操作是通过SQL语句来执行的，而无论是执行代码还是数据项都必须写在SQL语句之中，这就导致如果我们在数据项中加入了某些SQL语句关键字
（比如说SELECT、DROP等等），这些关键字就很可能在数据库写入或读取数据时得到执行。

**SQL注入原理**
SQL注入就是指web应用程序对用户输入的数据合法性没有过滤或者是判断，前端传入的参数是攻击者可以控制，并且参数带入数据库的查询，攻击者可以通过构造恶意的sql语句来实现对数据库的任意操作。
举例说明
```php
$id=$_GET['id']
$sql=SELECT * FROM users WHERE id=$id LIMIT 0,1
```

SQL注入漏洞产生的条件：

- 参数用户可控：前端传入的参数内容由用户控制
- 参数带入数据库的查询：传入的参数拼接到SQL语句，并且带入数据库的查询

**Sql注入漏洞类型**
按照注入点分：
- 数字
- 字符
- 搜索
按照提交方式分：
- GET
- POST
- HEAD
- COOKIE
按照执行效果来分：
- 基于布尔的盲注
- 基于时间的盲注
- 基于报错的注入
- 单引号
- 双引号
- 数字
- 联合查询注入
总的来说有：联合注入、布尔注入、报错注入、时间注入、堆叠注入、二次注入、宽字节注入、cookie注入等等等
**Mysql与Sql注入的相关知识**
3.1 information_schema

- 在MySQL5.0版本后，MySQL默认在数据库中存放一个“information_schema”的数据库，在该库中，我们需要记住三个表名，分别是<mark>schemata，tables，columns</mark>。
- schemata表存储的是该用户创建的所有数据库的库名，需要记住该表中记录数据库名的字段名为<mark>schema_name</mark>。
- tables表存储该用户创建的所有数据库的库名和表名，要记住该表中记录数据库库名和表名的字段分别是<mark>table_schema</mark>、<mark>table_name</mark>
- columns表存储该用户创建的所有数据库的库名、表名、字段名，要记住该表中记录数据库库名、表名、字段名为<mark>table_schema、table_name、columns_name</mark>。
```texttile
schemata表：存放了所有的库名，存放在schema_name

tables表：存放了所有的库名以及对应的表名
                    ||            ||
                table_schema    table_name

columns：库名     =>       表名      =>    字段名
         ||              ||              ||
    table_schema  table_name    column_name
```
查找所有的库名
```texttile
select schema_name from information_schema.SCHEMATA;
```
查找指定库中所有的表名：`security`

```texttile
select table_name from information_schema.tables where table_schema="security";
```

查找指定库中的指定表中的字段：`security`=>`users`

```texttile
select column_name from information_schema.columns 
where table_schema="security" and table_name="users";
```
**数据表的结构**
- 数据库（database）：按照数据结构来组织、存储和管理数据的仓库多个数据表的集
- 数据表（table）：以矩阵方式存储数据，在操作界面中以表格形式展现
- 列(column): 具有相同数据类型的数据的集合
- 行(row): 每一行用来描述某条记录的具体信息
- 值(value): 行的具体信息, 每个值必须与该列的数据类型相同
- 表头(header): 每一列的名称
- 键(key): 键的值在当前列中具有唯一性。
数据库的查询语句
想要查询的值A= select 所属字段名A from 所属表名 where 对应字段名B=值B
**重点需要记住的几个函数**
  - version()；当前mysql的版本
- database();当前网站使用的数据库
- user();当前MySQL的用户
- group_concat()：它的作用是将某一列的多个值合并成一个字符串，并用逗号分隔
- substr()：截取字符串
- ascii(a)：把字符转换成ascii码
- updatexml():用于报错注入
- extractvalue():用于报错注入
**注释符号**
- --空格 空格可以使用+代替 （url编码%23表示注释）
- /**/
- `/*!*/` 内联注释
