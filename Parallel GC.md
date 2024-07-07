```java
java -XX:+UseParallelGC -jar Application.java
```

It’s the default _GC_ of the _JVM_ from Java 5 until Java 8 and is sometimes called Throughput Collectors. Unlike [[Serial GC]], it **uses multiple threads for managing heap space,** but it also freezes other application threads while performing _GC_.


If we use this _GC_, we can specify maximum garbage collection _threads and pause time, throughput, and footprint_ (heap size).

The numbers of garbage collector threads can be controlled with the command-line option 

```java
-XX:ParallelGCThreads=<N>
```

The maximum pause time goal (a hint to the garbage collector that pause time of N milliseconds or less are desired) is specified with the command-line option:

```java
-XX:MaxGCPauseMillis=<N>
```

The time spent doing garbage collection versus the time spent outside of garbage collection is called the maximum throughput target and can be specified by the command-line option 

```java
-XX:GCTimeRatio=<N>
```

The maximum heap footprint (the amount of heap memory that a program requires while running) is specified using the option 

```java
-Xmx<N>
```