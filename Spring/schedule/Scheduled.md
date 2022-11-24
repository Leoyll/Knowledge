
## 定时任务
```java
import org.springframework.scheduling.annotation.Scheduled;

public class Test{
/*  
    fixedDelay:控制方法执行的间隔时间，是以上一次方法执行完开始算起，如上一次方法执行阻塞住了，那么直到上一次执行完，并间隔给定的时间后，执行下一次。
    cron表达式:可以定制化执行任务，但是执行的方式是与fixedDelay相近的，也是会按照上一次方法结束时间开始算起。
    fixedRate:是按照一定的速率执行，是从上一次方法执行开始的时间算起，如果上一次方法阻塞住了，下一次也是不会执行，但是在阻塞这段时间内累计应该执行的次数，当不再阻塞时，一下子把这些全部执行掉，而后再按照固定速率继续执行。
    initialDelay: 在容器启动后,延迟30秒后再执行一次定时器，以后每1800秒再执行一次定时器
**/
@Scheduled(fixedDelay = 1800000, initialDelay = 30000)
@Scheduled(cron = "${statistics.ams.day-cron}")
private void scheduledTaskTest() { }
}
```
```java
import org.springframework.scheduling.annotation.EnableScheduling;
@EnableScheduling
public class Application {}

```

### 改成多线程异步方式
```java
//  **fixedDelay、fixedRate不能用@Async注解**

//  使用类或者启动类添加@EnableAsync注解
@EnableAsync
public class Application {
    public static void main(String[] args) {}
}

//  方发上添加@Async注解
@Scheduled(cron = "${statistics.ams.day-cron}")
@Async
private void scheduledTaskTest() { }
/*
    cronExpression定义时间规则，Cron表达式由6或7个空格分隔的时间字段组成：秒 分钟 小时 日期 月份 星期 年（可选）
    ?字符只在日期域和星期域中使用。它被用来指定“非明确的值”:月份中的日期和星期中的日期这两个元素时互斥的一起应该通过设置一个问号来表明不想设置那个字段
    “10-12”在小时域意味着“10点、11点、12点”
    “MON,WED,FRI”在星期域里表示”星期一、星期三、星期五”
    “0/15”在秒域意思是每分钟的0，15，30和45秒;如：/10 等价于0在“/”前面，即 0/10
    L是‘last’的省略写法可以表示day-of-month和day-of-week域，但在两个字段中的意思不同，例如day-of- month域中表示一个月的最后一天。如果在day-of-week域表示‘7’或者‘SAT’，如果在day-of-week域中前面加上数字，它表示 一个月的最后几天，例如‘6L’就表示一个月的最后一个星期五
    字符“W”只允许日期域出现。这个字符用于指定日期的最近工作日。例如：如果你在日期域中写 “15W”，表示：这个月15号最近的工作日。所以，如果15号是周六，则任务会在14号触发。如果15好是周日，则任务会在周一也就是16号触发。如果 是在日期域填写“1W”即使1号是周六，那么任务也只会在下周一，也就是3号触发，“W”字符指定的最近工作日是不能够跨月份的。字符“W”只能配合一个 单独的数值使用，不能够是一个数字段，如：1-15W是错误的。
    “L”和“W”可以在日期域中联合使用，LW表示这个月最后一周的工作日。
    字符“#”只允许在星期域中出现。这个字符用于指定本月的某某天。例如：“6#3”表示本月第三周的星期五（6表示星期五，3表示第三周）。“2#1”表示本月第一周的星期一。“4#5”表示第五周的星期三。
    字符“C”允许在日期域和星期域出现。这个字符依靠一个指定的“日历”。也就是说这个表达式的值依赖于相关的“日历”的计算结果，如果没有“日历” 关联，则等价于所有包含的“日历”。如：日期域是“5C”表示关联“日历”中第一天，或者这个月开始的第一天的后5天。星期域是“1C”表示关联“日历” 中第一天，或者星期的第一天的后1天，也就是周日的后一天（周一）。

 */
```