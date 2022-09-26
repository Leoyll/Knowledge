## JVM启动参数说明
```java
java -Xms750m -Xmx750m -Xmn512m -Xss1024k -XX:CMSFullGCsBeforeCompaction=5 -XX:+UseCMSCompactAtFullCollection -XX:+PrintGC -XX:+PrintGCDetails -XX:+PrintGCTimeStamps -Xloggc:/tmp/jvm.log -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath=/tmp/heapdump.hprof -Dfile.encoding=utf-8 -jar /data/app/test.jar

-Xms750表示堆内存初始值为750M
-Xmx750m表示堆内存最大值750M
-Xmn512m 设置年轻代大小为512m。整个JVM内存大小=年轻代大小 + 年老代大小 + 持久代大小。持久代一般固定大小为64m，所以增大年轻代后，将会减小年老代大小。此值对系统性能影响较大，Sun官方推荐配置为整个堆的3/8
-Xss1024k 设置每个线程的堆栈大小。JDK5.0以后每个线程堆栈大小为1M，以前每个线程堆栈大小为256K。更具应用的线程所需内存大小进行调整。在相同物理内存下，减小这个值能生成更多的线程。但是操作系统对一个进程内的线程数还是有限制的，不能无限生成，经验值在3000~5000左右。
-XX:CMSFullGCsBeforeCompaction=5 由于并发收集器不对内存空间进行压缩、整理，所以运行一段时间以后会产生“碎片”，使得运行效率降低。此参数设置运行次FullGC以后对内存空间进行压缩、整理。
-XX:+UseCMSCompactAtFullCollection 打开对年老代的压缩。可能会影响性能，但是可以消除内存碎片
-XX:+PrintGC 每次GC时打印相关信息
-XX:+PrintGCDetails 每次GC时打印详细信息
-XX:+PrintGCTimeStamps 打印每次GC的时间戳
-Xloggc:/tmp/jvm.log 设置垃圾回收日志打印的文件，文件名称可以自定义
-XX:+HeapDumpOnOutOfMemoryError 设置当首次遭遇内存溢出时导出此时堆中相关信息
-XX:HeapDumpPath=/tmp/heapdump.hprof 指定导出堆信息时的路径或文件名
```

## 获取JVM启动参数
```java
//  -DalphaName=local
System.getProperty("alphaName");

ManagementFactory.getRuntimeMXBean().getInputArguments();
//  https://codeantenna.com/a/5eD9jvAJAS
//==========================Memory=========================
MemoryMXBean memoryMBean = ManagementFactory.getMemoryMXBean();
MemoryUsage usage = memoryMBean.getHeapMemoryUsage();
System.out.println("初始化 Heap: " + (usage.getInit()/1024/1024) + "mb");
System.out.println("最大Heap: " + (usage.getMax()/1024/1024) + "mb");
System.out.println("已经使用Heap: " + (usage.getUsed()/1024/1024) + "mb");
System.out.println("Heap Memory Usage: " + memoryMBean.getHeapMemoryUsage());
System.out.println("Non-Heap Memory Usage: " + memoryMBean.getNonHeapMemoryUsage());

//==========================Runtime=========================
RuntimeMXBean runtimeMBean = ManagementFactory.getRuntimeMXBean();
System.out.println("JVM name : " + runtimeMBean.getVmName());
System.out.println("lib path : " + runtimeMBean.getLibraryPath());
System.out.println("class path : " + runtimeMBean.getClassPath());
System.out.println("getVmVersion() " + runtimeMBean.getVmVersion());

    //java options
    List argList = runtimeMBean.getInputArguments();
    for(String arg : argList){
        System.out.println("arg : " + arg);
    }

//==========================OperatingSystem=========================
OperatingSystemMXBean osMBean = (OperatingSystemMXBean) ManagementFactory.getOperatingSystemMXBean();
//获取操作系统相关信息
System.out.println("getName() "+ osMBean.getName());
System.out.println("getVersion() " + osMBean.getVersion());
System.out.println("getArch() "+osMBean.getArch());
System.out.println("getAvailableProcessors() " + osMBean.getAvailableProcessors());

//==========================Thread=========================
//获取各个线程的各种状态，CPU 占用情况，以及整个系统中的线程状况
ThreadMXBean threadMBean=(ThreadMXBean)ManagementFactory.getThreadMXBean();
System.out.println("getThreadCount() " + threadMBean.getThreadCount());
System.out.println("getPeakThreadCount() " + threadMBean.getPeakThreadCount());
System.out.println("getCurrentThreadCpuTime() " + threadMBean.getCurrentThreadCpuTime());
System.out.println("getDaemonThreadCount() " + threadMBean.getDaemonThreadCount());
System.out.println("getCurrentThreadUserTime() "+ threadMBean.getCurrentThreadUserTime());

//==========================Compilation=========================
CompilationMXBean compilMBean=(CompilationMXBean)ManagementFactory.getCompilationMXBean();
System.out.println("getName() " + compilMBean.getName());
System.out.println("getTotalCompilationTime() " + compilMBean.getTotalCompilationTime());

//==========================MemoryPool=========================
//获取多个内存池的使用情况
List mpMBeanList= ManagementFactory.getMemoryPoolMXBeans();
for(MemoryPoolMXBean mpMBean : mpMBeanList){
    System.out.println("getUsage() " + mpMBean.getUsage());
    System.out.println("getMemoryManagerNames() "+ mpMBean.getMemoryManagerNames().toString());
}

//==========================GarbageCollector=========================
//获取GC的次数以及花费时间之类的信息
List gcMBeanList=ManagementFactory.getGarbageCollectorMXBeans();
for(GarbageCollectorMXBean gcMBean : gcMBeanList){
    System.out.println("getName() " + gcMBean.getName());
    System.out.println("getMemoryPoolNames() "+ gcMBean.getMemoryPoolNames());
}

//==========================Other=========================
//Java 虚拟机中的内存总量,以字节为单位
int total = (int)Runtime.getRuntime().totalMemory()/1024/1024;
System.out.println("内存总量 ：" + total + "mb");
int free = (int)Runtime.getRuntime().freeMemory()/1024/1024;
System.out.println("空闲内存量 ： " + free + "mb");
int max = (int) (Runtime.getRuntime().maxMemory() /1024 / 1024);
System.out.println("最大内存量 ： " + max + "mb");
```
### Samples
![image](https://user-images.githubusercontent.com/54012569/192170329-868fb993-d1db-4ed3-8feb-e8244fd5efbb.png)


