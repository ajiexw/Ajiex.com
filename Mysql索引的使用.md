Tags: Mysql

Mysql中所有数据类型都可以被索引。

分类：普通索引，唯一性索引，全文索引，单列索引，多列索引，空间索引。

优点：提高查询、分级和排序的时间。

缺点：占空间，创建、更新记录时额外耗费时间。

使用原则:
+ 选择唯一性索引
+ 为经常需要排序、分组、联合操作的字段建立索引，如ORDER BY,GROUP BY,DISTINCT,UNION等操作的字段
+ 为经常作为查询条件的字段建立索引（WHERE）
+ 限制索引数目，避免过多的浪费空间
+ 尽量使用数据量少的索引
+ 删除不再使用或很少使用的索引

创建索引（3种方式）：

- 创建表的时候创建索引
- 在已经存在的表上创建索引
- 使用ALTER TABLE语句来创建索引

    #创建表的时候创建索引
    create table 表名(
        属性名 数据类型 [完整性约束],
        属性名 数据类型 [完整性约束],
        ...
        [UNIQUE | FULLTEXT | SPATIAL INDEX | KEY [别名] （属性名1[(长度)] [ASC | SESC])]
    );


    #1、创建普通索引
    CREATE TABLE index1 (
        id INT,
        name VARCHAR(20),
        sex BOOLEAN,
        INDEX(id)
    );
    SHOW CREATE TABLE index1\G;


    2、创建唯一性索引
    CREATE TABLE index2(
        id INT UNIQUE,
        name VARCHAR(20),
        UNIQUE INDEX index2_id(id ASC)
    );
    SHOW CREATE TABLE index2\G;
    
    3、创建全文索引
    CREATE TABLE index3 (
        id INT,
        info VARCHAR(20),
        FULLTEXT INDEX index3_info(info)
    ) ENGINE=MyISAM;


    4、创建单列索引
    CREATE TABLE index4 (
        id INT,
        subject VARCHAR(30),
        INDEX index4_st(subject(10))
    );
    注意:叧索引 subject 前 10 个字符

    5、创建多列索引
    CREATE TABLE index5 (
        id INT,
        name VARCHAR(20),
        sex CHAR(4),
        INDEX index5_ns(name, sex)
    );
    EXPLAIN select * from index5 where name=’123’\G;
    EXPLAIN select * from index5 where name=’123’and sex=’N’\G;

    6、创建空间索引
    CREATE TABLE index6 (
        id INT,
        Space GEOMETRY NOT NULL,
        SPATIAL INDEX index6_sp(space)
    )ENGINE=MyISAM;



    #在已经存在的表上创建索引
    CREATE [UNIQUE|FULLTEXT|SPATIAL] INDEX 索引名 ON 表名 (属性名[(长度)] [ASC|DESC]);
    1、创建普通索引
    CREATE INDEX index7_id on example0(id);
    2、创建唯一性索引
    CREATE UNIQUE INDEX index_8_id ON index8(course_id);
    3、创建全文索引
    CREATE FULLTEXT INDEX index9_info ON index9(info);
    4、创建单列索引
    CREATE INDEX index10_addr ON index10(address(4));
    5、创建多列索引
    CREATE INDEX index11_na ON index11(name, address);
    6、创建空间索引
    CREATE SPATIAL INDEX index12_line on index12(line);




    #用 ALTER TABLE 语句来创建索引
    ALTER TABLE 表名 ADD [UNIQUE|FULLTEXT|SPATIAL] INDEX 索引名 (属性名[(长度)] [ASC|DESC]);
    1、创建普通索引
    ALTER TABLE example0 ADD INDEX index12_name(name(20));
    2、创建唯一性索引
    ALTER TABLE index14 ADD UNIQUE INDEX index14_id(course_id);
    3、创建全文索引
    ALTER TABLE index15 ADD INDEX index15_info(info);
    4、创建单列索引
    ALTER TABLE index 16 ADD INDEX index16_addr(address(4));
    5、创建多列索引
    ALTER TABLE index17 ADD INDEX index17_na(name, address);
    6、创建空间索引
    ALTER TABLE index18 ADD INDEX index18_line(line);


    #删除索引
    DROP INDEX 索引名 ON 表名;


    #禁用索引
    ALTER TABLE table DISABLE/ENABLE KEYS;
    
    #禁用唯一索引
    SET UNIQUE_CHECK=0/1

