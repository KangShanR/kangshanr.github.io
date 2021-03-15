---
title: JVM-GC
tags: [JVM, GC]
date: 2021-03-15 23:23
---

## 判定对象存亡

### 引用计数法

### 可达性分析法

从 GC root 出发可达的对象.

GC root 包括:

- 栈帧中本地变量表中的对象
- 方法区类静态属性引用的对象
- 方法区常量引用的对象
- 本地方法栈中 JNI 引用的对象


### 引用类型

- 强引用 strong reference,直接的对象引用 GC 不可达
- 软引用 soft reference, 在 OOM 前的 GC 可达
- 弱引用 weak reference, 不影响 GC 可达
- 虚引用 photon reference, 不对 GC 有任何影响, 用于 GC 后的回调通知.

引用队列

## 回收算法

### 标记清除法

mark-sweeping

对已回收的内存进行标记,随后再清除.缺点:空间碎片过多,当大对象需要空间时可能导致 GC,而此时空间总利用率不一定很高.

### 复制

copying

将空间分成两部分,分配空间只在其中一部分进行直到空间不够使用时再将使用的紧凑复制到另一部分空闲空间.缺点:空间利用率只有一半.

在新生代上使用此算法可优化:新生代大部分对象生命周期很短,一个大空间 Edam区,两个较小 survivor 空间(默认8:1:1),当GC后把Edam区与使用的 Survivor 区中还存活的对象 copying 到原空间的 survivor 区中.这样可以保证空间利用率.当 survivor 空间不够时将使用老年代空间.

### 标记整理法

mark-compact

在标记后,将存活的空间紧凑地移动到空间尾部,以提高空间利用率.

### 分代收集

GC 收集器中,将堆分成新生代与老年代,不同的代使用不同的回收算法.新生代使用 Copying 算法,而老年代使用 Mark-compact/Mark-sweeping 算法.

## 垃圾收集器

### Serial

单线程收集器

工作时用户线程需要停止 stop the world

client 模式下使用

### ParNew

相对于 Serial 的优势在于是多线程

可以与第一代并发老年代收集器(CMS)联用.

### Parallel Scavenge

使用复制算法新生代收集器.

关注点与 CMS 不同在于:CMS 关注尽可能缩短用户线程在 GC 时停顿时间,而 Parallel Scavenge 关注吞吐量 throughput = 用户代码运行时间 / (用户代码运行时间 + 垃圾收集时间).


### Serial Old

### CMS

### G1