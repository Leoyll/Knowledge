# MySQL_knowledge

## index
### show index
```
show indexes from tb_example;
```
### add index
```
alter table tb_example add index index_userId(user_id);
```


### drop index
```
alter table tb_example drop index index_userId;
```

## set
```
SET @var1 = 1;
SET @var4 = (select menuid from tb_example_table3 where menuname = 'test');
select @var2 := id, @var3 := name from tb_example_table where age = 33;

INSERT INTO tb_example_table2 (id, name, varSet, menuid)
VALUES (@var2, @var3, @var1, @var4);
```
