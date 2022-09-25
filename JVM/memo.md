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
```
![image](https://user-images.githubusercontent.com/54012569/192170329-868fb993-d1db-4ed3-8feb-e8244fd5efbb.png)

