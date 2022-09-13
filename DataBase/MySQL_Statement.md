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

