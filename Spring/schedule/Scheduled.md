```java
import org.springframework.scheduling.annotation.Scheduled;

/*  
    fixedDelay控制方法执行的间隔时间，是以上一次方法执行完开始算起，如上一次方法执行阻塞住了，那么直到上一次执行完，并间隔给定的时间后，执行下一次。
    cron表达式可以定制化执行任务，但是执行的方式是与fixedDelay相近的，也是会按照上一次方法结束时间开始算起。
    fixedRate是按照一定的速率执行，是从上一次方法执行开始的时间算起，如果上一次方法阻塞住了，下一次也是不会执行，但是在阻塞这段时间内累计应该执行的次数，当不再阻塞时，一下子把这些全部执行掉，而后再按照固定速率继续执行。
    initialDelay: 在容器启动后,延迟30秒后再执行一次定时器，以后每1800秒再执行一次定时器
**/
@Scheduled(fixedDelay = 1800000, initialDelay = 30000)
@Scheduled(cron = "${statistics.ams.day-cron}")


import org.springframework.scheduling.annotation.EnableScheduling;
@EnableScheduling
public class SpringbootStart {...}

```