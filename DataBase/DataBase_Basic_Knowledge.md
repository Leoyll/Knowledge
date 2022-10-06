# /DataBase/DataBase_Basic_Knowledge

## JDBC (Java DataBase Connectivity)
* Java和数据库（关系型数据库）之间的一个桥梁
* 一个规范而不是一个实现，能够执行SQL语句
* 不属于某一个数据库的接口，而是可以用于定义程序与数据库连接规范，通过一整套接口，由各个不同的数据库厂商去完成所对应的实现类，由sun公司提出
* 执行sql过程为：类加载-->获取连接-->书写SQL-->执行语句--->处理结果集

## 连接池
* 因为建立数据库连接是一个非常耗时、耗资源的行为，所以通过连接池预先同数据库建立一些连接，放在内存中，应用程序需要建立数据库连接时直接到连接池中申请一个就行，用完后再放回去，极大的提高了数据库连接的性能问题，节省了资源和时间。

## 数据源
* JDBC2.0 提供了javax.sql.DataSource接口，它负责建立与数据库的连接，当在应用程序中访问数据库时 不必编写连接数据库的代码，直接引用DataSource获取数据库的连接对象即可。用于获取操作数据Connection对象。

## 数据源与数据库连接池组件
* 数据源建立多个数据库连接，这些数据库连接会保存在数据库连接池中，当需要访问数据库时，只需要从数据库连接池中
* 获取空闲的数据库连接，当程序访问数据库结束时，数据库连接会放回数据库连接池中。

## 常用的数据库连接池技术
* C3P0、DBCP、Proxool和DruidX
    * 这些连接技术都是在jdbc的规范之上建立完成的

## Druid
官方网站文档：https://github.com/alibaba/druid/wiki/Druid%E8%BF%9E%E6%8E%A5%E6%B1%A0%E4%BB%8B%E7%BB%8D

网文：
https://www.cnblogs.com/knowledgesea/p/11202918.html

* 使用Druid实现对MSSQL数据库进行增删查
    1. 引入druid依赖
    2. 引入com.microsoft.sqlserver.sqldjbc4依赖（由此依赖可以看出JDBC与druid的关系，druid是基于jdbc规范建立的上层应用）
    3. 写代码
    4. 配置druid的datasource
    5. 建立Connection
    6. 创建Statement或者PreparedStatement接口执行SQL
    7. 处理结果
    8. 释放资源

**note**：上面有个小插曲就是根据druid生成密码，命令：
```java=
D:\Maven\repository\com\alibaba\druid\1.1.5> java -cp .\druid-1.1.5.jar  com.alibaba.druid.filter.config.ConfigTools 密码
# If there is `&` in the password, there will be something wrong when using the encrypted pwd.

Output:
    privateKey: xxx
    publicKey: xxx
    password: xxx
```
之后版本 
```java=
 public static void main(String[] args) throws Exception {
        String[] strs = new String[]{"Tuhumima123%]"};
        com.alibaba.druid.filter.config.ConfigTools.main(strs);
    }　
```
解密
```java=
// Maven: com.alibaba:druid:1.0.26
com.alibaba.druid.filter.config.ConfigTools public static String decrypt(String publicKeyText, String cipherText)
```

## Connect to Database from Linux Server
```sql
mysql -h hostAddress -u userName -p password
```
