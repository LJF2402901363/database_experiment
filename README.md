

# 修改MySQL数据库实验

## 1.数据库定义与操作



### 1.1创建数据库语句

```
CREATE  DATABASE [IF NOT EXISTS] 数据库名 [选项...];
```

 这里创建一个DBMS的数据库

 create database if not exists DBMS character set gbk;

![img](images\1.jpg) 

这里使用了if not exists 这个条件，意思是如果不存在该数据库则创建，如果已经存在了则不创建。

比如这个DBMS数据库已经被创建，这时候就不会重复创建这个数据库，如果不使用这个if not exists那么直接使用创建数据库语句：create database DBMS character set gbk;

则会报错：

![img](images\2.jpg) 

### 1.2.修改数据库信息：

```bash
 alter database 数据库名 修改选项  修改的新值
```

```
  alter database DBMS character set utf8mb4;
```

![img](images\3.jpg) 

使用查看数据库信息命令查看当前数据库DBMS的信息：

```
 show create database dbms;
```

![img](images\4.jpg) 

 

可以发现，我们刚开始创建的时候这个数据库的编码设置“gbk”，现在已经改变为了“utf8mb4”，然后开始将编码修改为原来的样子：

 

![img](images\5.jpg) 

 

可以发现修改成功了。

### 1.3创建一个临时数据库tempDBMS然后删除

 

#### 1.3.1建表语句：drop database if  exists tempDBMS;

![img](images\6.jpg) 

#### 1.3.2删除数据库：drop database 数据库名

 drop database  tempDBMS;

![img](images\7.jpg) 

 

可以看到该数据库已经被删除了。

### 1.4.创建表

#### 1.4.1新建

```bash
CREATE TABLE employee (employeeID CHAR(6) NOT NULL PRIMARY KEY, name CHAR(10) NOT NULL, education CHAR(4) NOT NULL,birth DATE NOT NULL, gender TINYINT(1) NOT NULL DEFAULT 1,workYear TINYINT(1),address VARCHAR(100),phone CHAR(12), departmentID CHAR(3) REFERENCES department(departmentID));
```

![img](images\8.jpg) 

#### 1.4.2从已有数据中新建：

```bash
CREATE  TABLE [IF NOT EXISTS] 表名  [ ( ) LIKE 已有表名 [ ] ] | [AS ( 表达式 )];
```

#### ![img](images\9.jpg)

![img](images\10.jpg)



### 1.5修改表

#### 1.5.1首先创建一个临时例子的表 person：

```bash
create table if not exists person (id int(8) primary key auto_increment,name varchar(20) not null,password varchar(20) not null,sex tinyint(1) not null,address varchar(128));
```

![img](images\11.jpg)

### 1.5.2修改表名： 

```
rename table  表名  to  新的表名
```

 修改表person的名为newperson： 

![img](images\12.jpg) 

### 1.5.2删除列：

```
alter table table-name drop col-name;
```

 alter table newperson drop sex ;

![img](images\13.jpg) 

可见删除列“sex”成功。

 

#### 1.5.3增加列(单列)：

```
alter table table-name add col-name col-type comment 'xxx';
```

![img](images\14.jpg) 

 

#### 1.5.4增加表字段并指明字段放置为第一列：

```
alter table table-name add col-name col-type COMMENT 'sss' FIRST;
```

alter table newperson add column age int(3) comment "年龄" first;

![img](images\15.jpg) 

 

### 1.5.5增加表字段并指明字段放置为特定列后面：

```
alter table table-name add col-name col-type after col-name-1;
```

 

```
 alter table newperson add column jobName varchar(20) comment "工作名称" after id;
```

![img](images\26.jpg) 

 

#### 1.5.6使用MODIFY修改字段类型：

```
alter table table-name  modify  column col-name col-type;
```

 

```
 alter table newperson modify name varchar(40) not null;
```

![img](images\17.jpg) 

#### 1.5.7使用CHANGE修改字段类型：

```
alter table table-name change col-name col-name col-type;
```

 

```
 alter table newperson change address newAddress varchar(64) not null;
```

![img](images\18.jpg) 

 

#### 1.5.8修改字段的默认值：

```
alter table table-name alter col-name set default 要设置的默认值;
```

```
 alter table newperson alter age set default 18;
```

![img](images\19.jpg) 

#### 1.5.9字段删除默认值：

```
alter table table-name alter col-name drop default;
```

```
alter table newperson alter age drop default;
```

![img](images\20.jpg) 

###  1.6.删除临时表：

```
drop table 表名 [if exists]
 drop table if exists;
```


![img](images\21.jpg)

###  1.7思考与练习

**创建一个基本表的时候，LIKE和AS有何区别？试着对比表数据和表结构。**

创建表时从已有数据中复制：

CREATE  TABLE employee2 LIKE employee ;和CREATE  TABLE employee3 AS (SELECT * FROM employee );的区别：

CREATE  TABLE employee2 LIKE employee ;创建表时复制的是employee 的表结构，但是并没有复制employee 中的数据到employee2中，查询数据时为空。

![img](images\22.jpg) 

![img](images\23.jpg) 

CREATE  TABLE employee3 AS (SELECT * FROM employee );创建表时不仅把employee 的结构复制过来了，其中的数据也复制过来了，employee3 和employee3的数据一致。

## 2数据更新实验

### 2.1数据库定义与操作语言

#### 2.1.1创建一个测试表test用于测试：

```bash
create table if not exists test ( id int(8) primary key auto_increment,name varchar(20) not null,price float,gender tinyint(1) default 0,age int(3),birth date );
```

![img](images\24.jpg) 

#### 2.1.2向test表中插入数据

##### 2.1.2.1插入单条数据

```
INSERT INTO <表名> [ <列名1> [ , … <列名n>] ]
VALUES (值1) [… , (值n) ];
```

语法说明如下。

**<表名>：指定被操作的表名。**

**<列名>：指定需要插入数据的列名。若向表中的所有列插入数据，则全部的列名均可以省略，直接采用 INSERT<表名>VALUES(…) 即可。**

**VALUES 或 VALUE 子句：该子句包含要插入的数据清单。数据清单中数据的顺序要和列的顺序相对应。**

![img](images\25.jpg) 

#### 2.1.3从已有数据中插入：

```
使用 INSERT INTO 表名  SELECT [列名1,列名2,...]  FROM  表名  语句复制表数据
```

INSERT INTO…SELECT…FROM 语句用于快速地从一个或多个表中取出数据，并将这些数据作为行数据插入另一个表中。SELECT 子句返回的是一个查询到的结果集，INSERT 语句将这个结果集插入指定表中，结果集中的每行数据的字段数、字段的数据类型都必须与被操作的表完全一致。

![img](images\27.jpg) 

![img](images\28.jpg) 

#### 2.1.4更新test的内容

```bash
UPDATE <表名> SET 字段 1=值 1 [,字段 2=值 2… ] [WHERE 子句 ]
[ORDER BY 子句] [LIMIT 子句]
```

![img](images\29.jpg)

![img](images\30.jpg) 

![img](images\31.jpg) 

![img](images\32.jpg) 

#### 2.1.5删除数据

```
DELETE FROM <表名> [WHERE 子句] [ORDER BY 子句] [LIMIT 子句]
```

语法说明如下：

**<表名>：指定要删除数据的表名。**

**ORDER BY 子句：可选项。表示删除时，表中各行将按照子句中指定的顺序进行删除。**

**WHERE 子句：可选项。表示为删除操作限定删除条件，若省略该子句，则代表删除该表中的所有行。**

**LIMIT 子句：可选项。用于告知服务器在控制命令被返回到客户端前被删除行的最大值。**

****

**注意：在不使用 WHERE 条件的时候，将删除所有数据。**

 

![img](images\33.jpg) 

![img](images\34.jpg) 

![img](images\35.jpg) 

##  3 数据查询实验

### 3.1单表查询

#### 3.1.1Select 语句，SQL核心，语法格式如下：

```
SELECT [ALL | DISTINCT | DISTINCTROW ][HIGH_PRIORITY]… 
列名表达式 …
[FROM  table_reference ... ]	    /*FROM子句*/
[WHERE 条件]				   /*WHERE子句*/
[GROUP BY {列名| 表达式 | position} [ASC | DESC], ... [WITH ROLLUP]]	     /*GROUP BY子句*/    [HAVING 条件]		         /*HAVING 子句*/
[ORDER BY {列名 | 表达式 | position} [ASC | DESC] , ...]		/*ORDER BY子句*/
[LIMIT {[offset,] row_count|row_count OFFSET offset}]; /*LIMIT子句*/
```

**SQL关键字的执行顺序：**

> **from xx join xx on ------where ------ 定义别名- ------ group by(group by 中不能用聚合函数) ------having（having 可以使用聚合函数 & 别名） ------select distinct-----order by --limit**

#### 3.1.2查询所有employee的数据

![img](images\36.jpg) 

#### 3.1.3通过where条件查询

![img](images\37.jpg) 

![img](images\38.jpg) 

#### 3.1.4通过where多条件查询

![img](images\39.jpg)![img](E:\学习专用\数据库综合实验\images\wps39.jpg) 

![img](images\40.jpg) 

#### 3.1.5聚合函数

![img](images\41.jpg) 

##### 3.1.5.1统计所有行数

![img](images\42.jpg) 

![img](images\43.jpg) 

##### 3.1.5.2查找所有员工中工作时间最长的年数：

![img](images\44.jpg) 

##### 3.1.5.3查找所有员工中工作时间最短的年数：

![img](images\wps45.jpg) 

##### 3.1.5.4统计所有员工工作年长总和

![img](images\wps46.jpg) 

###### 1.3.1.5.5统计员工的平均工作年长

![img](images\wps47.jpg) 

#### 3.1.6分组统计查询

使用 GROUP BY 关键字的语法格式如下：

```
GROUP BY  <字段名>
```

其中，“字段名”表示需要分组的字段名称，多个字段时用逗号隔开。

##### 3.1.6.1按照workYear分组查询

![img](images\wps48.jpg) 

GROUP BY单独使用 GROUP BY 关键字时，查询结果会只显示每个分组的第一条记录。

 

##### 3.1.6.2按照workYear分组并统计每组的记录数

![img](images\wps49.jpg) 

##### 3.1.6.3GROUP BY 关键字可以和 GROUP_CONCAT() 函数一起使用。GROUP_CONCAT() 函数会把每个分组的字段值都显示出来。

![img](images\wps50.jpg) 

#### 3.1.7ORDER BY 关键字主要用来将查询结果中的数据按照一定的顺序进行排序。

其语法格式如下：

```
ORDER BY <字段名> [ASC|DESC]
语法说明如下。
```

字段名：表示需要排序的字段名称，多个字段时用逗号隔开。

ASC|DESC：ASC表示字段按升序排序；DESC表示字段按降序排序。其中ASC为默认值。

 

使用 ORDER BY 关键字应该注意以下几个方面：

ORDER BY 关键字后可以跟子查询。

当排序的字段中存在空值时，ORDER BY 会将该空值作为最小值来对待。

ORDER BY 指定多个字段进行排序时，MySQL 会按照字段的顺序从左到右依次进行排序。

 

##### 3.1.7.1单个字段通过workYear升序排序

![img](images\wps51.jpg) 

##### 3.1.7.2单个字段通过workYear降序排序

 

![img](images\wps52.jpg) 

##### 3.1.7.3首先通过workYear升序排序，然后通过departmentID升序

![img](images\wps53.jpg) 

##### 3.1.7.4LIMIT 是 MySQL 中的一个特殊关键字，用于指定查询结果从哪条记录开始显示，一共显示多少条记录。

LIMIT 关键字有 3 种使用方式，即指定初始位置、不指定初始位置以及与 OFFSET 组合使用。

LIMIT 关键字可以指定查询结果从哪条记录开始显示，显示多少条记录。

LIMIT 指定初始位置的基本语法格式如下：

LIMIT 初始位置，记录数

其中，“初始位置”表示从哪条记录开始显示；“记录数”表示显示记录的条数。第一条记录的位置是 0，第二条记录的位置是 1。后面的记录依次类推。

注意：LIMIT 后的两个参数必须都是正整数。

 

![img](images\wps54.jpg) 

显示查询数据中的前5条数据

![img](images\wps55.jpg) 

从第3个位置开始显示5条数据

 

![img](images\wps56.jpg) 

从第3个位置开始显示5条数据

 

 

### 3.2多表查询

#### 3.2.1全连接查询 employee和department

![img](images\wps57.jpg) 

#### 3.2.2全连接条件查询

![img](images\wps58.jpg) 

 

#### 3.2.3内连接使用 INNER JOIN 关键字连接两张表，并使用 ON 子句来设置连接条件。

如果没有连接条件，INNER JOIN 和 CROSS JOIN 在语法上是等同的，两者可以互换。

内连接的语法格式如下：

```
SELECT <字段名> FROM <表1> INNER JOIN <表2> [ON子句]
```

语法说明如下。

字段名：需要查询的字段名称。

<表1><表2>：需要内连接的表名。

INNER JOIN ：内连接中可以省略 INNER 关键字，只用关键字 JOIN。

ON 子句：用来设置内连接的连接条件。

INNER JOIN 也可以使用 WHERE 子句指定连接条件，但是 INNER JOIN ... ON 语法是官方的标准写法，而且 WHERE 子句在某些时候会影响查询的性能。多个表内连接时，在 FROM 后连续使用 INNER JOIN 或 JOIN 即可。内连接可以查询两个或两个以上的表。

##### 3.2.3.1使用 inner join 

![img](images\wps59.jpg) 

 

##### 3.2.3.3使用inner join on 查询

![img](images\wps60.jpg) 

 

#### 3.2.4查询每个雇员的情况及其薪水情况

![img](images\wps61.jpg) 

 

#### 3.2.5使用内连接查询名字为“刘明”的员工所在部门

![img](images\wps62.jpg) 

#### 3.2.6查找财务部收入在2000元以上的雇员姓名和薪水详情

![img](images\wps63.jpg) 

 

#### 3.2.7查询财务部雇员的最高和最低实际收入

![img](images\wps64.jpg) 

#### 3.2.8查询employee中男性和女性的人数

![img](images\wps65.jpg) 

#### 3.2.9查找员工数超过2人的部门名称和员工数量

![img](images\wps66.jpg) 

 

#### 3.2.10将employee表中的员工号码由大到小排列

![img](images\wps67.jpg) 

## 4.视图实验

### 4.1视图的定义

> ​	MySQL 视图（View）是一种虚拟存在的表，同真实表一样，视图也由列和行构成，但视图并不实际存在于数据库中。行和列的数据来自于定义视图的查询中所使用的表，并且还是在使用视图时动态生成的。数据库中只存放了视图的定义，并没有存放视图中的数据，这些数据都存放在定义视图查询所引用的真实表中。

​	使用视图查询数据时，数据库会从真实表中取出对应的数据。因此，视图中的数据是依赖于真实表中的数据的。一旦真实表中的数据发生改变，显示在视图中的数据也会发生改变。视图可以从原有的表上选取对用户有用的信息，那些对用户没用，或者用户没有权限了解的信息，都可以直接屏蔽掉，作用类似于筛选。这样做既使应用简单化，也保证了系统的安全。

### 4.2创建视图

### 4.2.1基本语法

可以使用 CREATE VIEW 语句来创建视图。

语法格式如下：

```bash
CREATE VIEW <视图名> AS <SELECT语句>
```

语法说明如下。

①<视图名>`：指定视图的名称。该名称在数据库中必须是唯一的，不能与其他表或视图同名。

②<SELECT语句>`：指定创建视图的 SELECT 语句，可用于查询多个基础表或源视图。

#### 4.2.2对于创建视图中的 SELECT 语句的指定存在以下限制：

> ①用户除了拥有 CREATE VIEW 权限外，还具有操作中涉及的基础表和其他视图的相关权限。
>
> ②SELECT 语句不能引用系统或用户变量。
>
> ③SELECT 语句不能包含 FROM 子句中的子查询。
>
> ④SELECT 语句不能引用预处理语句参数。
> 视图定义中引用的表或视图必须存在。但是，创建完视图后，可以删除定义引用的表或视图。可使用 CHECK TABLE 语句检查视图定义是否存在这类问题。

​	视图定义中允许使用 ORDER BY 语句，但是若从特定视图进行选择，而该视图使用了自己的 ORDER BY 语句，则视图定义中的 ORDER BY 将被忽略。

​	视图定义中不能引用 TEMPORARY 表（临时表），不能创建 TEMPORARY 视图。WITH CHECK OPTION 的意思是，修改视图时，检查插入的数据是否符合 WHERE 设置的条件。

### 4.3查看视图的字段信息

查看视图的字段信息与查看数据表的字段信息一样，都是使用 DESCRIBE 关键字来查看的。具体语法如下：

```bash
DESCRIBE 视图名;
```

或简写成：

```bash
DESC 视图名;
```



### 4.4修改视图

#### 4.4.1基本语法

可以使用 ALTER VIEW 语句来对已有的视图进行修改。

语法格式如下：

```
ALTER VIEW <视图名> AS <SELECT语句>
```

语法说明如下：

> ①<视图名>`：指定视图的名称。该名称在数据库中必须是唯一的，不能与其他表或视图同名。
>
> ②<SELECT 语句>`：指定创建视图的 SELECT 语句，可用于查询多个基础表或源视图。


​	需要注意的是，对于 ALTER VIEW 语句的使用，需要用户具有针对视图的 CREATE VIEW 和 DROP 权限，以及由 SELECT 语句选择的每一列上的某些权限。

​	修改视图的定义，除了可以通过 ALTER VIEW 外，也可以使用 DROP VIEW 语句先删除视图，再使用 CREATE VIEW 语句来实现。

#### 4.4.2修改视图内容

·视图是一个虚拟表，实际的数据来自于基本表，所以通过插入、修改和删除操作更新视图中的数据，实质上是在更新视图所引用的基本表的数据。

> 注意：对视图的修改就是对基本表的修改，因此在修改时，要满足基本表的数据定义。

​	**某些视图是可更新的。也就是说，可以使用 UPDATE、DELETE 或 INSERT 等语句更新基本表的内容。对于可更新的视图，视图中的行和基本表的行之间必须具有一对一的关系。**

​	还有一些特定的其他结构，这些结构会使得视图不可更新。更具体地讲，如果视图包含以下结构中的任何一种，它就是不可更新的：

①聚合函数 SUM()、MIN()、MAX()、COUNT() 等。

②DISTINCT 关键字。

③GROUP BY 子句。

④HAVING 子句。

⑤UNION 或 UNION ALL 运算符。

⑥位于选择列表中的子查询。

⑦FROM 子句中的不可更新视图或包含多个表。

⑧WHERE 子句中的子查询，引用 FROM 子句中的表。

⑨ALGORITHM 选项为 TEMPTABLE（使用临时表总会使视图成为不可更新的）的时候。

### 4.5删除视图

#### 4.5.1基本语法

可以使用 DROP VIEW 语句来删除视图。

语法格式如下：

```bash
DROP VIEW <视图名1> [ , <视图名2> …]
```

其中：`<视图名>`指定要删除的视图名。DROP VIEW 语句可以一次删除多个视图，但是必须在每个视图上拥有 DROP 权限。

### 4.6查看视图中的数据

查看视图中的数据可以使用

```
select  ... from  视图名 ...
```

   语句来查询，和查询表中数据一样用法。比如：

```bash
select age from v_em where name = "李丽";
```

```bash
select v_em.employeeID,v_dp.name ,workYear   from v_dp JOIN v_em ON v_dp.employeeID = v_em.employeeID ;
```

### 4.7示例

#### 4.7.1创建DBEM数据库上的视图v_dp，包含department的全部信息

 

```
create view v_dp as select  from department with check option;
```

 

![img](images\wps71.jpg) 

 

#### 4.7.2创建DBEM数据库上的视图v_em，包含员工号码、姓名和实际收入

 

```
create view v_em(employeeID,name,realIncome) as select employee.employeeID,employee.name,(salary.income - salary.outcome ) as realIncome from  employee  join  salary on employee.employeeID = salary.employeeID;
```

![img](images\wps72.jpg)

#### 4.7.3从v_em视图中查询姓名为“李丽”的员工的实际收入

```
 select realIncome from v_em where name = "李丽";
```


![img](images\wps73.jpg)

#### 4.7.4.向v_dp视图中插入一行数据：6，广告部，推广产品。执行完之后分别查看视图v_dp和表department中发生的变化。

```
insert into v_dp(departmentID,departName,comment) values(6,"广告部","推广产品");
```

![img](images\wps74.jpg)

可以发现，当在视图v_dp中插入一条数据时，department中也插入了一条数据。

#### 4.7.5尝试向v_em视图中插入一行数据，看看会发生什么情况

```
insert into v_em("100020","赵柳",8000);
```

ERROR 1064 (42000): You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '"100020","赵柳",8000)' at line 1

![img](images\wps75.jpg) 

插入数据时发生了错误。

#### 4.7.6修改视图v_em中号码为102208的雇员的姓名为“赵柳“

```
update v_em set name = "赵柳" where employeeID = "102208";
```


![img](images\wps76.jpg)

#### 4.7.7删除视图v_dp中部门号为“2”的数据

```
delete from v_dp where departmentID = 2;
```

![img](images\wps77.jpg) 

![img](images\wps78.jpg)

## 5.索引实验

### 5.1索引的定义

> ​	索引是一种特殊的数据库结构，由数据表中的一列或多列组合而成，可以用来快速查询数据表中有某一特定值的记录。

​	通过索引，查询数据时不用读完记录的所有信息，而只是查询索引列。否则，数据库系统将读取每条记录的所有信息进行匹配。

​	可以把索引比作新华字典的音序表。例如，要查“库”字，如果不使用音序，就需要从字典的 400 页中逐页来找。但是，如果提取拼音出来，构成音序表，就只需要从 10 多页的音序表中直接查找。这样就可以大大节省时间。	因此，使用索引可以很大程度上提高数据库的查询速度，还有效的提高了数据库系统的性能。

### 5.2使用索引的优点

①通过创建唯一索引可以保证数据库表中每一行数据的唯一性。

②可以给所有的 MySQL 列类型设置索引。

③可以大大加快数据的查询速度，这是使用索引最主要的原因。

④在实现数据的参考完整性方面可以加速表与表之间的连接。

⑤在使用分组和排序子句进行数据查询时也可以显著减少查询中分组和排序的时间

### 5.3使用索引的缺点

①创建和维护索引组要耗费时间，并且随着数据量的增加所耗费的时间也会增加。

②索引需要占磁盘空间，除了数据表占数据空间以外，每一个索引还要占一定的物理空间。如果有大量的索引，索引文件可能比数据文件更快达到最大文件尺寸。

③当对表中的数据进行增加、删除和修改的时候，索引也要动态维护，这样就降低了数据的维护速度。

### 5.4 MySQL 中，通常有以下两种方式访问数据库表的行数据

#### 5.4.1 顺序访问

​	顺序访问是在表中实行全表扫描，从头到尾逐行遍历，直到在无序的行数据中找到符合条件的目标数据。顺序访问实现比较简单，但是当表中有大量数据的时候，效率非常低下。例如，在几千万条数据中查找少量的数据时，使用顺序访问方式将会遍历所有的数据，花费大量的时间，显然会影响数据库的处理性能。

#### 5.4.2索引访问

​	索引访问是通过遍历索引来直接访问表中记录行的方式。使用这种方式的前提是对表建立一个索引，在列上创建了索引之后，查找数据时可以直接根据该列上的索引找到对应记录行的位置，从而快捷地查找到数据。索引存储了指定列数据值的指针，根据指定的排序顺序对这些指针排序。

​	例如，在学生基本信息表 tb_students 中，如果基于 student_id 建立了索引，系统就建立了一张索引列到实际记录的映射表。当用户需要查找 student_id 为 12022 的数据的时候，系统先在 student_id 索引上找到该记录，然后通过映射表直接找到数据行，并且返回该行数据。因为扫描索引的速度一般远远大于扫描实际数据行的速度，所以采用索引的方式可以大大提高数据库的工作效率。

​	简而言之，不使用索引，MySQL 就必须从第一条记录开始读完整个表，直到找出相关的行。表越大，查询数据所花费的时间就越多。如果表中查询的列有一个索引，MySQL 就能快速到达一个位置去搜索数据文件，而不必查看所有数据，这样将会节省很大一部分时间。

### 5.5索引类型

#### 5.5.1B-TREE索引

①普通索引(INDEX)：最基本索引，没有唯一性限制
②唯一性索引(UNIQUE)：索引列的值不能重复
③主键(PRIMARY KEY)：创建表的时候指定，每个表只能有一个主键，主键的值不可重复，也不可为空（NULL）

#### 5.5.2HASH索引（当表类型为MEMORY或HEAP时可用）

##  创建 HASH索引

创建一个包含 (employeeID, name, education) 等字段的临时员工表(tmpEmployee)，并在该表的员工编号字段上创建一个HASH索引 

首先创建表：

```
create table if not exists tmpEmployee(employeeID varchar(6) primary key
not null,name varchar(10) not null,education varhcar(10));
```

然后建立一个hash索引：create index  索引名 using  hash  on 表名

```
create index in_hash using hash on tmpEmployee(employeeId);
```

 

![img](images\wps104.jpg)

#### 5.5.3R-TREE索引

​	MySQL支持对空间数据库进行R-TREE索引

### 5.6MySQL 提供了三种创建索引的方法：

#### 5.6.1 使用 CREATE INDEX 语句

​	可以使用专门用于创建索引的 CREATE INDEX 语句在一个已有的表上创建索引，**但该语句不能创建主键**。

语法格式：

```bash
CREATE <索引名> ON <表名> (<列名> [<长度>] [ ASC | DESC])
```

语法说明如下：

> ①<索引名>`：指定索引名。一个表可以创建多个索引，但每个索引在该表中的名称是唯一的。
>
> ②<表名>`：指定要创建索引的表名。
>
> ③<列名>`：指定要创建索引的列名。通常可以考虑将查询语句中在 JOIN 子句和 WHERE 子句里经常出现的列作为索引列。
>
> ④<长度>`：可选项。指定使用列前的 length 个字符来创建索引。使用列的一部分创建索引有利于减小索引文件的大小，节省索引列所占的空间。在某些情况下，只能对列的前缀进行索引。索引列的长度有一个最大上限 255 个字节（MyISAM 和 InnoDB 表的最大上限为 1000 个字节），如果索引列的长度超过了这个上限，就只能用列的前缀进行索引。另外，BLOB 或 TEXT 类型的列也必须使用前缀索引。
>
> ⑤ASC|DESC`：可选项。`ASC`指定索引按照升序来排列，`DESC`指定索引按照降序来排列，默认为`ASC`。



例如，在employee表的name列建立一个索引in_employee

```
CREATE INDEX in_employee ON employee(name=);
CREATE INDEX in_employee ON student( name asc);  #升序索引
```

**注：可以在创建表的同时创建索引**

可以用

```
 SHOW INDEX FROM  表名   
```

显示已经创建的索引信息

```
show index from employee;
```

![img](images\wps105.jpg)![img](images\wps106.jpg)\

![img](images\wps107.jpg)

#### 5.6.2使用 CREATE TABLE 语句

索引也可以在创建表（CREATE TABLE）的同时创建。在 CREATE TABLE 语句中添加以下语句。语法格式：

```bash
CONSTRAINT PRIMARY KEY [索引类型] (<列名>,…)
```

在 CREATE TABLE 语句中添加此语句，表示在创建新表的同时创建该表的主键。

语法格式：

```
KEY | INDEX [<索引名>] [<索引类型>] (<列名>,…)
```

在 CREATE TABLE 语句中添加此语句，表示在创建新表的同时创建该表的索引。

语法格式：

```
UNIQUE [ INDEX | KEY] [<索引名>] [<索引类型>] (<列名>,…)
```

在 CREATE TABLE 语句中添加此语句，表示在创建新表的同时创建该表的唯一性索引。

语法格式：

```
FOREIGN KEY <索引名> <列名>
```

在 CREATE TABLE 语句中添加此语句，表示在创建新表的同时创建该表的外键。

​	**在使用 CREATE TABLE 语句定义列选项的时候，可以通过直接在某个列定义后面添加 PRIMARY KEY 的方式创建主键。而当主键是由多个列组成的多列索引时，则不能使用这种方法，只能用在语句的最后加上一个 PRIMARY KRY(<列名>，…) 子句的方式来实现。**



##### 5.6.2.1唯一索引（unique）:

```
create unique index in_name on employee (name);
```

 

![img](images\wps108.jpg)



##### 5.6.2.2创建复合主键

```
create index in_employeeId_name on employee(employeID,name);
```

![img](images\wps109.jpg)

##### 5.6.2.3创建表的时候创建索引

创建与department表相同结构的表department1，并将departName设为主键，departmentID上建立一个索引 。

 

**首先建立一个表department1表：**

```
CREATE TABLE `department1` (

  `departmentID` char(3 )  not null,

  `departName` char(20)  NOT NULL,

  `comment` varchar(100)  DEFAULT NULL,

  PRIMARY KEY (`departName`)

 ) ENGINE=InnoDB DEFAULT CHARSET=gbk ;
```

 

然后给department1表添加一个索引：

```
create index in_departmentId on department1(departmentID);
```

![img](images\wps110.jpg)



#### 5.6.3 使用 ALTER TABLE 语句

CREATE INDEX 语句可以在一个已有的表上创建索引，ALTER TABLE 语句也可以在一个已有的表上创建索引。在使用 ALTER TABLE 语句修改表的同时，可以向已有的表添加索引。具体的做法是在 ALTER TABLE 语句中添加以下语法成分的某一项或几项。

语法格式：

```
ADD INDEX [<索引名>] [<索引类型>] (<列名>,…)
```

在 ALTER TABLE 语句中添加此语法成分，表示在修改表的同时为该表添加索引。

语法格式：

```
ADD PRIMARY KEY [<索引类型>] (<列名>,…)
```

在 ALTER TABLE 语句中添加此语法成分，表示在修改表的同时为该表添加主键。

语法格式：

```
ADD UNIQUE [ INDEX | KEY] [<索引名>] [<索引类型>] (<列名>,…)
```

在 ALTER TABLE 语句中添加此语法成分，表示在修改表的同时为该表添加唯一性索引。

语法格式：

```
ADD FOREIGN KEY [<索引名>] (<列名>,…)
```

在 ALTER TABLE 语句中添加此语法成分，表示在修改表的同时为该表添加外键。

### 5.7查看索引

```
SHOW INDEX FROM <表名> [ FROM <数据库名>]
```

> 语法说明如下：
>
> ①<表名>：指定需要查看索引的数据表名。
>
> ②<数据库名>：指定需要查看索引的数据表所在的数据库，可省略。比如，SHOW INDEX FROM student FROM test; 语句表示查看 test 数据库中 student 数据表的索引。

### 5.8删除索引

当不再需要索引时，可以使用 DROP INDEX 语句或 ALTER TABLE 语句来对索引进行删除。

#### 5.8.1 使用 DROP INDEX 语句

语法格式：

```
DROP INDEX <索引名> ON <表名>
```

语法说明如下：

> ①<索引名>`：要删除的索引名。
>
> ②<表名>`：指定该索引所在的表名。

#### 5.8.2使用 ALTER TABLE 语句

​	根据 ALTER TABLE 语句的语法可知，该语句也可以用于删除索引。具体使用方法是将 ALTER TABLE 语句的语法中部分指定为以下子句中的某一项。

> ①DROP PRIMARY KEY：表示删除表中的主键。一个表只有一个主键，主键也是一个索引。
>
> ②DROP INDEX index_name：表示删除名称为 index_name 的索引。
>
> ③DROP FOREIGN KEY fk_symbol：表示删除外键。

**注意：如果删除的列是索引的组成部分，那么在删除该列时，也会将该列从索引中删除；如果组成索引的所有列都被删除，那么整个索引将被删除。**



使用DROP INDEX语句删除索引：

```
drop index in_employee on employee;
```

![img](images\wps111.jpg) 

 

show create table employee;

注：也可以用 ALTER TABLE  来删除索引： 

```
alter table employee drop index in_phone ;
```



![img](images\wps112.jpg)

### 5.9研究与思考

#### 5.9.1使用CREATE  INDEX语句能创建主键吗？添加主键与添加普通索引有什么区别？

​	不能通过使用create index语句来创建主键。添加主键会自动创建主键索引，普通索引需要自己手动创建。

#### 5.9.2如果删除基本表的一个列或者多个列，该列上的索引会受怎样的影响？

​	索引是由数据库进行维护的，当我们对存在的索引进行增删查改的操作时，如果涉及到索引列，数据库都会对索引表进行更新。因此可以知道，当表中某个列被删除后，在该列上的索引也会被删除掉。

## 6.实体完整性实验

### 6.1定义

> **实体完整性：**是指保证表中所有的行唯一。实体完整性要求表中的所有行都有一个唯一标识符。这个唯一标识符可能是一列，也可能是几列的组合，称为主键。也就是说，表中的主键在所有行上必须取唯一值。强制实体完整性的方法有：索引、UNIQUE约束、PRIMARY KEY约束或IDENTITY属性。

​	如：student表中sno（学号）的取值必须唯一，它唯一标识了相应记录所代表的学生，学号重复是非法的。学生的姓名不能作为主键，因为完全可能存在两个学生同名同姓的情况。

### 6.2创建表时定义实体完整性：创建一个表 employee1，只含employeeID、name、gender和education列。以name为主键作为列name的完整性约束，employeeID为替代键作为表的完整性约束。

 

```bash
create table if not exists employee1(employeeID varchar(6) not null unique,name varchar(10) primary key,education varchar(10));
```

![img](images\wps113.jpg) 

 

### 6.3创建表后定义实体完整：定义不含主键的表employee2，包含上述属性列，然后定义实体完整性，以employeeID为主码.

```bash
create table if not exists employee2( employeeID varchar(6) not  null,name varchar(10) not null,education varchar(6));
```

```
 alter table employee2 add primary key(employeeID);
```

 

![img](images\wps114.jpg) 

### 6.4.设计记录增加到employee1表和employee2表，验证实体完整性是否起作用

```
insert into employee1 values("100","root","本科");
insert into employee1 values("100","root","本科");
```

![img](images\wps1157.jpg) 

```
insert into employee2 values("100","root","本科");
insert into employee2 values("100","root","本科");
```

![img](images\wps118.jpg) 

 

可见employee1和employee2的完整性都得到了验证。

## 7.参照性完整实验

## 7.1定义

> **参照完整性：**是指保证主关键字（被引用表）和外部关键字（引用表）之间的参照关系。它涉及两个或两个以上表数据的一致性维护。外键值将引用表中包含此外键的记录和被引用表中主键与外键相匹配的记录关联起来。在输入、更改或删除记录时，参照完整性保持表之间已定义的关系，确保键值在所有表中一致。这样的一致性要求确保不会引用不存在的值，如果键值更改了，那么在整个数据库中，对该键值的所有引用要进行一致的更改。参照完整性是基于外键与主键之间的关系。

​	例如学生学习课程的课程号必须是有效的课程号，score表（成绩表）的外键cno（课程号）将参考course表（课程表）中主键cno（课程号）以实现数据完整性。

### 7.2语法

#### 7.2.1创建表时定义参照完整性

##### 7.2.1.1列级参照完整性

列定义时加上关键字 REFERENCES ref_table_name(ref_key)

##### 7.2.1.2表级参照完整性

###### 7.2.1.2.1表定义时，在语句最后加上

```
CONSTRAINT ref_key_name FOREIGN KEY (ref_key) REFERENCES ref_table_name(ref_key)
```

### 7.2.2创建表后定义参照完整性

使用ALTER TABLE命令

#### 7.2.3参照性定义

> 参照性定义=：
> REFERENCES 表名 [(索引列名 ... )]
> [ON DELETE  {RESTRICT | CASCADE | SET NULL | NO ACTION}]
> [ON UPDATE  {RESTRICT | CASCADE | SET NULL | NO ACTION}]
> 索引列名=：
> 列名 [(长度)] [ASC | DESC]

#### 7.2.4参照动作

指定这个参照动作应用哪一条语句：这里有两条相关的语句，即UPDATE和DELETE语句；
指定采取哪个动作：可能采取的动作是RESTRICT（限制）、CASCADE（级联）、SET NULL、NO ACTION和SET DEFAULT。

> ①RESTRICT（限制）：当要删除或更新父表时，如果子表有匹配的项，拒绝对父表的删除或更新操作
> ②CASCADE（级联）：在父表上update/delete记录时，同步update/delete掉子表的匹配记录 .
> ③SET NULL：在父表上update/delete记录时，将子表上匹配记录的列设为null (要注意子表的外键列不能为not null)   。如果外键列没有指定NOT NULL限定词，这就是合法的。
> ④NO ACTION：NO ACTION意味着不采取动作，就是如果有一个相关的外键值在被参考的表里，删除或更新父表中主要键值的企图不被允许，和RESTRICT一样
> ⑤SET DEFAULT：作用和SET NULL一样，只不过SET DEFAULT是指定子表中的外键列为默认值

## 7.3创建语句：

```bash
create table if not exists salary1(employeeID char(6) not null,income double,outcome double,foreign key(employeeID) references salary(employeeID) on delete CASCADE on update CASCADE);
```

![img](images\wps80.jpg) 

 

### 7.4.从salary表中复制数据到salary1表：

 

```bash
insert into salary1(employeeID,income,outcome) select employeeID,income,outcome from salary;
```

![img](images\wps81.jpg) 

### 7.5删除salary表中的一行数据，再查看salary1表的内容，看看会发生什么情况

![img](images\wps82.jpg) 

从表中可以发现，从salary表中删除了employeeID =10008的记录，然后发现从表salary1中EmployeeID=10008的记录也被删除了。这是因为salary1表中参照完整性的外键为EmployeeID，其参照性键并且指定了参照性动作delete和update的级别为：CASCADE（级联）：在父表上update/delete记录时，同步update/delete掉子表的匹配记录。因此当表salary表中数据发生变化时，会同步到salary1表中。

 

 

 

 

 

 

## 8.存储过程实验

### 8.1存储过程定义

> 存储过程是一组为了完成特定功能的 SQL 语句集合。使用存储过程的目的是将常用或复杂的工作预先用 SQL 语句写好并用一个指定名称存储起来，这个过程经编译和优化后存储在数据库服务器中，因此称为存储过程。当以后需要数据库提供与已定义好的存储过程的功能相同的服务时，只需调用“CALL存储过程名字”即可自动完成。

### 8.2存储过程的优点

##### **1) 封装性**

通常完成一个逻辑功能需要多条 SQL 语句，而且各个语句之间很可能传递参数，所以，编写逻辑功能相对来说稍微复杂些，而存储过程可以把这些 SQL 语句包含到一个独立的单元中，使外界看不到复杂的 SQL 语句，只需要简单调用即可达到目的。并且数据库专业人员可以随时对存储过程进行修改，而不会影响到调用它的应用程序源代码。

##### 2) 可增强 SQL 语句的功能和灵活性

存储过程可以用流程控制语句编写，有很强的灵活性，可以完成复杂的判断和较复杂的运算。

##### 3) 可减少网络流量

由于存储过程是在服务器端运行的，且执行速度快，因此当客户计算机上调用该存储过程时，网络中传送的只是该调用语句，从而可降低网络负载。

##### 4) 高性能

当存储过程被成功编译后，就存储在数据库服务器里了，以后客户端可以直接调用，这样所有的 SQL 语句将从服务器执行，从而提高性能。但需要说明的是，存储过程不是越多越好，过多的使用存储过程反而影响系统性能。

##### 5) 提高数据库的安全性和数据的完整性

存储过程提高安全性的一个方案就是把它作为中间组件，存储过程里可以对某些表做相关操作，然后存储过程作为接口提供给外部程序。这样，外部程序无法直接操作数据库表，只能通过存储过程来操作对应的表，因此在一定程度上，安全性是可以得到提高的。

##### 6) 使数据独立

数据的独立可以达到解耦的效果，也就是说，程序可以调用存储过程，来替代执行多条的 SQL 语句。这种情况下，存储过程把数据同用户隔离开来，优点就是当数据表的结构改变时，调用表不用修改程序，只需要数据库管理者重新编写存储过程即可。

### 8.3创建存储过程

**语法格式如下：**

> CREATE PROCEDURE <过程名> ( [过程参数[,…] ] ) <过程体>
> [过程参数[,…] ] 格式
> [ IN | OUT | INOUT ] <参数名> <类型>

语法说明如下：

##### 1) 过程名

​	存储过程的名称，默认在当前数据库中创建。若需要在特定数据库中创建存储过程，则要在名称前面加上数据库的名称，即 db_name.sp_name。需要注意的是，名称应当尽量避免选取与 MySQL 内置函数相同的名称，否则会发生错误。

##### 2) 过程参数

​	存储过程的参数列表。其中，`< 参数名 >`为参数名，`< 类型 >`为参数的类型（可以是任何有效的 MySQL 数据类）。当有多个参数时，参数列表中彼此间用逗号分隔。存储过程可以没有参数（此时存储过程的名称后仍需加上一对括号），也可以有 1 个或多个参数。

​	MySQL 存储过程支持三种类型的参数，即输入参数、输出参数和输入/输出参数，分别用 IN、OUT 和 INOUT 三个关键字标识。其中，输入参数可以传递给一个存储过程，输出参数用于存储过程需要返回一个操作结果的情形，而输入/输出参数既可以充当输入参数也可以充当输出参数。

​	需要注意的是，参数的取名不要与数据表的列名相同，否则尽管不会返回出错信息，但是存储过程的 SQL 语句会将参数名看作列名，从而引发不可预知的结果。

##### 3) 过程体

​	存储过程的主体部分，也称为存储过程体，包含在过程调用的时候必须执行的 SQL 语句。这个部分以关键字 **BEGIN** 开始，以关键字 **END** 结束。若存储过程体中只有一条 SQL 语句，则可以省略 BEGIN-END 标志。

​	在存储过程的创建中，经常会用到一个十分重要的 MySQL 命令，即 DELIMITER 命令，特别是对于通过命令行的方式来操作 MySQL 数据库的使用者，更是要学会使用该命令。

​	**注意**：在 MySQL 中，服务器处理 SQL 语句默认是以分号作为语句结束标志的。然而，在创建存储过程时，存储过程体可能包含有多条 SQL 语句，这些 SQL 语句如果仍以分号作为语句结束符，那么 MySQL 服务器在处理时会以遇到的第一条 SQL 语句结尾处的分号作为整个程序的结束符，而不再去处理存储过程体中后面的 SQL 语句，这样显然不行。

为解决以上问题，通常使用 **DELIMITER** 命令将结束命令修改为其他字符。语法格式如下：

```bash
DELIMITER $$
```

语法说明如下：

- $$ 是用户定义的结束符，通常这个符号可以是一些特殊的符号，如两个“?”或两个“￥”等。
- 当使用 DELIMITER 命令时，应该避免使用反斜杠“\”字符，因为它是 MySQL 的转义字符。


在 MySQL 命令行客户端输入如下 SQL 语句。

```bash
mysql > DELIMITER ??
```

成功执行这条 SQL 语句后，任何命令、语句或程序的结束标志就换为两个问号“??”了。

若希望换回默认的分号“;”作为结束标志，则在 MySQL 命令行客户端输入下列语句即可：

```bash
mysql > DELIMITER ;
```

注意：DELIMITER 和分号“;”之间一定要有一个空格。在创建存储过程时，必须具有 CREATE ROUTINE 权限。

#### 8.3.1创建存储过程例子 

```bash
mysql> delimiter $$
mysql> create procedure show_employeeInfo()
    -> begin
    -> select *from employee;
    -> end$$
```

### 8.4查看存储过程结构：

```bash
show create procedure  show_employeeInfo \G;
```

![image-20201204152514002](images\50.png)

### 8.5删除存储过程：

```bash
DROP PROCEDURE [ IF EXISTS ] <过程名>
```

语法说明如下：

①过程名：指定要删除的存储过程的名称。

②IF EXISTS：指定这个关键字，用于防止因删除不存在的存储过程而引发的错误。

**注意：**存储过程名称后面没有参数列表，也没有括号，在删除之前，必须确认该存储过程没有任何依赖关系，否则会导致其他与之关联的存储过程无法运行。

![image-20201204152831123](images\51.png)

### 8.6修改存储过程的语法格式如下：

```bash
ALTER PROCEDURE 存储过程名 [ 特征 ... ]
```

`特征`指定了存储过程的特性，可能的取值有：

①CONTAINS SQL 表示子程序包含 SQL 语句，但不包含读或写数据的语句。

②NO SQL 表示子程序中不包含 SQL 语句。

③READS SQL DATA 表示子程序中包含读数据的语句。

④MODIFIES SQL DATA 表示子程序中包含写数据的语句。

⑤SQL SECURITY { DEFINER |INVOKER } 指明谁有权限来执行。

⑥DEFINER 表示只有定义者自己才能够执行。

⑦INVOKER 表示调用者可以执行。

⑧COMMENT 'string' 表示注释信息。

### 8.7存储过程调用

```bash
CALL 存储过程名([参数…]);
```

### 8.8存储函数

和存储过程一样，都是在数据库中定义一些 SQL 语句的集合。存储函数可以通过 return 语句返回函数值，主要用于计算并返回一个值。而存储过程没有直接返回值，主要用于执行操作。

##### 8.1.8.1语法在 MySQL 中，使用 **CREATE FUNCTION** 语句来创建存储函数，其语法形式如下：

```bash
CREATE FUNCTION sp_name ([func_parameter[...]])
RETURNS type
[characteristic ...] routine_body
```

其中：

①sp_name 参数：表示存储函数的名称；

②func_parameter：表示存储函数的参数列表；

③RETURNS type：指定返回值的类型；

④characteristic 参数：指定存储函数的特性，该参数的取值与存储过程是一样的；

⑤routine_body 参数：表示 SQL 代码的内容，可以用 BEGIN...END 来标示 SQL 代码的开始和结束。

**注意：**在具体创建函数时，函数名不能与已经存在的函数名重名。除了上述要求外，推荐函数名命名（标识符）为 function_xxx 或者 func_xxx。

func_parameter 可以由多个参数组成，其中每个参数由参数名称和参数类型组成，其形式如下：

[IN | OUT | INOUT] param_name type;

其中：

①IN 表示输入参数，OUT 表示输出参数，INOUT 表示既可以输入也可以输出；

②param_name 参数是存储函数的参数名称；

③type 参数指定存储函数的参数类型，该类型可以是 MySQL 数据库的任意数据类型。

#### 8.8.2例子

```bash
Delimiter $$;
 create function real_income(employeeID_1 varchar(6),employeeID_2 varchar(6)) returns tinyint(1) begin if (select income - outcome from salary where employeeID=employeeID_1) >  (select income - outcome from salary where employeeID=employeeID_2) then return 0; else return 1;  end if ;  end$$
```



###  8.9定义变量

MySQL 中可以使用 **DECLARE** 关键字来定义变量，其基本语法如下：

DECLARE var_name[,...] type [DEFAULT value]

其中：

- DECLARE 关键字是用来声明变量的；
- var_name 参数是变量的名称，这里可以同时定义多个变量；
- type 参数用来指定变量的类型；
- DEFAULT value 子句将变量默认值设置为 value，没有使用 DEFAULT 子句时，默认值为 NULL。

#### 8.9.1例 1

下面定义变量 my_sql，数据类型为 INT 类型，默认值为 10。SQL 语句如下：

```bash
DECLARE my_sql INT DEFAULT 10;
```



#### 8.9.2. 为变量赋值

MySQL 中可以使用 **SET** 关键字来为变量赋值，SET 语句的基本语法如下：

```bash
SET var_name = expr[,var_name = expr]...
```

其中：

- SET 关键字用来为变量赋值；
- var_name 参数是变量的名称；
- expr 参数是赋值表达式。


注意：一个 SET 语句可以同时为多个变量赋值，各个变量的赋值语句之间用逗号隔开。

#### 8.9.3例 2

下面为变量 my_sql 赋值为 30。SQL 语句如下：

```bash
SET my_sql=30;
```

MySQL 中还可以使用 **SELECT..INTO** 语句为变量赋值。其基本语法如下：

```bash
SELECT col_name [...] INTO var_name[,...]
FROM table_name WEHRE condition
```

其中：

- col_name 参数表示查询的字段名称；
- var_name 参数是变量的名称；
- table_name 参数指表的名称；
- condition 参数指查询条件。


注意：当将查询结果赋值给变量时，该查询语句的返回结果只能是单行。

### 8.10例子

#### 8.10.1.创建一个存储过程实例

```
mysql> delimiter $$

mysql> create procedure show_employeeInfo()

  -> begin

  -> select *from employee;

  -> end$$

Query OK, 0 rows affected (0.49 sec)
```

 

![img](images\wps88.jpg) 

然后查看过程存储创建结构：

![img](images\wps89.jpg) 

最后调用存储过程函数：

![img](images\wps90.jpg) 

 

#### 8.10.2.删除存储过程：使用DROP PROCEDURE语句删除存储过程，语法格式为：

```
DROP PROCEDURE [IF EXISTS] 存储过程名;
```

![img](images\wps92.jpg) 

 

#### 8.10.3创建一个存储过程，计算employee表中的员工人数，并存储到一个局部变量中，调用存储过程，并查看该变量结果（使用select @variable）

```
 create procedure count_employee(out count_emp int)  begin set count_emp = (select count(*) from employee) ;  end $$ 
```

![img](images\wps94.jpg) 

![img](images\wps95.jpg) 

调用并查询结果：

![img](images\wps96.jpg) 

#### 8.10.4.创建一个存储过程，比较两个员工的实际收入，若前者比后者高就输出0，否则输出1，员工用其员工编号识别。

```
Delimiter $$;

 create function real_income(employeeID_1 varchar(6),employeeID_2 varchar(6)) returns tinyint(1) begin if (select income - outcome from salary where employeeID=employeeID_1) >  (select income - outcome from salary where employeeID=employeeID_2) then return 0; else return 1;  end if ;  end$$
```

这时出现报错：

![img](images\wps97.jpg) 

 

这是我们开启了bin-log, 我们就必须指定我们的函数是否是
1 DETERMINISTIC 不确定的
2 NO SQL 没有SQl语句，当然也不会修改数据
3 READS SQL DATA 只是读取数据，当然也不会修改数据
4 MODIFIES SQL DATA 要修改数据
5 CONTAINS SQL 包含了SQL语句
其中在function里面，只有 DETERMINISTIC, NO SQL 和 READS SQL DATA 被支持。如果我们开启了 bin-log, 我们就必须为我们的function指定一个参数。
在MySQL中创建函数时出现这种错误的解决方法：
set global log_bin_trust_function_creators=TRUE;

![img](images\wps98.jpg) 

问题解决。

然后开始调用函数：

![img](images\wps99.jpg) 

成功实现需要的功能。

 

## 9触发器

### 9.1触发器是什么

> MySQL 的触发器和存储过程一样，都是嵌入到 MySQL 中的一段程序，是 MySQL 中管理数据的有力工具。不同的是执行存储过程要使用 CALL 语句来调用，而触发器的执行不需要使用 CALL 语句来调用，也不需要手工启动，而是通过对数据表的相关操作来触发、激活从而实现执行。比如当对 student 表进行操作（INSERT，DELETE 或 UPDATE）时就会激活它执行。

触发器与数据表关系密切，主要用于保护表中的数据。特别是当有多个表具有一定的相互联系的时候，触发器能够让不同的表保持数据的一致性。

在 MySQL 中，只有执行 INSERT、UPDATE 和 DELETE 操作时才能激活触发器，其它 SQL 语句则不会激活触发器。

那么为什么要使用触发器呢？比如，在实际开发项目时，我们经常会遇到以下情况：

- 在学生表中添加一条关于学生的记录时，学生的总数就必须同时改变。
- 增加一条学生记录时，需要检查年龄是否符合范围要求。
- 删除一条学生信息时，需要删除其成绩表上的对应记录。
- 删除一条数据时，需要在数据库存档表中保留一个备份副本。


虽然上述情况实现的业务逻辑不同，但是它们都需要在数据表发生更改时，自动进行一些处理。这时就可以使用触发器处理。例如，对于第一种情况，可以创建一个触发器对象，每当添加一条学生记录时，就执行一次计算学生总数的操作，这样就可以保证每次添加一条学生记录后，学生总数和学生记录数是一致的。

### 9.2创建基本语法

在 MySQL 5.7 中，可以使用 CREATE TRIGGER 语句创建触发器。

语法格式如下：

```bash
CREATE  trigger <触发器名> < BEFORE | AFTER >
<INSERT | UPDATE | DELETE >
ON <表名> FOR EACH Row<触发器主体>
```

语法说明如下。

##### 1) 触发器名

触发器的名称，触发器在当前数据库中必须具有唯一的名称。如果要在某个特定数据库中创建，名称前面应该加上数据库的名称。

##### 2) INSERT | UPDATE | DELETE

触发事件，用于指定激活触发器的语句的种类。

注意：三种触发器的执行时间如下。

- INSERT：将新行插入表时激活触发器。例如，INSERT 的 BEFORE 触发器不仅能被 MySQL 的 INSERT 语句激活，也能被 LOAD DATA 语句激活。
- DELETE： 从表中删除某一行数据时激活触发器，例如 DELETE 和 REPLACE 语句。
- UPDATE：更改表中某一行数据时激活触发器，例如 UPDATE 语句。

##### 3) BEFORE | AFTER

BEFORE 和 AFTER，触发器被触发的时刻，表示触发器是在激活它的语句之前或之后触发。若希望验证新数据是否满足条件，则使用 BEFORE 选项；若希望在激活触发器的语句执行之后完成几个或更多的改变，则通常使用 AFTER 选项。

##### 4) 表名

与触发器相关联的表名，此表必须是永久性表，不能将触发器与临时表或视图关联起来。在该表上触发事件发生时才会激活触发器。同一个表不能拥有两个具有相同触发时刻和事件的触发器。例如，对于一张数据表，不能同时有两个 BEFORE UPDATE 触发器，但可以有一个 BEFORE UPDATE 触发器和一个 BEFORE INSERT 触发器，或一个 BEFORE UPDATE 触发器和一个 AFTER UPDATE 触发器。

##### 5) 触发器主体

触发器动作主体，包含触发器激活时将要执行的 MySQL 语句。如果要执行多个语句，可使用 BEGIN…END 复合语句结构。

##### 6) FOR EACH ROW

一般是指行级触发，对于受触发事件影响的每一行都要激活触发器的动作。例如，使用 INSERT 语句向某个表中插入多行数据时，触发器会对每一行数据的插入都执行相应的触发器动作。

> 注意：每个表都支持 INSERT、UPDATE 和 DELETE 的 BEFORE 与 AFTER，因此每个表最多支持 6 个触发器。每个表的每个事件每次只允许有一个触发器。单一触发器不能与多个事件或多个表关联。

另外，在 MySQL 中，若需要查看数据库中已有的触发器，则可以使用 SHOW TRIGGERS 语句。

### 9.3删除触发器基本语法

与其他 MySQL 数据库对象一样，可以使用 DROP 语句将触发器从数据库中删除。

语法格式如下：

DROP TRIGGER [ IF EXISTS ] [数据库名] <触发器名>

语法说明如下：

##### 1) 触发器名

要删除的触发器名称。

##### 2) 数据库名

可选项。指定触发器所在的数据库的名称。若没有指定，则为当前默认的数据库。

##### 3) 权限

执行 DROP TRIGGER 语句需要 SUPER 权限。

##### 4) IF EXISTS

可选项。避免在没有触发器的情况下删除触发器。

> 注意：删除一个表的同时，也会自动删除该表上的触发器。另外，触发器不能更新或覆盖，为了修改一个触发器，必须先删除它，再重新创建。

### 9.4触发器中关联表中的列

​	在MySQL触发器中的SQL语句可以关联表中的任意列。但不能直接使用列的名称去标志，那会使系统混淆，因为激活触发器的语句可能已经修改、删除或添加了新的列名，而列的旧名同时存在。因此必须用这样的语法来标志：“NEW.column_name”或者“OLD.column_name”。
 ①NEW.column_name用来引用新行的一列，
 ②OLD.column_name用来引用更新或删除它之前的已有行的一列。
对于INSERT语句，只有NEW是合法的；
 ③ 对于DELETE语句，只有OLD才合法；
 ④而UPDATE语句可以与NEW或OLD同时使用。



### 9.5delete触发器例子

创建触发器，在employee表中删除员工信息的同时将salary表中该员工的信息删除，以确保数据完整性。创建完后尝试删除employee表中的一行数据，然后查看salary表中的变化情况

```bash
delimiter $$
create trigger delete_emp_salaryInfo after delete  on employee for each row begin  delete from salary where salary.employeeID=employee.employeeID; end $$
delimiter ;
```

### 9.6update触发器

当修改employee表时，若将employee表中的员工工作时间增加一年，则将收入增加500，增加两年则收入增加1000，以此类推.

```bash
delimiter $$
create trigger update_salary_income after update on employee for each row begin update salary set income = income + 500 * (NEW.workYear -OLD.workYear) where NEW.employeeID = OLD.employeeID; end $$
```

### 9.7例子

#### 9.7.1创建触发器，在employee表中删除员工信息的同时将salary表中该员工的信息删除，以确保数据完整性。创建完后尝试删除employee表中的一行数据，然后查看salary表中的变化情况

```
delimiter $$
 create trigger delete_emp_salaryInfo after delete  on employee for each row begin  delete from salary where salary.employeeID=employee.employeeID; end $$
delimiter ;
```

![img](images\wps83.jpg) 

![img](images\wps84.jpg) 

![img](images\wps85.jpg)可见当删除Employee中ID为“102201”的员工时，在salary表中的对应数据也会被删除。

 

#### 9.7.2当修改employee表时，若将employee表中的员工工作时间增加一年，则将收入增加500，增加两年则收入增加1000，以此类推

 

 delimiter $$

```
create trigger update_salary_income after update on employee for each row begin update salary set income = income + 500 * (NEW.workYear -OLD.workYear) where NEW.employeeID = OLD.employeeID; end $$
delimiter ;
```

![img](images\wps86.jpg) 

![img](images\wps87.jpg) 

可见，当Employee中EmployeeID = 102208的workYear + 2时，对应的salary增加了1000.说明触发器是起了作用了。

## 10.SQL语句备份与恢复实验

> ​	数据库的主要作用就是对数据进行保存和维护，所以备份数据是数据库管理中最常用的操作。为了防止数据库意外崩溃或硬件损伤而导致的数据丢失，数据库系统提供了备份和恢复策略。保证数据安全的最重要的一个措施就是定期的对数据库进行备份。这样即使发生了意外，也会把损失降到最低。
>
> ​	数据库备份是指通过导出数据或者复制表文件的方式来制作数据库的副本。当数据库出现故障或遭到破坏时，将备份的数据库加载到系统，从而使数据库从错误状态恢复到备份时的正确状态

### 10.1MySQL 导出数据

#### 10.1.1语句基本格式如下:

```
SELECT 列名 FROM table [WHERE 语句] INTO OUTFILE '目标文件' [OPTIONS]
```

该语句用 SELECT 来查询所需要的数据，用 INTO OUTFILE 来导出数据。其中，`目标文件`用来指定将查询的记录导出到哪个文件。这里需要注意的是，目标文件不能是一个已经存在的文件。

[OPTIONS] 为可选参数选项，OPTIONS 部分的语法包括 FIELDS 和 LINES 子句，其常用的取值有：

- FIELDS TERMINATED BY '字符串'：设置字符串为字段之间的分隔符，可以为单个或多个字符，默认情况下为制表符‘\t’。
- FIELDS [OPTIONALLY] ENCLOSED BY '字符'：设置字符来括上 CHAR、VARCHAR 和 TEXT 等字符型字段。如果使用了 OPTIONALLY 则只能用来括上 CHAR 和 VARCHAR 等字符型字段。
- FIELDS ESCAPED BY '字符'：设置如何写入或读取特殊字符，只能为单个字符，即设置转义字符，默认值为‘\’。
- LINES STARTING BY '字符串'：设置每行开头的字符，可以为单个或多个字符，默认情况下不使用任何字符。
- LINES TERMINATED BY '字符串'：设置每行结尾的字符，可以为单个或多个字符，默认值为‘\n’ 。

**注意：FIELDS 和 LINES 两个子句都是自选的，但是如果两个都被指定了，FIELDS 必须位于 LINES的前面。**

#### 10.1.2开放MySQL的导入导出文件权限

在进行对数据库数据备份时如果不开放MySQL的导入导出文件权限，那么导入导出时将遇到提示

```
Mysql 导入文件提示 --secure-file-priv option 问题
```

首先使用命令进行查看

```
 show variables like "%secure%"
```

![image-20201218143048655](E:\学习专用\数据库综合实验\资料整理\images\image-20201218143048655.png)

（1）NULL，表示禁止。

（2）如果value值有文件夹目录，则表示只允许该目录下文件（PS：测试子目录也不行）。

（3）如果为空，则表示不限制目录。

#### 10.1.3解决方案

（1）方案一：

把导入文件放入secure-file-priv目前的value值对应路径即可。

（2）方案二：

把secure-file-priv的value值修改为准备导入文件的放置路径。

（3）方案三：修改配置

去掉导入的目录限制。可修改mysql配置文件（Windows下为my.ini, Linux下的my.cnf），在[mysqld]下面，查看是否有：

secure_file_priv =

如上这样一行内容，如果没有，则手动添加。如果存在如下行：

secure_file_priv = /home 

这样一行内容，表示限制为/home文件夹。而如下行：

secure_file_priv =

这样一行内容，表示不限制目录，等号一定要有，否则mysql无法启动。

修改完配置文件后，重启mysql生效。

重启后：

关闭：service mysqld stop

启动：service mysqld start

**比如在Linux中修改配置文件（安装目录下 etc/mysql/my.cnf）使用 ：**

```
vim  etc/mysql/my.cnf 
```

![image-20201218144008566](E:\学习专用\数据库综合实验\资料整理\images\image-20201218144008566.png)



### 10.2MySQL之mysqldump的使用

#### 10.2.1mysqldump 简介

`mysqldump` 是 `MySQL` 自带的逻辑备份工具。

它的备份原理是通过协议连接到 `MySQL` 数据库，将需要备份的数据查询出来，将查询出的数据转换成对应的`insert` 语句，当我们需要还原这些数据时，只要执行这些 `insert` 语句，即可将对应的数据还原。

#### 10.2.2备份命令

#### 命令格式

```java
mysqldump [选项] 数据库名 [表名] > 脚本名
```

或

```java
mysqldump [选项] --数据库名 [选项 表名] > 脚本名
```

或

```java
mysqldump [选项] --all-databases [选项]  > 脚本名
```

```
mysqldump只导出整个数据库结构（不包含数据）
mysqldump -h localhost –u root -p  -d database > xxx.sql

mysqldump只导出单个数据表结构（不包含数据）
mysqldump -h localhost –u root -p  -d database table > xxx.sql
mysqldump只导出整个数据库的数据（不包含结构）
mysqldump -h localhost –u root -p  -t database > xxx.sql

mysqldump只导出单个数据表的数据（不包含结构）
mysqldump -h localhost –u root -p  -t database table > xxx.sql


```

 **选项说明**

| 参数名                          | 缩写 | 含义                          |
| ------------------------------- | ---- | ----------------------------- |
| --host                          | -h   | 服务器IP地址                  |
| --port                          | -P   | 服务器端口号                  |
| --user                          | -u   | MySQL 用户名                  |
| --pasword                       | -p   | MySQL 密码                    |
| --databases                     |      | 指定要备份的数据库            |
| --all-databases                 |      | 备份mysql服务器上的所有数据库 |
| --compact                       |      | 压缩模式，产生更少的输出      |
| --comments                      |      | 添加注释信息                  |
| --complete-insert               |      | 输出完成的插入语句            |
| --lock-tables                   |      | 备份前，锁定所有数据库表      |
| --no-create-db/--no-create-info |      | 禁止生成创建数据库语句        |
| --force                         |      | 当出现错误时仍然继续备份操作  |
| --default-character-set         |      | 指定默认字符集                |
| --add-locks                     |      | 备份数据库表时锁定数据库表    |

#### 10.2.3 实例

备份所有数据库：

```java
mysqldump -h localhost -P 3306 -uroot -p --all-databases > /backup/back/all.db
```

备份指定数据库：

```java
mysqldump -h localhost -P 3306 -uroot -p dbms > /mysql/data/dbms/back/dbms.db
```

备份指定数据库指定表(多个表以空格间隔,这里备份的是数据库dbms下的employee ，salary和 department这三个表)

```java
mysqldump -h localhost -P 3306 -uroot -p dbms employee salary department > /mysql/data/dbms/back/emp-salary-dep.db
```

备份指定数据库排除某些表（备份除了employee表外的dbms数据库中的表。）

```java
mysqldump -h localhost -P 3306  -uroot -p dbms --ignore-table=dbms.employee  > /mysql/data/dbms/back/salary-dep.db
```

![image-20201218164139527](E:\学习专用\数据库综合实验\资料整理\images\image-20201218164139527.png)

#### 10.2.4还原命令

##### 10.2.4.1 系统行命令

```java
mysqladmin -h localhost -P 3306 -uroot -p create db_name 
mysql  -uroot -p  db_name < /mysql/data/dbms/back/db_name.db

注：在导入备份数据库前，db_name如果没有，是需要创建的； 而且与db_name.db中数据库名是一样的才可以导入。
```

##### 10.2.4.2soure 方法

```java
mysql > use db_name
mysql > source /mysql/data/dbms/back/db_name.db
```



### 10.3导入数据

#### 10.3.1mysql 命令导入

使用 mysql 命令导入语法格式为：

```
mysql -h localhost -P 3306 -u 用户名    -p 密码    <  要导入的数据库数据
```

**实例：**

```
mysql -h localhost -P 3306 -u root -p dbms < /mysql/data/dbms/back/salary.sql;

```

以上命令将备份的整个数据库 salary.sql 导入。

------

#### 10.3.2 source 命令导入

source 命令导入数据库需要先登录到数库终端：

```
mysql> create database dbms;      # 创建数据库
mysql> use dbms;                  # 使用已创建的数据库 
mysql> set names utf8;           # 设置编码
mysql> source /home/mysql/data/dbms/back/salary.sql  # 导入备份数据库
```

------

#### 10.3.3使用 LOAD DATA 导入数据

基本语法：

```
　　load data  [low_priority] [local] infile 'file_name txt' [replace | ignore]
　　into table tbl_name
　　fields
　　[terminated by'\t']
　　[OPTIONALLY] enclosed by '']
　　[escaped by'\' ]]
　　[lines terminated by'\n']
　　[ignore number lines]
　　[(col_name)]
```

> 说明：
>
> load data infile语句从一个文本文件中以很高的速度读入一个表中。使用这个命令之前，mysqld进程（服务）必须已经在运行。由于安全原因，当读取位于服务器上的文件时，文件必须处于数据库目录或可被所有人读取。另外，为了对服务器上文件使用load data infile，在服务器主机上必须有file的权限。
>
> 1、如果你指定关键词low_priority，那么MySQL将会等到没有其他人读这个表的时候，才把数据插入。可以使用如下的命令： 
> 　　load data  low_priority infile "/home/mark/data sql" into table Orders;
>
> 2、如果指定local关键词，则表明从客户主机读文件。如果local没指定，文件必须位于服务器上。
>
> 3、replace和ignore关键词控制对现有的唯一键记录的重复的处理。如果你指定replace，新行将代替有相同的唯一键值的现有行。如果你指定ignore，跳过有唯一键的现有行的重复行的输入。如果你不指定任何一个选项，当找到重复键时，出现一个错误，并且文本文件的余下部分被忽略。例如：
> 　　load data  low_priority infile "/home/mark/data sql" replace into table Orders;
>
> 4、分隔符
> （1） fields关键字指定了文件字段的分割格式，如果用到这个关键字，MySQL剖析器希望看到至少有下面的一个选项： 
> 　　　　terminated by分隔符：意思是以什么字符作为分隔符
> 　　　　enclosed by字段括起字符
> 　　　　escaped by转义字符
> 　　　　terminated by描述字段的分隔符，默认情况下是tab字符（\t） 
> 　　　　enclosed by描述的是字段的括起字符。
> 　　　　escaped by描述的转义字符。默认的是反斜杠（backslash：\ ）  
> 　　　例如：load data infile "/home/mark/Orders txt" replace into table Orders fields terminated by',' enclosed by '"';
>
> （2）lines 关键字指定了每条记录的分隔符默认为'\n'即为换行符
> 　　如果两个字段都指定了，那fields必须在lines之前。如果不指定fields关键字，缺省值与这样写相同： fields terminated by'\t' enclosed by ’ '' ‘ escaped by'\\'
> 　　如果你不指定一个lines子句，缺省值与这样写的相同： lines terminated by'\n'
> 　　例如：load data infile "/jiaoben/load.txt" replace into table test fields terminated by ',' lines terminated by '\n';
>
> 5、 load data infile 可以按指定的列把文件导入到数据库中。 当我们要把数据的一部分内容导入的时候，，需要加入一些栏目（列/字段/field）到MySQL数据库中，以适应一些额外的需要。比如，我们要从Oracle数据库升级到MySQL数据库的时候，
> 下面的例子显示了如何向指定的栏目(field)中导入数据： 
> 　　load data infile "/home/Order txt" into table Orders(Order_Number, Order_Date, Customer_ID);
>
> 6.IGNORE number LINES: 忽略文件前几行。
>
> 7.当在服务器主机上寻找文件时，服务器使用下列规则： 
> （1）如果给出一个绝对路径名，服务器使用该路径名。 
> （2）如果给出一个有一个或多个前置部件的相对路径名，服务器相对服务器的数据目录搜索文件。  
> （3）如果给出一个没有前置部件的一个文件名，服务器在当前数据库的数据库目录寻找文件。 
> 例如： /myfile txt”给出的文件是从服务器的数据目录读取，而作为“myfile txt”给出的一个文件是从当前数据库的数据库目录下读取。
>
>
> 注意：字段中的空值用\N表示



#### 10.3.4使用 mysqlimport 导入数据

mysqlimport 客户端提供了 LOAD DATA INFILEQL 语句的一个命令行接口。mysqlimport 的大多数选项直接对应 LOAD DATA INFILE 子句。

从文件 salary.txt 中将数据导入到 dbms数据库中的salary表中, 可以使用以下命令：

```
$ mysqlimport -u root -p --local dbms salary.txt
password *****
```

mysqlimport 命令可以指定选项来设置指定格式,命令语句格式如下：

```
$ mysqlimport -u root -p --local --fields-terminated-by=":" 
   --lines-terminated-by="\r\n"  dbms salary.txt
password *****
```

mysqlimport 语句中使用 --columns 选项来设置列的顺序：

```
$ mysqlimport -u root -p --local --columns=b,c,a 
    dbms salary.txt
password *****
```

------

​    **使用mysqlimport -?命令，可以查看mysqlimport的具体参数及详细说明。下表是一些常见的选项：**

| -c, --columns=name                   | Use only these columns to import the data to. Give the column names in a comma separated list. This is same as giving columns to LOAD DATA INFILE. | 该选项采用用逗号分隔的列名作为其值。列名的顺序指示如何匹配数据文件列和表列。 |
| ------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| -C, --compress                       | Use compression in server/client protocol.                   | 压缩在客户端和服务器之间发送的所有信息（如果二者均支持压缩） |
| -d, --delete                         | First delete all rows from table.                            | 新数据导入数据表中之前删除数据数据表中的所有信息             |
| --fields-terminated-by=name          | Fields in the textfile are terminated by …                   | 指定数据之间的分隔符。默认的分隔符是跳格符（Tab）            |
| --fields-enclosed-by=name            | Fields in the importfile are enclosed by ...                 | 指定文本文件中数据的记录是以什么括起的， 很多情况下数据以双引号括起。 默认的情况下数据是没有被字符括起的 |
| --fields-optionally-enclosed-by=name | Fields in the i.file are opt. enclosed by …                  | 字段包括符，只用在CHAR和VERCHAR字段上                        |
| --fields-escaped-by=name             | Fields in the i.file are escaped by ...                      | 转义字符                                                     |
| -f, --force                          | Continue even if we get an sql-error.                        | 不管是否遇到错误，MySQLimport将强制继续插入数据              |
| -?, --help                           | Displays this help and exits.                                | 显示帮助消息并退出                                           |
| -h, --host=name                      | Connect to host.                                             | 将数据导入给定主机上的MySQL服务器。默认主机是localhost       |
| -i, --ignore                         | If duplicate unique key was found, keep old row.             | 跳过或者忽略那些有相同唯一关键字的行， 导入文件中的数据将被忽略 |
| --ignore-lines=#                     | Ignore first n lines of data infile.                         | 忽视数据文件的前n行                                          |
| --lines-terminated-by=name           | Lines in the i.file are terminated by ...                    | 行记录分隔符。 默认的情况下MySQLimport以newline为行分隔符    |
| -L, --local                          | Read all files through the client.                           | 从本地客户端读入输入文件                                     |
| -l, --lock-tables                    | Lock all tables for write (this disables threads).           | 数据被插入之前锁住表，防止在更新数据库时，用户的查询和更新受到影响 |
| --low-priority                       | Use LOW_PRIORITY when updating the table.                    | 低优先级                                                     |
| -p, --password[=name]                | Password to use when connecting to server. If password is not given it's asked from the tty. | 提示输入密码                                                 |
| -W, --pipe                           | Use named pipes to connect to server.                        | 使用命名管道连接服务器                                       |
| -P, --port=#                         | Port number to use for connection or 0 for default to, in order of preference, my.cnf, $MYSQL_TCP_PORT, /etc/services, built-in default (3306). | 用于连接的TCP/IP端口号                                       |
| --protocol=name                      | The protocol of connection (tcp,socket,pipe,memory).         | 连接使用的协议                                               |
| -r, --replace                        | If duplicate unique key was found, replace old row.          | 与－i选项的作用相反；此选项将替代表中有相同唯一关键字的记录  |
| --shared-memory-base-name=name       | Base name of shared memory.                                  | 共享内存连接名。该选项只用于Windows                          |
| -s, --silent                         | Be more silent.                                              | 沉默模式。只有出现错误时才输出                               |
| -S, --socket=name                    | Socket file to use for connection.                           | 当连接localhost时使用的套接字文件(为默认主机)                |
| --use-threads=#                      | Load files in parallel. The argument is the number of threads to use for loading data. | 并行多线程导入个数                                           |
| -u, --user=name                      | User for login if not current user.                          | 连接服务器时MySQL使用的用户名                                |
| -v, --verbose                        | Print info about the various stages.                         | 冗长模式。打印出程序操作的详细信息                           |
| -V, --version                        | Output version information and exit.                         | 显示版本（version）                                          |

。



#### 10.3.5测试用例（这里使用Ubuntu18.04服务器中使用Docker安装的MySQL8进行测试）

##### 10.3.5.1将用不同的存放格式（自由设计）备份DBEM数据库中的employee, salary两个基本表，其中employee表要求只备份employeeID, name, education等三个字段。

```sql
select employeeID,name,education from employee into outfile "employee.csv" character set utf8 FIELDS TERMINATED BY '\t' ;

```

![image-20201218142646599](E:\学习专用\数据库综合实验\资料整理\images\image-20201218142646599.png)



![image-20201218142727440](E:\学习专用\数据库综合实验\资料整理\images\image-20201218142727440.png)

```sql
select* from salary into outfile "salary.txt" character set utf8 FIELDS TERMINATED BY '\t' ;
select* from department into outfile "department.docx" character set utf8 FIELDS TERMINATED BY '\t' ;

```

![image-20201218144512457](E:\学习专用\数据库综合实验\资料整理\images\image-20201218144512457.png)

然后在备份中目录（secure_file_priv 的value中目录。笔者导出时直接outfile   “salary.txt”这里会默认将数据导出到mysql/data下中 dbms数据库对应文件夹名dbms的目录下）：

![image-20201218144756169](E:\学习专用\数据库综合实验\资料整理\images\image-20201218144756169.png)

![image-20201218145239171](E:\学习专用\数据库综合实验\资料整理\images\image-20201218145239171.png)



##### 10.3.5.2根据上述任务所保存的文件，将相关数据恢复到基本表中，其中要求employee表在恢复之前事先随机删除几条记录，SQL语句中要求指定replace功能

![image-20201218150712281](E:\学习专用\数据库综合实验\资料整理\images\image-20201218150712281.png)

## ![img](file:///C:\Users\ADMINI~1\AppData\Local\Temp\ksohtml14552\wps5.jpg)![img](E:\学习专用\数据库综合实验\资料整理\images\wps6.jpg) 

可以发现，使用replace命令会首先删除表中和所插入数据中键相同的数据然后再导入数据。并且除了employeeId，name,education这三项外都是NULL。



## 11. 客户端工具备份与恢复实验

### 11.1使用mysqldump命令备份DBEM数据库中的salary表

将数据库dbms中的salary表备份到当前所在目录中（你所在文件夹下使用指令的地方）backup文件夹下的salary.sql文件中。

```
mysqldump -h localhost -P 3307 -u root -p dbms salary > backup/salary.sql
```

![image-20201218194833086](E:\学习专用\数据库综合实验\资料整理\images\image-20201218194833086.png)

![image-20201218194803329](E:\学习专用\数据库综合实验\资料整理\images\image-20201218194803329.png)

### 11.2.使用mysqldump命令备份整个DBEM数据库

```
mysqldump -h localhost -P 3307 -u root -p dbms --default-character-set=utf8 > backup/dbms.sql
```

将数据库dbms备份到当前所在目录中（你所在文件夹下使用指令的地方）backup文件夹下的dbms.sql文件中。

![image-20201218195007875](E:\学习专用\数据库综合实验\资料整理\images\image-20201218195007875.png)

![image-20201218194929936](E:\学习专用\数据库综合实验\资料整理\images\image-20201218194929936.png)

### 11.3.删除employee表，然后使用mysql命令，利用上述保存的文件恢复employee表

①首先删除employee表

![image-20201218201559341](E:\学习专用\数据库综合实验\资料整理\images\image-20201218201559341.png)

②从数据库dbms的的全备份dbms.sql中查询employee表的结构

```
sed -e'/./{H;$!d;}' -e 'x;/CREATE TABLE `employee`/!d;q' dbms.sql
```

![image-20201218201748317](E:\学习专用\数据库综合实验\资料整理\images\image-20201218201748317.png)

②使用查询的Employee表结构在MySQL中创建employee表

```
DROP TABLE IF EXISTS `employee`;
/*!40101 SET @saved_cs_client     = @@character_set_client */;
/*!50503 SET character_set_client = utf8mb4 */;
CREATE TABLE `employee` (
  `employeeID` char(6) NOT NULL,
  `name` char(10) NOT NULL,
  `education` char(4) NOT NULL,
  `birth` date NOT NULL,
  `gender` tinyint(1) NOT NULL DEFAULT '1',
  `workYear` tinyint(1) DEFAULT NULL,
  `address` varchar(100) DEFAULT NULL,
  `phone` char(12) DEFAULT NULL,
  `departmentID` char(3：) DEFAULT NULL,
  PRIMARY KEY (`employeeID`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_0900_ai_ci;
/*!40101 SET character_set_client = @saved_cs_client */;

 
```

![image-20201218203847259](E:\学习专用\数据库综合实验\资料整理\images\image-20201218203847259.png)

③从dbms.sql备份中查询出employee表的数据，并保存到employee.sql文件中

```
grep 'INSERT INTO `employee`' dbms.sql >employee.sql
```

![image-20201218201955024](E:\学习专用\数据库综合实验\资料整理\images\image-20201218201955024.png)

④从employee.sql中恢复数据到MySQL的dbms数据库中的employee表。

```
mysql -h 121.4.41.89 -P 3307 -u root -p dbms --default-character-set=utf8 < employee.sql
```

![image-20201218221330238](E:\学习专用\数据库综合实验\资料整理\images\image-20201218221330238.png)

这样数据就完整地从全备份数据库dbms.sql中提取出来最终恢复到MySQL数据库了。

**ps：在进行导入导出文件是需要设置默认编码，为了防止乱码**

### 11.4.删除salary表的部分数据，然后使用mysqlimport命令及--replace功能恢复salary表，其中salary表的数据备份文件使用实验4.1中的文件



①首先删除两条数据

![image-20201218222233191](E:\学习专用\数据库综合实验\资料整理\images\image-20201218222233191.png)

②从备份的salary.csv中恢复数据：

```sql
mysqlimport -h 121.4.41.89 -P 3307 -u root -p --default-character-set=utf8  --replace  dbms salary.csv;
```

![image-20201218230050713](E:\学习专用\数据库综合实验\资料整理\images\image-20201218230050713.png)

![image-20201218230022613](E:\学习专用\数据库综合实验\资料整理\images\image-20201218230022613.png)

这样数据就完整地从全备份数据库dbms.sql中提取出来最终恢复到MySQL数据库了。

**ps：**在进行导入导出文件是需要设置默认编码，为了防止乱码

## 12.自主存取控制实验

### 12.1MySQL的权限

> 用户连接到MySQL，可以做各种查询，这都是MySQL用户与权限功能在背后维持着操作。

用户与数据库服务器交互数据，分为两个阶段：

（1）你有没有权连接上来
（2）你有没有权执行本操作

#### 12.1.1你有没有权连接上来

服务器如何判断用户有没有权连接上来？

依据：

1）你从哪里来？host
2）你是谁？user
3）你的密码是多少？password

用户的这三个信息，存储在mysql库中的user表中。

修改host域，使IP可以连接

```
mysql>``update` `user` `set` `host=``'192.168.137.123'` `where` `user` `= ``'root'``;``mysql>flush ``privileges``; ``--冲刷权限
```

修改用户密码

```
mysql>``update` `user` `set` `password``=``password``(``'11111111'``) ``where` `xxx;``mysql>flush ``privileges``; ``--冲刷权限
```

#### 12.1.2你有没有权执行本操作

> 在mysql中，有一个库是mysql库，在这个库中有三个表，一个是user表，user表中存储了所有用户的权限信息。一个是db表，db表存储的是所有用户在数据库层的权限信息。一个是tables_priv表，tables_priv表存储的是所有用户在表层的权限信息。

用户登录，user表首先能限制用户登录，其次还保存了该用户的全局权限，如果该用户没有任何权限，那么将从db表中查找该用户是否有某个数据库的操作权限，如果都没有，将从table_priv表中查找该用户是否有某个表的操作权限，如果有，则该用户可以按照已有的权限来操作该表。

### 12.2创建MySQL的用户

**命令:**

```
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
```

说明：

> username：你将创建的用户名.
>
> host：指定该用户在哪个主机上可以登陆，如果是本地用户可用localhost，如果想让该用户可以从任意远程主机登陆，可以使用通配符%代替host。
>
> password：该用户的登陆密码，密码可以为空，如果为空则该用户可以不需要密码登陆服务器。



**关于创建用户时提示“Operation CREATE USER failed for   XXX”的解决办法**：

![image-20201219185206053](E:\学习专用\数据库综合实验\资料整理\images\image-20201219185206053.png)

出现该原因的结果有可能是你使用了 

```
delete  from  mysql.user  where user ='user_1';
```

的语句删除用户，即使你刷新了 flush  privileges；也没用。

删除用户需要使用  

```
drop user   用户
```

![image-20201219185517676](E:\学习专用\数据库综合实验\资料整理\images\image-20201219185517676.png)

然后重新进行创建就好了：

```
create user 'user_1'@'%' identified by '123456';
```

![image-20201219185556427](E:\学习专用\数据库综合实验\资料整理\images\image-20201219185556427.png)

### 12.3用户授权

![image-20201219111003565](E:\学习专用\数据库综合实验\资料整理\images\image-20201219111003565.png)

#### 12.3.1授权命令格式

```sql
GRANT privileges ON databasename.tablename TO 'username'@'host';
```

> privileges：用户的操作权限，如SELECT，INSERT，UPDATE等，如果要授予所的权限则使用ALL;
>
> databasename：数据库名;
>
> tablename：表名，如果要授予该用户对所有数据库和表的相应操作权限则可用*表示，如*.*;

例子:

```sql
GRANT SELECT, INSERT ON test.user TO 'user_1'@'%';
GRANT ALL ON *.* TO 'user_1'@'%';
GRANT ALL ON maindataplus.* TO 'user_1'@'%';
```

**注意:**

用以上命令授权的用户不能给其它用户授权，如果想让该用户可以授权，用以下命令:

```sql
GRANT privileges ON databasename.tablename TO 'username'@'host' WITH GRANT OPTION;
```

#### 12.3.2使用REVOKE命令可以回收授予的权限，语法格式如下：

```
REVOKE priv_type ON {表名|数据库名} FROM 用户;
```

**注：只有拥有当前数据库全局CREATE或者UPDATE权限的用户才能使用REVOKE命令。**

#### 12.3.3全局授权和收回

全局授权格式（使用通配符*）：

```sql
grant` `[权限1,权限2,权限3] ``on` `*.* ``to` `user``@``'host'` `identified ``by` `'password'
```

常用权限：all、create、drop、select、insert、delete、update

授权：

创建lisi用户，host为192.168.191.%，%通配符表示192.168.191.xxx结尾的主机都可以连接，密码为12345678。

```sql
grant` `all` `on` `*.* ``to` `lisi@``'192.168.191.%'` `identified ``by` `'12345678'``;
```

收回权限：

```sql
revoke` `all` `on` `*.* ``from` `lisi@``'192.168.191.%'``;
```

#### 12.3.4数据库级授权和收回

> 需求：让lisi用户拥有mysqlmaster数据库的所有操作权限

授权：

```
grant` `all` `on` `mysqlmaster.* ``to` `lisi@``'192.168.191.%'` `identified ``by` `'12345678'``;
```

收回：

```
revoke` `all` `on` `mysqlmaster.* ``from` `lisi@``'192.168.191.%'``;
```

#### 12.3.5表级授权和收回

> 需求：让lisi用户具有mysqlmaster数据库下的goods表的insert、update、select三个操作的权限。

授权：

```
grant` `insert``,``update``,``select` `on` `mysqlmaster.goods ``to` `lisi@``'192.168.191.%'` `identified ``by` `'12345678'``;
```

收回：

```
revoke` `insert``,``update``,``select` `on` `mysqlmaster.goods ``from` `lisi@``'192.168.191.%'``;
```

### 12.4更新用户语法格式如下：

#### 12.4.1如果是修改当前登录用户密码语法：

```
SET PASSWORD FOR 用户=PASSWORD(‘新密码’);
```

#### 12.4.2通过修改mysql数据库下的user表中的来修改（不可取）

命令和update普通表一样，语法格式如下：

```
UPDATE MYSQL.USER SET 属性名= 新属性值  WHERE  条件;
```

比如，修改User的用户名为“user_1”将其用户名改为“user_3”,密码改为“1234”。

> **注意：MySQL8之前修改密码是 set password= password("新密码");而MySQL8是 set authentication_string = "新密码".也就是MySQL8的密码字段为authentication_string 。**

```
update mysql.user set user ="user_3" ,authentication_string ="1234" where user ="user_1";
```

![image-20201219192328977](E:\学习专用\数据库综合实验\资料整理\images\image-20201219192328977.png)

可以发现如果直接使用 set authentication_string ='新密码',那么该密码直接被填充到用户中而没有经过加密。而我们知道，MySQL的密码是经过加密的，因此这样子使用update修改密码是有问题的，在你登录时候虽然用户名和密码都正确，但是却没法进行登录，会提示“ Access denied for user 'user_3'@'117.154.88.249' (using password: YES)”。

​	MySQL8.0后请使用alter修改用户密码，因为在MySQL8.0以后的加密方式为caching_sha2_password，如果使用update修改密码会给user表中root用户的authentication_string字段下设置newpassowrd值，当再使用alter user 'root'@'localhost' identified by 'newpassword'修改密码时会一直报错，**必须清空后再修改**（如果不首先清空这个authentication_string，在使用alter user  进行修改时会发生“Operation ALTER USER failed for  XXX”的提示）

![image-20201219194453549](E:\学习专用\数据库综合实验\资料整理\images\image-20201219194453549.png)

**因为authentication_string字段下只能是MySQL加密后的43位字符串密码，其他的会报格式错误，所以在MySQL8.0以后能修改密码的方法只能是使用alter来修改：**

```
ALTER USER   用户   IDENTIFIED  WITH mysql_native_password BY '新密码';
#或者是
ALTER USER 用户 IDENTIFIED BY ‘新密码’;
```

![image-20201219194554533](E:\学习专用\数据库综合实验\资料整理\images\image-20201219194554533.png)

比如：将用户 ‘user_1’@‘%’ 的密码改为“1234”

```sql
ALTER USER ‘user_1’@‘%’ IDENTIFIED BY ‘1234’;
#或者
ALTER USER   ‘user_1’@‘%’    IDENTIFIED  WITH mysql_native_password BY '1234';
```

最后刷新一定要刷新下

```sql
flush privileges;
```

### 12.5查询某个用户权限

```
show grants for  用户;
```

比如：

```
show grants for 'user_1'@'%';
```

![image-20201219140946410](E:\学习专用\数据库综合实验\资料整理\images\image-20201219140946410.png)

### 12.6删除用户

#### 12.6.1使用drop命令

```
drop user 用户;
```

比如：

```
drop user  'user_1'@'%';
```

![image-20201219185112596](E:\学习专用\数据库综合实验\资料整理\images\image-20201219185112596.png)

删除的是 用户 'user_1'@'%'。

#### 12.6.2使用delete命令

```sql
delete from  mysql.user  where  条件
```

 比如：

```
delete from  mysql.user where user='user_1' and host ='%';
```

删除的是 用户 'user_1'@'%'。

#### 12.6.3使用drop和delete命令的区别

##### 12.6.3.1drop user 用户名;

删除已经存在的用户，例如要删除‘user_1’这个用户，(drop user 'user_1';)默认删除的是user_1@”%”这个用户，如果还有其他用户，例如user_1@”localhost”,user_1@”ip”,则不会一起被删除。如果只存在一个用户'user_1'@”localhost”,使用语句（drop user 'user_1';）会报错，应该用（drop user 'user_1'@”localhost”;）如果不能确定（用户名@机器名）中的机器名，可以在mysql中的user表中进行查找，user列对应的是用户名，host列对应的是机器名。

##### 12.6.3.5 delete from user where user=”用户名” and host=”localhost”;

delete也是删除用户的方法，例如要删除'user_1'@”localhost”用户，则可以

```sql
delete from user where user=”user_1” and host=”localhost”;
```

**drop删除掉的用户不仅将user表中的数据删除，还会删除诸如db和其他权限表的内容。而（delete from mysql.user）只是删除了mysql数据库中user表的内容，其他表不会被删除，后期如果命名一个和已删除用户相同的名字，权限就会被继承。**

### 12.7测试用例

##### 12.7.1.创建用户user_1和user_2，密码都为123456

```sql
 create user 'user_1'@'%' identified by "123456";
create user 'user_2'@'%' identified by "123456";
```

![img](E:\学习专用\数据库综合实验\资料整理\images\wps7.jpg) 

![img](E:\学习专用\数据库综合实验\资料整理\images\wps8.jpg) 

##### 12.7.2.将用户user_2的名称修改为user_3，并将其密码修改为1234

![image-20201219223648097](E:\学习专用\数据库综合实验\资料整理\images\image-20201219223648097.png)

首先使用update mysql.user语句来更新user_2中的用户名

```sql
update  mysql.user set user ='user_3' where  user ='user_2' and host='%';
```


![img](E:\学习专用\数据库综合实验\资料整理\images\wps9.jpg)

然后使用alter命令修改密码：

```mysql
alter user 'user_3'@'%' identified with mysql_native_password by '1234';
```

![image-20201219225202606](E:\学习专用\数据库综合实验\资料整理\images\image-20201219225202606.png)

##### 12.7.3.以user_1身份登陆数据库

![image-20201219225740145](E:\学习专用\数据库综合实验\资料整理\images\image-20201219225740145.png)

从可以发现可以远程登录了。

##### 12.7.4.授予用户user_1对DBEM数据库中employee表的查询、插入、修改、删除等权限。

![img](E:\学习专用\数据库综合实验\资料整理\images\wps11.jpg)
这时候user_1就有了对Employee表的增删查改功能了。
![img](E:\学习专用\数据库综合实验\资料整理\images\wps12.jpg)
可以发现，user1只能访问dbms中的employee表的数据，并能进行增删查改功能。

 

##### 12.7.5.授予用户user_1对salary表的查询权限，并允许其将权限授予其他用户，然后用user_1登陆数据库并将salary表的查询权限授予user_3。

首先给与user_1对salary表的查询权限：

![img](E:\学习专用\数据库综合实验\资料整理\images\wps13.jpg) 

然后将对salary表的查询权限给user_3:

![img](E:\学习专用\数据库综合实验\资料整理\images\wps14.jpg) 

![img](E:\学习专用\数据库综合实验\资料整理\images\wps15.jpg) 

![img](E:\学习专用\数据库综合实验\资料整理\images\wps16.jpg)



##### 12.7.7.回收user_1的employee表上的select权


![img](E:\学习专用\数据库综合实验\资料整理\images\wps17.jpg)

 可以发现，user_1上的表的select权限已经被收回了。

## 13.并发控制实验

### 13.1并发的定义

> 并发即指在同一时刻，多个操作并行执行。MySQL对并发的处理主要应用了两种机制——是"锁"和"多版本控制"。

### 13.2并发控制

​	MySQL提供两个级别的并发控制：服务器级(the server level)和存储引擎级(the storage engine level)。加锁是实现并发控制的基本方法，MySQL中锁的粒度：

#### 13.2.1表级锁

MySQL独立于存储引擎提供表锁，例如，对于ALTER TABLE语句，服务器提供表锁(table-level lock)。

#### 13.2.2行级锁

InnoDB和Falcon存储引擎提供行级锁，此外，BDB支持页级锁。

#### 13.2.3其它

​	另外，值得一提的是，MySQL的一些存储引擎（如InnoDB、BDB）除了使用封锁机制外，还同时结合MVCC机制，即多版本并发控制(Multi-Version Concurrent Control)，来实现事务的并发控制，从而使得只读事务不用等待锁，提高了事务的并发性。

### 13.3锁的分类

#### 13.3.1共享锁

也称为读锁，读锁允许多个连接可以同一时刻并发的读取同一资源,互不干扰；

#### 13.3.2排他锁

也称为写锁，一个写锁会阻塞其他的写锁或读锁，保证同一时刻只有一个连接可以写入数据，同时防止其他用户对这个数据的读写。

#### 13.4事务处理

#### 13.4.1事务的ACID特性

数据库的事务处理的原则是保证ACID的正确性。事务是由一组SQL语句组成的逻辑处理单元，事务具有以下4个属性：

(1) 原子性（Atomicity）：事务是一个原子操作单元，其对数据的修改，要么全都执行，要么全都不执行，不可能只执行其中的一部分。（不可分割）

(2) 一致性（Consistent）：在事务开始和完成时，数据都必须保持一致状态。这意味着所有相关的数据规则都必须应用于事务的修改，以保持数据的完整性；事务结束时，所有的内部数据结构(如B树索引或双向链表)也都必须是正确的。（状态更改一致性）

(3) 隔离性（Isolation）：数据库系统提供一定的隔离机制，保证事务在不受外部并发操作影响的“独立”环境执行。这意味着事务处理过程中的中间状态对外部是不可见的。（执行过程隔离不可见）

(4) 持久性（Durable）：事务完成之后，它对于数据的修改是永久性的，即使出现系统故障也能够保持。（持久生效）

### 13.5 事务处理带来的问题

​	由于事务的并发执行，带来以下一些著名的问题：

#### 13.5.1更新丢失（Lost Update）

​	当两个或多个事务选择同一行，然后基于最初选定的值更新该行时，由于每个事务都不知道其他事务的存在，就会发生丢失更新问题－－最后的更新覆盖了由其他事务所做的更新。

#### 13.5.2脏读（Dirty Reads）

​	一个事务正在对一条记录做修改，在这个事务完成并提交前，这条记录的数据就处于不一致状态；这时，另一个事务也来读取同一条记录，如果不加控制，第二个事务读取了这些"脏"数据，并据此做进一步的处理，就会产生未提交的数据依赖关系。这种现象被形象地叫做"脏读"。

#### 13.5.2不可重复读（Non-Repeatable Reads）

​	一个事务在读取某些数据后的某个时间，再次读取以前读过的数据，却发现其读出的数据已经发生了改变、或某些记录已经被删除了！这种现象就叫做"不可重复读"。

#### 13.5.3 幻读（Phantom Reads）。

​	一个事务按相同的查询条件重新读取以前检索过的数据，却发现其他事务插入了满足其查询条件的新数据，这种现象就称为"幻读"。

### 13.6Mysql隔离级别

READ UNCOMMITTED ：事务可以看到其他事务没有被提交的数据（脏数据）。
READ COMMITTED ：事务可以看到其他事务已经提交的数据。
REPEATABLE READ ：保证事务中多次查询的结果相同（Innodb默认级别），会出现幻读。
SERIALIZABLE ：所有事务顺序执行，对所有read操作加锁。保证一致性。

![img](https://img2018.cnblogs.com/blog/1279026/201810/1279026-20181013152937250-755169707.png)

 

```mysql
#查看mysql隔离级别
Show variables  like 'tx_isolation';
#读未提交
set global transaction isolation level read  uncommitted;
#读已提交
set global transaction isolation level read  committed;
#可重复读
set global transaction isolation level repeatable read;
#可串行化
set global transaction isolation level serializable;
```



### 13.7多版本并发控制 

​	MVCC的实现：通过保存数据资源在不同时间点的快照实现的。根据事务开始的时间不同，每个事务看到的数据快照版本是不一样的。

​	InnoDB中的MVCC实现：通过在每行记录后面保存两个隐藏的列来实现，一个保存了行的创建时间，一个保存了行的过期时间。

#### 13.7.1SELECT

当读取记录时，存储引擎会选取满足下面两个条件的行作为读取结果。

读取记录行的开始版本号必须早于当前事务的版本号。也就是说，在当前事务开始之前，这条记录已经存在。在事务开始之后才插入的行，事务不会看到。

读取记录行的过期版本号必须晚于当前事务的版本号。也就是说，当前事务开始的时候，这条记录还没有过期。在事务开始之前就已经过期的数据行，该事务也不会看到。

#### 13.7.2INSERT

存储引擎为新插入的每一行保存当前的系统版本号作为这一行的开始版本号。

#### 13.7.3UPDATE

存储引擎会新插入一行记录，当前的系统版本号就是新记录行的开始版本号。同时会将原来行的过期版本号设为当前的系统版本号。

#### 13.7.4DELETE

存储引擎将删除的记录行的过期版本号设置为当前的系统版本号。

MVCC只在 REPEATABLE READ 和 READ COMMITTED 两个隔离级别下工作。