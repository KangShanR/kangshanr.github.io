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
