# MySQL_knowledge

## Trace SQL statement

```sql
# enable trace
set session optimizer_trace=”enabled=on”, end_markers_in_json=on;
# SQL statement
```

## index

### show index

```sql
show indexes from tb_example;
```

### add index

```sql
alter table tb_example add index index_userId(user_id);
```

### drop index

```sql
alter table tb_example drop index index_userId;
```

## Special Character

https://dev.mysql.com/doc/refman/8.0/en/string-literals.html <br>
![image](https://user-images.githubusercontent.com/54012569/157643912-ffd5164d-1773-474e-9c79-dbf02718f428.png)
![image](https://user-images.githubusercontent.com/54012569/157644044-478b8275-b83f-41ab-a1fe-3089dc128b33.png)
![image](https://user-images.githubusercontent.com/54012569/157644108-03876c65-6b1e-4404-803c-275805505abb.png)

### Excape special character

```sql
xxx where xxx like %\_SubStr\_%;    # filter the string which contains "_SubStr_"
```

## wildcard

```sql
SELECT * FROM members WHERE postal_address like '%code';
SELECT * FROM members WHERE postal_address LIKE 'code%';
SELECT * FROM members WHERE postal_address LIKE '%code%';
SELECT * FROM members WHERE postal_address LIKE 'code_';
SELECT * FROM members WHERE postal_address NOT LIKE 'code_';
```

### Escape keyword

```sql
SELECT * FROM members WHERE postal_address LIKE '67#%%' ESCAPE '#';     #check for the string “67%”
SELECT * FROM members WHERE postal_address LIKE '67#%%' ESCAPE '#';     #search for the movie “67% Guilty”
SELECT * FROM members WHERE postal_address LIKE '67=%%' ESCAPE '=';     #search for the movie “67% Guilty”
```

### _ underscore wildcard (for specific length)

The underscore wildcard is used to match exactly one character. Two matches exactly two characters, and so on. <br>
Let’s suppose that we want to search for all the movies that were released in the years 200x where x is exactly one character that could be any value. We would use the underscore wild card to achieve that. The script below select all the movies that were released in the year “200x”.<br>
![image](https://user-images.githubusercontent.com/54012569/157644583-8991cbf0-5410-461e-a694-4f1785f28604.png)

## set

```sql
SET @var1 = 1;
SET @var4 = (select menuid from tb_example_table3 where menuname = 'test');
select @var2 := id, @var3 := name from tb_example_table where age = 33;

INSERT INTO tb_example_table2 (id, name, varSet, menuid)
VALUES (@var2, @var3, @var1, @var4);
```

## replace

```sql
update tb_example
set  column1 = replace(@originalStr, @oldSubStr, @newSubStr)
where xxx;
#replace("a,b1,c2,b3", "b", "d") -> "a,d1,c2,d3"
```

## CONCAT

### concat multi-string with delimiter

```sql
update tb_example
set  column1 = CONCAT(@originalStr, @delimiter, @addSubStr)
where xxx;
#CONCAT("abc", ";", "def") -> "abc;def"
```

### concat select rows

```sql
SELECT person_id, GROUP_CONCAT(hobbies SEPARATOR ', ')
FROM peoples_hobbies
GROUP BY person_id;
```

## SHOW

```sql
SHOW CREATE VIEW viewName;
DESC tableName/viewName;
show create table tableName;

SHOW TABLES;
SHOW VIEWS; #not support in MYSQL
```

Examples:
DESC tableName/viewName;
![image](https://user-images.githubusercontent.com/54012569/190027772-c27024cc-ccd1-4d7d-9671-bdde3bba6c70.png)
show create table tableName;
![image](https://user-images.githubusercontent.com/54012569/190027888-78ff40e1-6507-47d1-b062-cece8ff9f078.png)

## SELECT - bit type column

```sql
select bin(is_del) from tableName;
```

![image](https://user-images.githubusercontent.com/54012569/190028177-a5ee32d6-2e9a-4114-860a-146ff5807426.png)
![image](https://user-images.githubusercontent.com/54012569/190028332-49fc8619-d1b5-4580-a837-48c88243637f.png)

## Create

### create View

```sql
  create or replace view view_name as
  select * from `db_name`.`table_name`;
```

The view definition is “**frozen**” at creation time and is not affected by subsequent changes to the definitions of the underlying tables. <br>
For example, if a view is defined as SELECT * on a table, new columns added to the table later do not become part of the view,
and columns dropped from the table result in an error when selecting from the view.

## Query Optimization

https://juejin.cn/post/6992173068416188423

### create suitable index

避免全表扫描，首先应考虑在 where 及 order by ，group by 涉及的列上建立索引

### 优化SQL语句

* 分析查询语句：通过对查询语句的分析，可以了解查询语句执行情况，找出查询语句执行的瓶颈，从而优化查询语句。
  通过explain（查询优化神器）用来查看SQL语句的执行结果，可以帮助选择更好的索引和优化查询语句，写出更好的优化语句。
  例如：explain select * from news;
* 任何地方都不要使用select * from t ，用具体的字段列表代替“*”，不要返回用不到的任何字段。
* 不在索引列做运算或者使用函数。
* 查询尽可能使用 limit 减少返回的行数，**减少数据传输时间和带宽浪费**。

### 优化数据库对象

1. 优化表的数据类型
   1. 使用 procedure analyse()函数对表进行分析，该函数可以对表中列的数据类型提出优化建议。表数据类型第一个原则是：使用能正确地表示和存储数据的最短类型。这样可以减少对磁盘空间、内存、CPU缓存的使用。　使用方法：select * from 表名 procedure analyse();
2. 对表进行拆分
   1. 垂直拆分（按照功能模块）
      1. 将表按照功能模块、关系密切程度划分出来，部署到不同的库上。例如：我们会建立定义数据库workDB、商品数据库payDB、用户数据库userDB等，分别用于存储项目数据定义表、商品定义表、用户数据表等。
      2. 把主键和一些列放在一个表中，然后把主键和另外的列放在零一个表中。如果一个表中某些列常用，而另外一个些不常用，则可以采用垂直拆分。
   2. 水平拆分（按照规则划分存储**）**
      1. 当一个表中的数据量过大时，我们可以把该表的数据按照某种规则进行划分，例如userID散列，然后存储到多个结构相同的表和不同的库中。
      2. 根据一列或者多列数据的值吧数据行放到两个独立的表中。
3. 使用中间表来提高查询速度
   1. 创建中间表，表结构和源表结构完全相同，转移要统计的数据到中间表，然后在中间表上进行统计，得出想要的结果。

### MySQL自身的优化

　　对MySQL自身的优化主要是对其配置文件my.cnf中的各项参数进行优化调整。如指定MySQL查询缓冲区的大小，指定MySQL允许的最大连接进程数等。

### 应用优化

　1）使用数据库连接池
　2）实用查询缓存
　　它的作用是存储 select 查询的文本及其相应结果。如果随后收到一个相同的查询，服务器会从查询缓存中直接得到查询结果。查询缓存适用的对象是更新不频繁的表，当表中数据更改后，查询缓存中的相关条目就会被清空。
