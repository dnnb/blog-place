---
title: SQL介绍
date: 2018-6-24 
categories: 数据库

---
## 简介

　　SQL是英文Structured Query Language的缩写，意思为结构化查询语言。SQL语言的主要功能就是同各种数据库建立联系，进行沟通。按照ANSI(美国国家标准协会)的规定，SQL被作为关系型数据库管理系统的标准语言。

<!--more-->

## 发展历史

　　在1970年代初，由IBM公司San Jose,California研究实验室的埃德加·科德发表将数据组成表格的应用原则（Codd's Relational Algebra）。

　　1974年，San Jose,California研究实验室的D.D.Chamberlin和R.F. Boyce对Codd's Relational Algebra在研制关系数据库管理系统System R中，研制出一套规范语言-SEQUEL（Structured English QUEry Language），并在1976年11月的IBM Journal of R&D上公布新版本的SQL（叫SEQUEL/2）。

　　1980年改名为SQL。

　　1979年ORACLE公司首先提供商用的SQL，IBM公司在DB2和SQL/DS数据库系统中也实现了SQL。

　　1986年10月，美国ANSI采用SQL作为关系数据库管理系统的标准语言（ANSI X3. 135-1986），后为国际标准化组织（ISO）采纳为国际标准。

　　1989年，美国ANSI采纳在ANSI X3.135-1989报告中定义的关系数据库管理系统的SQL标准语言，称为ANSI SQL 89，该标准替代ANSI X3.135-1986版本。

## SQL语句结构分类

#### 一、数据查询语言（DQL:Data Query Language）：

> DQL语句，也称为“数据检索语句”，用以从表中获得数据，确定数据怎样在应用程序给出。保留字SELECT是DQL（也是所有SQL）用得最多的动词，其他DQL常用的保留字有WHERE，ORDER BY，GROUP BY和HAVING。这些DQL保留字常与其他类型的SQL语句一起使用。

#### 二、 数据操作语言（DML：Data Manipulation Language）：

> DML包括动词INSERT，UPDATE和DELETE。它们分别用于添加，修改和删除表中的行。也称为动作查询语言。

#### 三、 事务处理语言（TCL：Transaction Control Language）：

> TCL语句能确保被DML语句影响的表的所有行及时得以更新。TCL语句包括BEGIN TRANSACTION，COMMIT和ROLLBACK。

#### 四、 数据控制语言（DCL：Data Control Language）：

> DCL的语句通过GRANT或REVOKE获得许可，确定单个用户和用户组对数据库对象的访问。某些RDBMS可用GRANT或REVOKE控制对表单个列的访问。

#### 五、 数据定义语言（DDL：Data Definition Language）：

> DDL的语句包括动词CREATE和DROP。在数据库中创建新表或删除表（CREAT TABLE 或 DROP TABLE）；为表加入索引等。DDL包括许多与人数据库目录中获得数据有关的保留字。它也是动作查询的一部分。

## 用法介绍

### 数据定义语言DDL

数据定义语言DDL用来创建数据库中的各种对象-----表、视图、索引、同义词、聚簇等（CREATE TABLE/VIEW/INDEX/SYN/CLUSTER）：

#### 对数据库的操作

##### 创建:

	create database <数据库名>；创建数据库
	create database <数据库名> character set utf8; 创建数据库并设置编码为utf-8；
	create database <数据库名> character set gbk collate gbk_chinese_ci  创建数据库，并设置编码为gbk，增加校验

##### 删除

	drop database <数据库名>; 删除数据库

##### 修改

	alter database <数据库名> character set utf8;  修改数据库编码为utf-8；

##### 查询

	show databases; 查看所有数据库
	show create database <数据库名>; 查看创建数据库的定义信息

##### 其它

	select database(); 查看当前使用数据库
	use <数据库名>; 使用数据库

#### 对数据表操作：

##### 创建
	
	CREATE TABLE <表名> (
	  `id` int(11) NOT NULL AUTO_INCREMENT,
	  `name` varchar(255) DEFAULT NULL,
	  `remark` varchar(255) DEFAULT NULL,
	  `appid` int(11) DEFAULT NULL,
	  `weight` float DEFAULT '1',
	  PRIMARY KEY (`id`),
	  UNIQUE KEY `appid` (`appid`,`name`) USING BTREE,
	  CONSTRAINT `appid` FOREIGN KEY (`appid`) REFERENCES <数据库名> (<要关联的列明>) ON DELETE CASCADE ON UPDATE CASCADE
	) ENGINE=InnoDB AUTO_INCREMENT=33 DEFAULT CHARSET=utf8

	
##### 查询

	show tables; 当前数据库中的所有表
	desc <表名>; 查看表的字段信息
	show create table <表名>;  查看表格的创建细节

##### 修改

	alter table <表名> add <列名> <数据类型>; 在表中增加一列；
	alter table <表名> modify <列名> varchar(60); 修改列的数据类型 ;
	alter table <表名> change <旧列名> <新列名> varchar(100); 修改列名，带上数据类型；
	alter table <表名> drop <列名>; 删除列；
	alter table <表名> character set utf8; 修改表的字符集为utf-8；

##### 删除

	rename table <旧的表名> to <新的表名>; 修改表名；
	drop table <表名>; 删除表(结构和数据)；
	truncate table <表名>;清空表记录
	delete from <表名> [where <删除条件>];删除表记录可根据条件删除部分

### 数据控制语言DCL

数据控制语言DCL用来授予或回收访问数据库的某种特权，并控制数据库操纵事务发生的时间及效果，对数据库实行监视等。

在MySQL中，不仅可以指定谁可以连接到数据库服务器，还可以指定用户连接的主机。因此，MySQL中的用户帐号由用户名，以及使用@字符分隔的主机名组成。

例如，如果admin用户从localhost主机连接到MySQL数据库服务器，则用户帐户是书写形式是：admin@localhost，其中@符号是一个固定的分隔符。

admin用户只能从本地主机(localhost)连接到MySQL数据库服务器，而不是其它远程主机。

>注意：如要限制admin用户只能从192.168.10.12主机登录，那么可以书写为：`admin@192.168.10.12`，如果想要允许从任意主机登录，那么可以书写为：`admin@%`；MySQL将用户帐户授权存储在mysql数据库的user表中。


#### create 创建用户

	create user 用户名@地址 identified by 密码;

> 其中使用的IP地址，可以设置为localhost(代表本机)或者'%'（代表允许所有IP地址登录）

#### grant 给用户授权
	
 	GRANT SELECT,INSERT ON 数据库名.* TO 用户名@IP地址 [IDENTIFIED BY 密码];
	给用户添加对其中一个数据库所有表SELECT和INSERT的权限

	grant all [privileges] on 数据库名 to 用户名@IP地址
	给用户添加对某个数据库所有的操作权限，关键字 “privileges” 可以省略

	grant all on . to 用户名@IP地址
	给某个用户添加对所有数据库的权限
	
	FLUSH PRIVILEGES;
	刷新权限

#### revoke 撤销授权

	REVOKE INSERT ON 数据库.* FROM 用户名@IP地址;
	取消用户对其中一个数据库所有表INSERT的权限

	revoke all on . from 用户名@IP地址;
	取消用户对所有数据库的操作

	FLUSH PRIVILEGES;
	刷新权限

> 注意：grant, revoke 用户权限后，该用户只有重新连接 MySQL数据库，权限才能生效。mysql不同版本直接略有差异...
	
#### ROLLBACK 事物回滚

	标识事务的开始：

		start transaction

	使用ROLLBACK 命令用来回退（撤销）：

		start transaction;
		delete from 表名;
		...(各种修改删除操作)
		rollback;

 
>ROLLBACK 只能在一个事务处理内使用（在执行一条 START TRANSACTION 命令之后）。

#### COMMIT提交事物
	
一般的MySQL语句都是直接针对数据库表执行和编写的。这就是所谓的隐含提交（implicit commit），即提交（写或保存）操作是自动进行的。但是，在事务处理块中，提交不会隐含地进行。为进行明确的提交，使用 COMMIT 语句，如下所示：

	start transaction;
	delete from 表名 where id = 123;
	delete from 表名 where id = 321;
	commit;

提交数据有三种类型：显式提交、隐式提交及自动提交。

1. 显式提交
	
	用COMMIT命令直接完成的提交为显式提交。

1. 隐式提交
	
	用SQL命令间接完成的提交为隐式提交。这些命令有：ALTER，AUDIT，COMMENT，CONNECT，CREATE，DISCONNECT，DROP，EXIT，GRANT，NOAUDIT，QUIT，REVOKE，RENAME。

1. 自动提交
	
	若把AUTOCOMMIT设置为ON，则在插入、修改、删除语句执行后，系统将自动进行提交。命令为：SET AUTOCOMMIT ON；

### 数据操作语言DML

DML主要有三种形式：

1. 插入：INSERT

	insert into 表名(...) values(...);

1. 更新：UPDATE
	
	update 表名 set 列名 = 更新内容 [where 条件];

1. 删除：DELETE

	delete from 表名[where 条件];

>注意： 在mysql中，字符串类型和日期类型都要用单引号括起来。


### 数据查询语言DQL

数据查询语言DQL由SELECT子句，FROM子句，WHERE子句组成：

	SELECT <字段名表>FROM <表或视图名>WHERE <查询条件>

数据库执行DQL语句不会对数据进行改变，而是让数据库发送结果集给客户端。

模糊查询 like    通配符： _任意一个字母   % 任意个字母。

#### 无条件单表查询

1. 查询表中所有列 

	Select * from emp;

2. 查询表中指定列 
	
	可以更改列的顺序

		Select ename,eno from emp; 
		Select eno,ename from emp; 
		

3. 算术运算符 

	Select 子句中支持加减乘除和函数 

	查看员工年薪
 
		Select ename,empno,sal*12+comm from emp; 

	员工转正后，月薪上调20%， 请查询出所有员工转正的月薪；转正日期为入职后的6个月 

		select ENAME ‘员工姓名’ , SAL ‘实习工资’ , hiredate ‘入职日期’, hiredate+interval 6 month ‘转正日期’, sal*1.2 ‘转正工资’ from emp; 
	
	员工试用期6个月，转正后月薪上调20%，请查询所有员工工作第一年的年薪所得（不考虑奖金部分，年薪的试用期6个月的月薪+转正后的6个月的月薪） 

		select ENAME ‘员工姓名’ , SAL ‘实习工资’ , hiredate ‘入职日期’, hiredate+interval 6 month ‘转正日期’, sal*6+sal*1.2*6 ‘年薪’ from emp; 
		或者 
		select ename,sal,DATE_ADD(hiredate,INTERVAL 6 MONTH) as newdate from emp;

4. null 

	任何值与null计算结果都为null 
	通过ifnull 将 null 置为0 

		select ename,sal * 12+ifnull(comm,0) from emp;

5. 别名 

	Select 列 ‘列名’ from emp; 

	Select 列 as ‘列名’ from emp; 

	员工试用期6个月，转正后月薪上调20%，请查询出所有员工工作第一年的所有收入，需要考虑奖金部分,要求显示列标题为员工姓名，工资收入， 奖金收入，总收入 
	
		select ENAME ‘员工姓名’ , sal*6+sal*1.2*6 ‘工资收入’, ifnull(comm, 0) ‘奖金收入’ , sal*6+sal*1.2*6+ifnull(comm, 0) ‘年薪’ from emp;

6. 连接字符串 

	Oracle 使用||进行字符串连接 

	Sql server 使用+进行字符串连接 

	Mysql 使用cancat()函数进行字符串连接 

		select concat(ename,‘的岗位是’,job) ‘员工岗位’ from emp;

7. 消除重复行
 
	select distinct job from emp;

#### 有条件单表查询

1． Where 

关系运算符（< <= > >= <> != = ） 可以对数值型 字符型 日期型 直接进行运算 

	select empno,ename,deptno from emp where ename>’王’; 

特殊运算符（between and in like is null） 

	between 上限 and 下限 结果集操作包含上限和下限对应的行 
	
	like 模糊查询 %零个或者多个字符_表示有且仅有一个字符可以通过escape标示符实现对_ % 本身这两个字符的查找 不区分大小写

	select empno,ename,deptno from emp where empno between '00002' and '00003';
	select *  from emp where comm in(500,800);
	select empno,ename,deptno from emp where empno in('00002','00003' );
	select *  from emp where ename like '王%';
	select *  from emp where hiredate like '_____03%';
	select *  from emp where ename like '%/_%' escape '/';

逻辑运算符（ and or not） 

优先级: not and or

数量限制

	Sql server top 
	Oracle renum 
	Mysql limit 

>Limit 0,5 0代表起始行 5代表起始行开始连续的行的个数 
>limit (pagesize-1)*pizenum,pizenum

2．Order by
 
默认为升序 asc 

降序序显式声明为desc
 
	select * from emp order by deptno desc; 
	select ename,job from emp order by deptno asc;

多列排序
 
	select * from emp order by deptno asc,sal desc; 
	select ename,job from emp order by 2 asc;

mysql中用命令行复制表结构的方法主要有一下几种:

1.只复制表结构到新表 

	CREATE TABLE 新表 SELECT * FROM 旧表 WHERE id>2 ; 
	或 
	CREATE TABLE 新表 LIKE 旧表 ; 

> 注意:上面两种方式，前一种方式是不会复制主键类型和自增方式，而后一种方式是把旧表的所有字段类型都复制到新表。

2.复制表结构及数据到新表 

	CREATE TABLE 新表 SELECT * FROM 旧表

3.复制旧表的数据到新表(假设两个表结构一样) 

	INSERT INTO 新表 SELECT * FROM 旧表

4.复制旧表的数据到新表(假设两个表结构不一样) 

	INSERT INTO 新表(字段1,字段2,…….) SELECT 字段1,字段2,…… FROM 旧表

#### 多表查询

1. 交叉连接

　　不使用任何条件匹配,查询结果为笛卡尔乘积（就是两个集合中的每一个成员，都与对方集合中的任意一个成员有关联）。示例：
	
	select a.*,b.* from a,b;

2. 内连接

　　查询结果返回两张表中相匹配的行，内连接分为隐式连接和显示连接两种，示例：

	显示：
	select * from
	a [inner] join b
	on a.id = b.id;

	隐式：
	select a.*, b.*
	from a, b
	where a.id = b.id;


3. 外连接

左外连：

会显示左边表内所有的值，不论在右边表内匹不匹配，匹配不到的右边表中的数据为NULL。示例：
	
	select a.*, b.* 
	from a left join b
	on a.id = b.id;

右外连：

会显示右边表内所有的值，不论在左边表内匹不匹配，匹配不到的左边表中的数据为NULL。示例：
	
	select a.*, b.* 
	from a right join b
	on a.id = b.id;

4. 子查询
	
子查询是将一个查询语句嵌套在另一个查询语句中。内层查询语句的查询结果，可以为外层查询语句提供查询条件。示例：
	
	select * 
	from a
	where id in (select id from b where age<8);

#### 函数

**字符函数**

大小写处理 

	LOWER(str) UPPER(str) 
	SELECT LOWER(ename) from emp; 
	SELECT UPPER(ENAME) from emp;

字符处理函数 

	CONCAT(str1,str2,…) SUBSTR(str,pos,len) LENGTH(str)

	SELECT CONCAT(ename,’ 的薪水是 ‘,sal) 薪水 FROM emp; 
	SELECT SUBSTR(ename,1,1) 姓氏 FROM emp; 
	SELECT SUBSTR(ename,2) 名字 FROM emp; 
	SELECT SUBSTR(ename,-1,1) 名字 FROM emp;
	SELECT ename,LENGTH(ename) 名字存储字节长度 FROM emp; 
	SELECT ename,CHAR_LENGTH(ename) 名字字符长度 FROM emp;

字符处理函数 

　　INSTR(str,substr) ：填补
 
　　LPAD(str,len,padstr) ：用字符串 padstr对 str进行左边填补直至它的长度达到 len个字符长度，然后返回 str。如果 str的长度长于 len’，那么它将被截除到 len个字符。 

　　RPAD(str,len,padstr) ：用字符串 padstr对 str进行右边填补直至它的长度达到 len个字符长度，然后返回 str。如果 str的长度长于 len’，那么它将被截除到 len个字符。 

　　TRIM([remstr FROM] str) ：修剪，去掉字符串首尾特点的字符，默认为空格。

	SELECT * from dept; 
	SELECT loc,INSTR(LOC,'楼') FROM dept; 
	SELECT loc,LPAD(loc,10,'#') FROM dept; 
	SELECT loc,RPAD(loc,2,'#') FROM dept; 
	SELECT loc,REPLACE(loc,'楼','栋') FROM dept 
	SELECT TRIM(' 你好 '),TRIM('s' from 'sssdsssddss') FROM DUAL;

查询姓名包含大写或小写字母a的员工姓名:
 
	select ename,CHAR_LENGTH(ename) from emp where instr(upper(ename),'A')>0;

**数值函数**

	SELECT FLOOR(RAND()*100) FROM DUAL; 
	SELECT sal,CONCAT('$',ROUND(sal,2))from emp;

>ROUND：Round（number，[decimals]）<br> 
>number : 待做四舍五入处理的数值 <br> 
>decimals : 指明需保留小数点后面的位数。可选项，忽略它则保留0位小数，精确到个位；为负数，表示为小数点左边四舍五入处理。

**日期函数**

时间相加 

	SELECT DATE_ADD(CURDATE(), INTERVAL 31 DAY) 
	SELECT ADDDATE(CURDATE(), INTERVAL 31 DAY)

时间查询 

	SELECT DATE(‘2003-12-31 01:02:03’) 
	SELECT DATEDIFF(‘1997-11-30’,’1997-12-31’) 
	SELECT SUBDATE(CURDATE(), INTERVAL 3 DAY) 
	SELECT INTERVAL 1 DAY + ‘1997-12-31’

日期格式调整
	 
	SELECT DATE_FORMAT(CURDATE(),’%Y年%M月%D日’) 
	SELECT DAYOFMONTH(NOW()),DAYOFWEEK(NOW())

取出日期 

	SELECT DATE(NOW())


> 注意：<br>
> 查询语句书写顺序： select  -  from  -  where  -  group by  --  having  --  ordre by --  limit;<br>
查询语句执行顺序： from -  where -- group by -- having -- select  -- order by -- lilmit;

...

