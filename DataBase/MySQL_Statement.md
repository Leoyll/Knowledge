# MySQL_knowledge

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

## Excape Symbol
```sql
xxx where xxx like %\_SubStr\_%;    # filter the string which contains "_SubStr_"
```

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
SHOW TABLES;
SHOW VIEWS; #not support in MYSQL
```

