## JPA EntityManager
四个主要方法 ——persist,merge,refresh和remove

实体的生命周期：新实体（new），持久化态（managed），游离态（detached），删除状态（removed）
EntityManger控制实体在四个状态之间转换 <br>
![image](https://user-images.githubusercontent.com/54012569/148463832-4930db8e-27fc-418d-b090-753f46e31d86.png)
![image](https://user-images.githubusercontent.com/54012569/148463889-76030692-329e-4549-b9e7-84d2bd5eb6c0.png)

```java
// org.springframework.data.jpa.repository.JpaRepository<T, ID extends java.io.Serializable> public abstract <S extends T> java.util.List<S> save(Iterable<S> entities)
JpaRepository.save(list) 
// 含merge操作，即update也在save里
// save方法会预检查该entity是否持久化，isNew会判断该对象的Id类型 是否实现Persistable或EntityInformation进行重写isNew方法，如果Id是Number类型，直接判断value==0 true 执行entityManager.persist 否则执行entityManager.merge()??
```
