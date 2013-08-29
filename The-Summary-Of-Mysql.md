Date: 2013-05-21
Tags: Linux Mac Mysql

Title:The Summary Of Mysql

##数据存储方式
人工管理-文件系统-数据库系统

##数据库范型
数据库应该遵循的规则,也称为范式。最常用的4种范式：第一范式，第二范式，第三范式，BCN范式。

##SQL语法组成：
全称为结构化查询语言（Structured Query Language)，数据库管理系统通过SQL语言管理数据库中的数据。

分为3个部分：
- 数据定义语言：CREATE TABLE、 DROP TABLE、 ALTER TABLE
- 数据操作语言：查询、插入、删除、修改（SELECT、INSERT、DELETE、UPDATE）
- 数据控制语言：存取许可、存取权限（GRANT、REVOKE）

##常见数据库系统
- 甲骨文的Oracle
- IBM的DB2
- 微软的Access和SQL Server
- 开源PostgreSQL
- 开源MySQL
- 文件数据库SQLite
- 内存数据库HQL

##Mysql数据类型
- 整数类型：整型可以由十进制和十六进制表示。十六进制：0x[1-9A-F],0x中的x必须小写。包括tinyint,smallint,mediumint,int,bigint。
- 浮点数：float,double,decimal(m,d)。decimal(6,2)定义的数字如1234.56，指总长6位，小数点后精确到2位。
- 字符串:char定长，varchar(m)不定长,tinytext，text，mediumtext，longtext。
- 日期和时间：year年，date日期，time时间，datetime时间，timestamp时间。
- 二进制：binary(m),varbinary(m),bit(m),tinyblob,blob,mediumblob,longdlob。
- NULL值。

##登录语法： 
    mysql [-u username] [-h host] [-p[password]] [dbname] 
    #示例：(初始管理帐号是root，没有密码）
    mysql -uroot -hlocalhost -p123456 zarkpy

    #-e执行SQL语句，可用此语句配合操作系统定时任务,达到自动处理数据的功能,如定时将某表中过期的数据删除。
    mysql –h hostname|hostIP –P port –u username –p[password] databaseName -e "SQL语句"

##Bash内直接打开目标数据库：
    alias mszarkpy = 'mysql -uroot --default-character-set=utf8 --database zarkpy'

##mysql的几个重要目录：
mysql的数据库文件、配置文件、命令文件分别在不同的目录。

相关命令：/usr/bin,如mysql,mysqladmin,mysqldump等命令。

数据库目录：/var/lib/mysql/

配置文件：/usr/share/mysql

启动脚本：/etc/init.d

##启动与停止
启动：/etc/init.d/mysql start 

停止：/usr/bin/mysqladmin -u root -p shutdown 

##操作数据库
    create database database_name;    #创建数据库
    show databases;    #显示所有数据库
    use database_name;    #打开某个数据库
    select database();    #查看当前使用的数据库
    drop database [if exist] database_name;    #删除，给出if exists子句，删除不存在的数据库时不会出错。


##数据库存储引擎
innoDB：支持事务，回滚，外键约束、崩溃恢复。表结构存在.frm文件中，数据和索引存在表空间中，读写效率稍差，占用空间大。

MyISAM：不支持事务和并发。表结构存在.frm文件中，.myd存储数据，.myi存储索引。快速，占用空间小，成熟稳定，易于管理。

    #在创建一个新的MySQL数据表时，可以为它设置存储引擎:
    create table tmp(…)ENGINE=MyISAM

    show engines \G;    #当前mysql支持的所有engine，row列表形式显示
    show engines;    #当前mysql支持的所有engine，表格形式显示
    show variables like '%engine%';    #查看当前库的engine

##创建表
    show tables   #显示数据库中的所有表

    #在当前数据库中创建表：
    create table 表名(
       列名1 数据类型 [<列的完整性约束>],
       列名2 数据类型 [<列的完整性约束>],
       ...
    );

    #建立一个表school,其由两列组成，第一列属性为非空，并做为主键,并自增:
    create table school(
       id int(10) not null auto_increment primary key,
       name varchar(20)
    );

完整性约束条件表：
- PRIMARY  KEY       主键
- FOREIGN  KEY       外键
- UNIQUE             唯一性约束
- NOT  NULL          不能为空          
- AUTO_INCREMENT     整数列默认自增1
- DEFAULT default_value       默认值约束
- UNSIGNED    无符号整数
- DEFAULT cur_timestamp 创建新记录时默认保存当前时间（仅适用timestamp数据列）
- ON UPDATE cur_timestamp 修改记录时默认保存当前时间（仅适用timestamp数据列）
- CHARACTER SET name 指定字符集（仅适用字符串）


完整性约束示例:

    #设置表的单字段主键：属性名 数据类型 PRIMARY KEY
    create table school(
       id int(10) primary key,
       name varchar(20)
    );

    #设置表的多字段主键：PRIMARY KEY(属性名1,属性名2,属性名3...)
    create table school(
       id int(10),
       name varchar(20),
       primary key(id,name)
    );

    #设置表的外键：FOREIGN KEY(属性名1,属性名2...) REFERENCES 表名(属性名1,属性名2...)
    create table score(
       cid int(10) not null auto_increment primary key,
       score int, 
       sid int,
       foreign key(sid) references student(sid)
       #定义外键别名
       #constraint score_foreign foreign key(sid) references student(sid)
    );

    #设置表的非空约束：属性名 数据类型 NOT NULL
    #设置表的唯一性约束：属性名 数据类型 UNIQUE
    #设置表的属性值自动增加：属性名 数据类型 AUTO_INCREMENT
    #设置表的属性的默认值：属性名 数据类型 DEFAULT 默认值
    create table school(
       id int(10) not null auto_increment primary key,
       name varchar(200)  charset utf8 not null default '',

    );

##显示表结构：

    describe User;
    desc User;
    show columns from User;
    show create table table_name;   #查看创建表的详细结构语句
    show create table table_name\G;   #查看创建表的详细结构语句


##修改表：

    ALTER TABLE 旧表名 RENAME [TO] 新表名;  #修改表名
    RENAME TABLE 原表名 TO 新表名;    #修改表名


    ALTER TABLE 表名 MODIFY 属性名 数据类型;    #修改字段的数据类型
    ALTER TABLE 表名 CHANGE 旧属性名 新属性名 新数据类型   #修改字段名及数据类型
    ALTER TABLE 表名 ADD 属性名1 数据类型 [完整性约束条件] [FIRST | AFTER 属性名2];    #增加字段
    ALTER TABLE 表名 DROP 属性名    #删除字段

    ALTER TABLE 表名 ENGINE=INNODB | MYISAM | MEMOERY;   #更改表的存储引擎

    ALTER TABLE 表名 DROP FOREIGN KEY 外键别名;   #删除表的外键约束

    ALTER TABLE 表名 DROP INDEX 字段名;    #删除某个表中某个字段的unique key

##删除表
    DROP TABLE 表名;   #删除没有被关联的普通表

    #删除被其他表关联的父表
    DROP TABLE 表名;    #直接删除会报错
    ALTER TABLE 表名 DROP FOREIGN KEY 外键别名;   #需要先删除其他表的外键约束
    DROP TABLE 表名;


##常用操作


查询记录：select * from User;

增加记录：insert into User values('','','')

只增加某些字段的记录：insert into table_name (col1,col2) values (col1,col2); 

增加多条记录：insert into User values('','',''),('','',''),('','','')

修改记录：update User set name = 'ajie' where id ='1';

删除记录：delete from User where id ='1';

清空表：delete from User;

在表中增加字段：alter table 表名 add 字段 类型 其他; 
                alter table User add test int(4) default '0'
                
在某个范围查询：select * from user where uid between 2 and 5

##索引
Mysql中所有数据类型都可以被索引。[\[查看更多\]](../../../../2012/12/05/Mysql索引的使用/ "Mysql索引的使用").

优点：提高查询、分级和排序的时间。

缺点：占空间，创建、更新记录时额外耗费时间。

##视图
作用：方便用户对数据的操作

    CREATE [ ALGORITHM = { UNDEFINED | MERGE | TEMPTABLE } ] 
    VIEW 视图名 [ ( 属性清单 ) ]
    AS SELECT 语句
    [ WITH [ CASCADED | LOCAL ] CHECK OPTION ] ;

    #ALGORITHM 参数表示视图选择的算法
        #UNDEFINED 未指定,自动选择
        #MERGE 表示将使用视图的语句和视图定义合并起来,使得视图定义的某一部分取代语句的对应部分
        #TEMPTABLE 表示将视图的结果存入临时表,然后使用临时表执行语句

    #LOCAL 参数表示更新视图时要满足该视图本身定义的条件即可;
    #CASCADED 参数表示更新视图时要满足所有相关视图和表的条件,默认值。
    #使用 CREATE VIEW 语句创建视图时,最好加上 WITH CHECK OPTION 参数和 CASCADED 参数。这样,从视图上派生出来的
    #新视图后,更新新视图需要考虑其父视图的约束条件。这种方式比较严格,可以保证数据的安全性。


    create view department_view1 as
    select * from department;
    
    create view department_view2 (name, function, localtion) as
    select d_name, function, address from department;
    
    create algorithm=merge view worker_view1(name,department, sex, age, address) as
    select name,department.d_name,sex,2009-birthday,address from worker,department
    where worker.d_id=department.d_id
    with local check option;


    查看视图
    DESCRIBE|DESC 视图名 ;
    SHOW TABLE STATUS LIKE ‘视图名’ ;
    SHOW CREATE VIEW 视图名;
    SELECT *
    FROM information_schema.views ;


    修改视图
    CREATE OR REPLACE | ALTER [ ALGORITHM = { UNDEFINED | MERGE | TEMPTABLE } ]
    VIEW 视图名 [ ( 属性清单 ) ]
    AS SELECT 语句
    [ WITH [ CASCADED | LOCAL ] CHECK OPTION ] ;


    更新视图
    更新视图是指通过视图来插入(INSERT)、更新(UPDATE)和删除(DELETE)表中的数据。因为是视图是一个虚拟表,其中
    没有数据。通过视图更新时,都是转换到基本表来更新。更新视图时,只能更新权限范围内的数据。超出了范围,就不能更新。
    原则:尽量不要更新视图
    语法和 UPDATE 语法一样
    哪些视图更新不了:
    - 视图中包含 SUM(), COUNT()等聚焦凼数的
    - 视图中包含 UNION、UNION ALL、DISTINCT、GROUP BY、HAVING 等关键字
    - 常量视图 CREATE VIEW view_now AS SELECT NOW()
    - 视图中包含子查询
    - 由不可更新的视图导出的视图
    - 创建视图时 ALGORITHM 为 TEMPTABLE 类型
    - 视图对应的表上存在没有默认值的列,而且该列没有包含在视图里
    - WITH [CASCADED|LOCAL] CHECK OPTION 也将决定视图是否可以更新
      LOCAL 参数表示更新视图时要满足该视图本身定义的条件即可;
      CASCADED 参数表示更新视图时要满足所有相关视图和表的条件,默认值。


    删除视图
    删除视图时,只能删除视图的定义,不会删除数据
    DROP VIEW [ IF EXISTS] 视图名列表 [ RESTRICT | CASCADE]


##触发器
触发器(TRIGGER)是由事件来触发某个操作。这些事件包括 INSERT、UPDATE和DELETE语句。当数据库系统执行这些事件时,就会激活触发器执行相应的操作。

    创建只有一个执行语句的触发器
    CREATE TRIGGER 触发器名 BEFORE | AFTER 触发事件
    ON 表名 FOR EACH
    ROW 执行语句

    创建有多个执行语句的触发器
    DELIMITER &&
    CREATE TRIGGER 触发器名 BEFORE | AFTER 触发事件
    ON 表名 FOR EACH ROW
    BEGIN
    执行语句列表
    END
    &&
    DELEMITER ;

    DELIMITER,一般SQL以“;”结束,在创建多个语句执行的触发器时,要用到“;“,所以用DELIMETER 来切换一下。
    CREATE TRIGGER dept_tig1 BEFORE INSERT ON department
    FOR EACH ROW
    INSERT INTO trigger_time VALUES(NOWS());

    查看触发器
    SHOW TRIGGERS ;
    SELECT * FROM information_schema.triggers ;


    触发器的使用
    MySQL中,触发器执行的顺序是BEFORE触发器、表操作(INSERT、UPDATE 和 DELETE)、AFTER 触发器。
    触发器中不能包含START TRANSACTION, COMMIT, ROLLBACK,CALL等。

    删除触发器
    DROP TRIGGER 触发器名 ;

##查询数据

    SELECT 属性列表
    FROM 表名和视图列表
    [WHERE 条件表达式 1]
    [GROUP BY 属性名 1 [HAVING 条件表达式 2]]
    [ORDER BY 属性名 2[ASC|DESC]]
    LIMIT [刜始位置,] 记录数


    #使用集合函数查询
    COUNT(), AVG(), MAX(), MIN(), SUM()

    #连接查询
    #内连接查询
    select a.*, b.* from a, b where a.xid=b.xid
    
    #外连接查询
    SELECT 属性名列表 FROM 表名 1 LEFT|RIGHT JOIN 表名 2
    ON 表名 1.属性 1=表名 2.属性 2;
    LEFT JOIN 左表全记录,右表符合条件
    RIGHT JOIN 右表全记录,左表符合条件

    #子查询
    SELECT * FROM computer_stu WHERE scrore >=ANY | ALL(SELECT score FROM scholarship)

    #合并查询结果
    SELECT 语句 1
    UNION | UNION ALL   #UNION合并所有查询结果,去掉重复项,UNION ALL简单合并
    SELECT 语句 2


使用正则表达式查询
    
    属性名 REGEXP ‘匹配方式’
    SELECT * FROM info WHERE name REGEXP ‘ab{1,3}’;

- ^ 字符串开始
- $ 字符串结束
- . 任意一个字符,包括回车和换行
- [字符集合] 匹配字符集合中的任一字符
- S1|S2|S3 三者任一
- * 任意多个
- + 1+个
- 字符串{N} 字符串出现 N 次
- 字符串{M,N} 字符串出现至少 M 次,至多 N 次



##插入数据
    #为表的所有字段插入数据
    insert into 表名 values (值 1,值 2...值 n)
    insert into 表名 (属性 1,属性 2...属性 n) values (值 1,值 2...值 n)

    #同时插入多条数据
    insert into 表名
        (属性 1,属性 2...属性 n) values (值 1,值 2...值 n),
        (属性 1,属性 2...属性 n) values (值 1,值 2...值 n),
        ...
        (属性 1,属性 2...属性 n) values (值 1,值 2...值 n);

    #将查询结果插入到表中
    insert into 表名 (属性 1,属性 2...属性 n)
    select 属性列表 from 表名 2 where 条件表达式
    use mysql;
    create table user1 (user char(16) not null);
    insert into user1 (user) select user from user;

##更新数据
    update 表名
    set 属性 1=值 1,属性 2=值 2,...
    where 条件表达式;

##删除数据
    delete from 表名 [条件表达式];


##系统信息函数
    VERSION()凼数返回数据库的版本号;
    CONNECTION_ID()凼数返回服务器的连接数,也就是刡现在为止 MySQL 服务的连接次数;
    DATABASE()和 SCHEMA()返回当前数据库名。
    USER()、SYSTEM_USER()、SESSION_USER()、CURRENT_USER()和 CURRENT_USER 这几个凼数可以返回当前用户的名称
    CHARSET(str)凼数返回字符串str的字符集,一般情况这个字符集就是系统的默认字符集;
    COLLATION(str)凼数返回字符串str的字符排列方式。
    LAST_INSERT_ID()凼数返回最后生成的AUTO_INCREMENT值。

##存储过程和存储函数

    #创建存储过程
    CREATE PROCEDURE sp_name ([proc_parameter[,...]])
    [characteristic ...] routine_body

    CREATE PROCEDURE num_from_employee (IN emp_id, INT, OUT count_num INT)
    READS SQL DATA
    BEGIN
    SELECT COUNT(*) INTO count_num
    FROM employee
    WHERE d_id=emp_id;
    END

    #创建存储函数
    CREATE FUNCTION sp_name ([func_parameter[,...]])
    RETURNS type
    [characteristic ...] routine_body

    CREATE FUNCTION name_from_employee(emp_id INT)
    RETURNS VARCHAR(20)
    BEGIN
    RETURN (SELECT name FROM employee WHERE num=emp_id);
    END


##调用存储过程和函数
存储过程是通过 CALL 诧句来调用的。
而存储凼数的使用方法不 MySQL 内部凼数的使用方法是一样的。
执行存储过程和存储凼数需要拥有 EXECUTE 权限。EXECUTE 权限的信息存储在 information_schema 数据库下面的 USER_PRIVILEGES 表中

    #调用存储过程
    CALL sp_name([parameter[,...]]) ;
    #调用存储函数
    存储凼数的使用方法不 MySQL 内部凼数的使用方法是一样的
    #查看存储过程和函数
    SHOW { PROCEDURE | FUNCTION } STATUS [ LIKE ' pattern ' ] ;
    SHOW CREATE { PROCEDURE | FUNCTION } sp_name ;
    SELECT * FROM information_schema.Routines WHERE ROUTINE_NAME=' sp_name ' ;

    #修改存储过程和函数
    ALTER {PROCEDURE | FUNCTION} sp_name [characteristic ...]
    characteristic:
    { CONTAINS SQL | NO SQL | READS SQL DATA | MODIFIES SQL DATA }
    | SQL SECURITY { DEFINER | INVOKER }
    | COMMENT 'string'
    #删除存储过程和函数
    DROP { PROCEDURE| FUNCTION } sp_name;

##光标的使用
存储过程中对多条记录处理,使用光标。
    
    #声明光标
    DECLARE cousor_name COURSOR FOR select statement;
    DECLARE cur_employee CURSOR FOR SELECT name, age FROM employee;

    #打开光标
    OPEN cursor_name;
    OPEN cur_employee;

    #使用光标
    FETCH cur_employee INTO var_name[,var_name...];
    FETCH cur_employee INTO emp_name, emp_age;

    #关闭光标
    CLOSE cursor_name
    CLOSE cur_employee

##增加Mysql用户

用CREATE USER语句新建

    CREATE USER user IDENTIFIED BY ’password’

用INSERT语句来新建普通用户

    INSERT INTO mysql.user(host,user,password,ssl_cipher,x509_issuer,x509_subject)
    VALUES (‘localhost’,’test2’,PASSWORD(‘test2’),’’,’’,’’);
    FLUSH PRIVELEGES;

用GRANT语句来新建普通用户
格式：grant select on 数据库.* to 用户名@登录主机 identified by "密码" 

    #增加一个用户user_1，密码为123，可以在任何主机上登录，
    #并对所有数据库有查询、插入、修改、删除的权限
    grant select,insert,update,delete on *.* to user_1@'%' identified by '123'
    
    #增加一个用户user_2，密码为123，只可以在localhost上登录,
    #并可以对数据库aaa进行查询、插入、修改、删除的操作
    grant select,insert,update,delete on aaa.* to user_2@localhost identified by '123'

    #允许做任何事，同root一样
    grant all [privileges] on ...

    #只允许登录，其他什么也不允许做
    grant usage on ...

#删除普通用户
    #用DROP USER语句来删除普通用户
    DROP USER user [,user]...;
    例:drop user ‘test’@’localhost’;

    #用DELETE语句来删除普通用户
    DELETE FROM mysql.user WHERE user=’username’ and host=’hostname’;
    FLUSH PRIVILEGES;

##用户管理
刚安装好的MySql包含一个含空密码的root帐户和一个匿名帐户，这是很大的安全隐患，在这里应把匿名帐户删除、 root帐户设置密码，可用如下命令进行：

    use mysql;
    delete from User where User="";
    update User set Password=PASSWORD('newpassword') where User='root';

如果要对用户所用的登录终端进行限制，可以更新User表中相应用户的Host字段。

##权限管理

    #授权
    grant select,insert,update,delete on aaa.* to user_2@localhost identified by '123'

    #收回权限
    REVOKE priv_type [(column_list)] [, priv_type [(column_list)]] ...
    ON [object_type] {tbl_name | * | *.* | db_name.*}
    FROM user [, user] ..

##修改登陆密码：

root用户修改自己的密码
    
    #使用mysqladmin命令来修改root用户的密码
    格式：mysqladmin -u用户名 -p旧密码 password 新密码 
    初始root没有密码时：mysqladmin -uroot password 123456
    更改旧密码：mysqladmin -uroot -p123456 password 654321
    
    #修改user表
    UPDATE mysql.user SET password=PASSWORD(“new_password”) WHERE user=’root’ and host=’’;
    FLUSH PRIVILEGES;
    
    #使用SET语句来修改root用户的密码
    SET PASSWORD=PASSWORD(“new_password”);

root用户修改普通用户密码
    
    #使用mysqladmin命令
    不适用,mysqladmin只能修改root 用户密码

    #修改user表
    UPDATE mysql.user SET password=PASSWORD(“new_password”) WHERE user=’’ and host=’’;
    FLUSH PRIVILEGES;
    
    #使用SET语句来修改普通用户的密码
    SET PASSWORD FOR ‘user’@’localhost’=PASSWORD(“new_password”);
    
    #用GRANT语句来修改普通用户的密码
    GRANT priv_type ON database.table TO user [IDENTIFIED BY [PASSWORD]’password
    [,user [IDENTIFIED BY [PASSWORD]’password’]]...

普通用户修改密码

    SET PASSWORD=PASSWORD(“new_password”);

root用户密码丢失的解决办法

    #1、使用—skip-grant-tables选项来启动MySQL服务
    mysqld –skip-grant-tables
    #/etc/init.d/mysql start –mysqld –skip-grant-tables

    #2、登录root,设置新密码    
    mysql –u root
    update mysql.user set password=password(“new_password”) where user=’root’ and host=’localhost’;

    #3、加载权限表
    fush privileges;

##备份数据库
    #导出整个数据库
    #mysqldump -u 用户名 -p --default-character-set=latin1 数据库名 > 导出的文件名（数据库默认编码是latin1）
    mysqldump -uroot --default-character-set=latin1 aoaola > aoaola_backup.sql
    
    #同时导出若干个数据库
    mysqldump -uroot -p --database aoaola zarkpy > ~/two.sql
    
    #同时导出所有数据库
    mysqldump -uroot -p --all-databases > ~/all.sql

    #导出一个表
    #mysqldump -u 用户名 -p 数据库名 表名 > 导出的文件名
    mysqldump -uroot aoaola User > aoaola_user.sql

    #导出一个数据库结构
    mysqldump -uroot -d –add-drop-table aoaola > aoaola_struc.sql

    #MyISAM 存储引擎的的表适用直接复制整个数据库目录



    #使用source命令导入数据库
    mysql -uroot aoaola
    source aoaola_backup.sql

    #使用mysqldump命令导入数据库
    mysqldump -uroot aoaola < aoaola_backup.sql

    #使用mysql命令导入数据库
    mysql -uroot -D aoaola < aoaola_backup.sql
    mysql -uroot -p dbname < backup.sql
    mysql -uroot -p < all.sql

##数据库迁移

    #相同版本的 MySQL 数据库乊间的迁移
    mysqldump –h host1 –u root –password=password1 –all-databases | mysql –h host2 –u root –password=password2
    mysqldump –h host1 –u root –ppassword databasename | mysql –h host2 –u root –ppassword databasename

##表的导出和导入

    #用 SELECT...INTO OUTFILE 导出文本文件
    SELECT [列名] FROM table [WHERE 诧句]
    INTO OUTFILE ‘目标文件’ [OPTION]

    #用mysqldump命令导出文本文件
    mysqldump –u root –pPassword –T 目标目录或文件 dbname table [option];

    #用mysql命令导出文本文件
    mysql –u root –pPassword –e “sql”dbname > c:/sql.txt
    mysql –u root –pPassword --xml | -X -e “sql”dbname > c:/sql.txt
    mysql –u root –pPassword --html | -H -e “sql”dbname > c:/sql.txt

    #用LOAD DATA INFILE方式导入文本文件
    LOAD DATA[LOCAL] INFILE file INTO TABLE table [OPTION]
    LOAD DATA INFILE C:/student.txt INTO TABLE student [OPTION]

    #用mysqlimport命令导入文本文件
    mysqlimport –u root –pPassword [--LOCAL] dbname file [OPTION]

##MySQL 日志
包括二进制日志，错误日志，通用查询日志，慢查询日志。

##Mac系统

除了在控制面板中开启和或关闭mysql，还可以使用终端命令行来控制启动，停止和重启：

sudo /Library/StartupItems/MySQLCOM/MySQLCOM [start|stop|restart]

