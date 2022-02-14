## Class path contains multiple SLF4J bindings
```
$ mvn dependency:tree
```
![image](https://user-images.githubusercontent.com/54012569/153834278-e7c49dd8-75fd-4aae-8912-b1bf00cdc2d9.png)
![image](https://user-images.githubusercontent.com/54012569/153834388-760e9139-7a91-4ed0-a64c-eafb5387569f.png)

```java
<dependency>
    <groupId>org.liquibase</groupId>
    <artifactId>liquibase-core</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```
