## JPA EntityManager
四个主要方法 ——persist,merge,refresh和remove

实体的生命周期：新实体（new），持久化态（managed），游离态（detached），删除状态（removed）
EntityManger控制实体在四个状态之间转换 <br>
![image](https://user-images.githubusercontent.com/54012569/148463832-4930db8e-27fc-418d-b090-753f46e31d86.png)
![image](https://user-images.githubusercontent.com/54012569/148463889-76030692-329e-4549-b9e7-84d2bd5eb6c0.png)

## JPA SAVE vs saveAndFlush
?? 在大多数情况下，我们将使用save()方法。但是有时，对于特定的用例，我们可能还需要使用saveAndFlush()方法 <br>
```java
// org.springframework.data.jpa.repository.JpaRepository<T, ID extends java.io.Serializable> public abstract <S extends T> java.util.List<S> save(Iterable<S> entities)
JpaRepository.save(list) 
// isNew ? persist : merge
// insert/save : update
// ??save方法会预检查该entity是否持久化，isNew会判断该对象的Id类型 是否实现Persistable或EntityInformation进行重写isNew方法，如果Id是Number类型，直接判断value==0 true 执行entityManager.persist 否则执行entityManager.merge()
```
## 
···java
package org.springframework.data.domain;

// public PageRequest(int page, int size, org.springframework.data.domain.Sort sort)
Pageable pageRequest = new PageRequest(0, INIT_BATCH_SIZE, new Sort("id"));
Page<xxxEntity> xxxEntityPage = xxxRepository.findAllxxx(pageRequest, ...);     // (1)
while (xxxEntityPage.hasContent()) {
    xxx;                                  //(3)
    paymentSourceRepository.flush();
    depositRequest = pageRequest.next();  //(1)和(2)查询结果不变时，才可使用该语句输出下个page的结果；如果(3)改变了数据库内容 并使(1)(2)查询结果有变化，则需重新考虑
              //比如(1)和(2)查询status为0的数据，(3)将本次查询到记录的status设为1，此时的(2)跟之前的(2)的结果是有变化的，则不能再depositRequest=pageRequest.next()
    xxxEntityPage = xxxRepository.findAllxxx(pageRequest, ...);                 // (2)
}

```
