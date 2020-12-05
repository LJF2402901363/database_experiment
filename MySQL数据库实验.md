# MySQL数据库实验

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

***\*其语法格式如下：\****

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
create view v_em(employeeID,name,realIncome) as select employee.employeeID,employee.name,(salary.income -\**** ***\*salary.outcome ) as realIncome from  employee  join  salary on employee.employeeID = salary.employeeID;
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