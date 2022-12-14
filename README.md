### 题目 01- 请你说一说什么是线程和进程？
### 区别
### 关系
 一个进程就是一个在內存中执行的应用程序，是系统运行程序的基本单位，从main方法开始到结束只有一个流程。线程是进程中的一个执行单元，一个进程可以产生多个线程。区别是每个进程都有独立的內存空间，而线程之间分享堆空间和方法区，但是线程栈空间和程序计数器是独立的。创建进程比创建线程消耗更多资源。
 
### 线程的上下文切换是什么？
线程的上下文切换就是将一个CPU分成时间片轮流让多个线程执行使用，每个时间分片只能让一个线程使用，时间到了即使还没有执行完，也要切换到下一个线程使用，以达到同一个时间段內执行多个任务的效果。在切换的过程中把当前线程的执行位置记录在程序计数器，以便下次切换回来时候找到对的位置加载回来。

### 线程的并发与并行有啥区别？
并发Concurrency： 同一个时间段里有多个任务在执行，但是单位时间內不一定是同时执行。
并行Parallel：单位时间內多个任务同时执行。

### 题目 02- 使用了多线程会带来什么问题呢？
使用了多线程会带来线程安全问题。

### 能不能详细说说线程安全问题
当多个线程执行同一个代码的共享变量的读写操作，而产生冲突导致不符合预期的结果，就会引发线程安全问题。

### 原子性、有序性和可见性能不能深入的谈一下
原子性是一系列的事务操作过程不能被打断，要么全执行，要么全不执行。
有序性是程序代码即使经过指令重排之后也要按照先后顺序执行。
可见性是当一个共享变量被一个线程修改了值，其他线程也能马上看到

### 题目 03- 什么是死锁？如何排查死锁? 
### 排查过程最好详细说明，最少说一种排查方案，越多越好 
死锁是两个线程互相等待获取对方的锁，用jstack 《pid》| grep 'BLOCKED' ，用arthas 然后thread -b

### 题目 04- 请你说一说 synchronized 和 volatile 的原理与区别
Synchronized保证代码块在多线程环境运行时，同一时间只有一个线程执行代码块。Synchronized 有用到锁。
用Volatile去声明了一个变量，JMM确保所有线程看到这个变量的值是一致的，保证了多线程场景下共享变量的可见性和有序性。Volatile没有用到锁。

### 什么是 JMM 内存模型？
JMM是对内存和缓存的抽象规范，屏蔽了线程对硬件的直接操作，同时也导致可见性问题。

### 什么是 happens-before 规则？
happens-before 有以下规则：
1是程序顺序规则，一个线程中的每个操作都要按照顺序发生才能有接下来后续操作， 上一个操作的结果要对下一个操作可见。
2是锁规则，对一个锁的解锁发生了才能有随后对这个锁的加锁，锁之前的结果要对锁之后可见。
3是Volatile变量规则，对一个Volatile变量的写操作发生后才能对这个变量的读操作，Volatile的写操作的结果对读操作可见。
4是传递性，如果A发生在B之前，B发生在C之前，那么A就是发生在C之前。

### 题目 05- 为什么使用线程池？如何创建线程池？
线程池是预先创建多个线程，便于管理分配并发任务和复用线程，提高响应速度和提供可扩展性，降低系统开销，因为频繁创建线程与销毀线程的开销很大。

### 手动创建和自动创建线程池都需要介绍 
### 最好介绍一下线程池的实现原理 
自动创建线程池，1 newFixedThreadPool 固定数量线程池，无界任务阻塞队列。2 newSingleThreadExecutor 一个线程的线程池，无界任务阻塞队列。3 newCachedThreadPool 可缓存线程的无界线程池，可以自动回收多余线程。4 newScheduleThreadPool 定时任务线程池。手动创建线程池，使用ThreadPoolExecutor创建，要考虑线程池大小，和拒绝策略。

### 题目 06-ThreadLocal 中 Map 的 key 为什么要使用弱引用？
### 阿里 Java 开发手册中，为什么说不清理自定义的 ThreadLocal 变量会导致内存泄露呢？
ThreadLocal中Map的key要使用弱引用以便GC可以把它回收，如果使用了强引用GC就无法把它回收，对象多了就会內存溢出。ThreadLocal中Map的value只能用强引用，所以必须在代码finally中清理remove变成null，否则会导致內存泄露，就如阿里Java开发手册中所提到的。
