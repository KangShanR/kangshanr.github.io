---
title: Synchronization
categories: Java
tags: [Java, Synchronization, CAS, Concurrent]
---

> Java 同步机制

## CAS

> CAS 机制借助处理器 CAS 指令可以相对锁指令节省不少开销,实现无锁并发. Compare and Swap

### Atomic

#### AtomicLong 与 LongAdder

- 在修改量小的时候 LongAdder 并未有什么优势,但当修改量足够大时 LongAdder 有更高的吞吐量,同时也有更高的空间消耗.

### ABA 问题

针对 ABA 问题,Java 提供了 AtomicStampedReference 工具类.

## JMM

## 6 个指令

1. read 将主存变量读取到线程工作内存
2. load 将线程工作内存的变量拷贝到副本以便执行引擎使用
3. use 执行引擎使用变量,当需要执行的字节码指令需要使用变量时使用
4. assign 将执行引擎操作后的变量赋值给工作内存变量
5. store 将工作内存变量指定到主存变量,以便随后的 write
6. write 将store工作内存变量传到主存变量中

### JMM 解决的问题

[reference]](https://zhuanlan.zhihu.com/p/29881777)

1. 多线程读同步与共享对象可见性
   1. 线程缓存导致可见性问题:一个线程读取到变量后修改,并未将修改后变量flush到主存,其他线程并不可见
      1. 解决方案: volatile 保证变量读取前从主存刷新,修改后将新值同步到主存
      3. synchronized 同步块可见性保证:lock 操作必须清空工作内存,执行引擎使用前必须执行 load 或 assign, unlock 前必须执行 store write 将变量写入主存.
      4. final 变量在构建期间没有 this 引用逃逸,构建完成后对其他线程可见
   2. 重排序导致可见性问题:编译器/指令并行/内存系统在不改变单线程主义的前提下,重新安排语句执行顺序.
      1. volatile 本身包含禁止重排序语义
      2. synchronized 一个变量在被 lock 前只允许一条线程对其进行lock 操作-->持有同一个锁的两个同步块只能串行进入
2. happens-before 关系
   1. 在 JMM 中如果一个操作要对另一个操作可见,那么这两个操作必须要存在 happens-before 关系:
      1. 程序顺序:一个线程中每个操作,happens-before 任意后续操作
      2. 监听锁:对于一个锁的解锁 happens-before 随后对这个锁的加锁
      3. volatile: 对于一个 volatile 域的写,happens-before 于任意后续
      4. 传递性: a happens-before b, b -> c, a -> c
3. 原子性:JMM 保证上面6个指令都是原子性
   1. 大致可认为 基本数据类型变量/引用类型变量/声明为 volatile 任何类型变量的访问读写是具备原子性(64位数据long double JMM 允许jvm 划分为再次32 bit 操作进行,即允许jvm不保证64bit数据的 load store read write 四个操作的原子性.但目前各平台下商用jvm都将64bit数值读写操作作为原子操作来对待,所以也不用专门对 long double 变量 添加  volatile 修辞).这些类型变量读写天然具有原子性,但 `volatile++` `基本变量++` 非原子性.
   2. 更大场景需要原子性时就需要 使用 synchronized 同步块来保证. lock unlock 指令在字节码指令为 `monitorenter` `monitorexit`.
