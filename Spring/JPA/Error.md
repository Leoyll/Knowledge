## No found for dependency: expected at least 1 bean which qualifies as autowire candidate for this dependency
Normally, we use @Repository to define the interface xxxRepository and the bean can be created.
However, the package name is written wrongly with "repositoty" other than "repository" and face the bean creation error.
```java

```

## findAll(Unknown Source)
Root cause: the type of the new Field of the XXXEntity does NOT match with the data in DB. <br>
For example, define Date type in XXXEntity and String in DB, which is a string in fact.

```java
import org.springframework.data.domain.Example;
import org.springframework.data.jpa.repository.JpaRepository;

jpaRepository.findAll(example);

2022-02-18 22:00:00.134 [pool-13-thread-1] ERROR com.ty.migration.MigrationService - org.hibernate.exception.GenericJDBCException: could not execute query; nested exc
eption is javax.persistence.PersistenceException: org.hibernate.exception.GenericJDBCException: could not execute query
org.springframework.orm.jpa.JpaSystemException: org.hibernate.exception.GenericJDBCException: could not execute query; nested exception is javax.persistence.PersistenceException: org.hibernate.exception.GenericJDBCException: could not execute query
        at org.springframework.orm.jpa.EntityManagerFactoryUtils.convertJpaAccessExceptionIfPossible(EntityManagerFactoryUtils.java:424) ~[spring-orm-4.3.25.RELEASE.jar:4.3.25.RELEASE]
        at org.springframework.orm.jpa.AbstractEntityManagerFactoryBean.translateExceptionIfPossible(AbstractEntityManagerFactoryBean.java:526) ~[spring-orm-4.3.25.RELEASE.jar:4.3.25.RELEASE]
        at org.springframework.dao.support.ChainedPersistenceExceptionTranslator.translateExceptionIfPossible(ChainedPersistenceExceptionTranslator.java:59) ~[spring-tx-4.3.25.RELEASE.jar:4.3.25.RELEASE]
        at org.springframework.dao.support.DataAccessUtils.translateIfNecessary(DataAccessUtils.java:209) ~[spring-tx-4.3.25.RELEASE.jar:4.3.25.RELEASE]
        at org.springframework.dao.support.PersistenceExceptionTranslationInterceptor.invoke(PersistenceExceptionTranslationInterceptor.java:147) ~[spring-tx-4.3.25.RELEASE.jar:4.3.25.RELEASE]
        at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:179) ~[spring-aop-4.3.25.RELEASE.jar:4.3.25.RELEASE]
        at org.springframework.data.jpa.repository.support.CrudMethodMetadataPostProcessor$CrudMethodMetadataPopulatingMethodInterceptor.invoke(CrudMethodMetadataPostProcessor.java:133) ~[spring-data-jpa-1.10.2.RELEASE.jar:na]
        at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:179) ~[spring-aop-4.3.25.RELEASE.jar:4.3.25.RELEASE]
        at org.springframework.aop.interceptor.ExposeInvocationInterceptor.invoke(ExposeInvocationInterceptor.java:92) ~[spring-aop-4.3.25.RELEASE.jar:4.3.25.RELEASE]
        at org.springframework.aop.framework.ReflectiveMethodInvocation.proceed(ReflectiveMethodInvocation.java:179) ~[spring-aop-4.3.25.RELEASE.jar:4.3.25.RELEASE]
        at org.springframework.aop.framework.JdkDynamicAopProxy.invoke(JdkDynamicAopProxy.java:213) ~[spring-aop-4.3.25.RELEASE.jar:4.3.25.RELEASE]
        at com.sun.proxy.$Proxy269.findAll(Unknown Source) ~[na:na]
        at com.ty.migration.MigrationBaseService.cloneEntity(MigrationBaseService.java:62) ~[vntg-core-6.0.0.jar:na]
        at com.ty.migration.MigrationBaseService.migrateEntity(MigrationBaseService.java:44) ~[vntg-core-6.0.0.jar:na]
        at com.ty.migration.MigrationBaseService.migrateEntity(MigrationBaseService.java:49) ~[vntg-core-6.0.0.jar:na]
        at com.ty.migration.MigrationBaseService.migrateEntity(MigrationBaseService.java:57) ~[vntg-core-6.0.0.jar:na]
        at com.ty.migration.UserMigrationService.migrateUser(UserMigrationService.java:127) ~[vntg-core-6.0.0.jar:na]
        at com.ty.migration.UserMigrationService.migrate(UserMigrationService.java:118) ~[vntg-core-6.0.0.jar:na]
        at com.ty.migration.MigrationService.clone(MigrationService.java:330) ~[vntg-core-6.0.0.jar:na]
        at com.ty.migration.MigrationService.clone(MigrationService.java:314) ~[vntg-core-6.0.0.jar:na]
        at com.ty.migration.MigrationService.migrateByUserId(MigrationService.java:250) ~[vntg-core-6.0.0.jar:na]
        at com.ty.migration.MigrationService.lambda$migrateByLog$2(MigrationService.java:134) ~[vntg-core-6.0.0.jar:na]
        at java.base/java.util.concurrent.Executors$RunnableAdapter.call(Executors.java:515) ~[na:na]
        at java.base/java.util.concurrent.FutureTask.run(FutureTask.java:264) ~[na:na]
        at java.base/java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1130) ~[na:na]
        at java.base/java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:630) ~[na:na]
        at java.base/java.lang.Thread.run(Thread.java:832) ~[na:na]
Caused by: javax.persistence.PersistenceException: org.hibernate.exception.GenericJDBCException: could not execute query
        at org.hibernate.jpa.spi.AbstractEntityManagerImpl.convert(AbstractEntityManagerImpl.java:1692) ~[hibernate-entitymanager-5.0.9.Final.jar:5.0.9.Final]
        at org.hibernate.jpa.spi.AbstractEntityManagerImpl.convert(AbstractEntityManagerImpl.java:1602) ~[hibernate-entitymanager-5.0.9.Final.jar:5.0.9.Final]
        at org.hibernate.jpa.internal.QueryImpl.getResultList(QueryImpl.java:492) ~[hibernate-entitymanager-5.0.9.Final.jar:5.0.9.Final]
        at org.hibernate.jpa.criteria.compile.CriteriaQueryTypeQueryAdapter.getResultList(CriteriaQueryTypeQueryAdapter.java:50) ~[hibernate-entitymanager-5.0.9.Final.jar:5.0.9.Final]
        at org.springframework.data.jpa.repository.support.SimpleJpaRepository.findAll(SimpleJpaRepository.java:455) ~[spring-data-jpa-1.10.2.RELEASE.jar:na]
        at org.springframework.data.jpa.repository.support.SimpleJpaRepository.findAll(SimpleJpaRepository.java:72) ~[spring-data-jpa-1.10.2.RELEASE.jar:na]
        at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke0(Native Method) ~[na:na]
        at java.base/jdk.internal.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:64) ~[na:na]
        at java.base/jdk.internal.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43) ~[na:na]
        at java.base/java.lang.reflect.Method.invoke(Method.java:564) ~[na:na]
```
