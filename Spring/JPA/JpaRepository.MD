## deep clone
![image](https://user-images.githubusercontent.com/54012569/160037649-677b7fa9-7620-48df-9d1f-892aebe7e630.png) <br>
对于数据类型是引用数据类型的成员变量，比如说成员变量是某个数组、某个类的对象等，那么浅拷贝会将该成员变量的引用值（内存地址）复制一份给新的对象。因为实际上两个对象的该成员变量都指向同一个实例。在这种情况下，在一个对象中修改该成员变量会影响到另一个对象的该成员变量值。

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
```java
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
## 多表关联的Entity的save and delete
```java
// PaymentSourceEntity with @OneToOne for PaymentSourceVerificationLogEntity
// 对应 PaymentSourceRepository.SAVE(PaymentSourceEntity) 会报错，原因是PaymentSourceEntity对象的save没有commit，PaymentSourceVerificationLogEntity无法save
// 解决办法：PaymentSourceEntity对象的PaymentSourceVerificationLogEntity set to null，save PaymentSourceEntity对象 后，将其关联属性赋值给PaymentSourceVerificationLogEntity对象；再通过代码实现save对应的PaymentSourceVerificationLogEntity的对象。

// delete
// PaymentSourceRepository.DELETE(PaymentSourceEntity) 不会删除关联的PaymentSourceVerificationLogEntity记录，需要通过代码先删关联的PaymentSourceVerificationLogEntity记录，再删PaymentSourceEntity
```

    

