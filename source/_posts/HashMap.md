---
title: HashMap
layout: post
date: 2017-04-13 15:02:43
categories: [Java]
tags: [programming,java, HashMap]
description: hash function && hash table
---

Hash 算法应用在 Java 集合框架。其中 HashTable 基本实现数据结构的 HashTable 。HashMap 的底层有 HashTable 同时也有红黑树。HashSet 的内部就是一个HashMap 。<!--more-->

## Map

- map接口：是一种将键对象和值对象进行关联的容器，而且值对象可以是另一个Map，这样类推下去可以形成多级映射；
- Map中键对象不允许重复，且键的唯一性很重要；
- 不建议将多个键映射到同一个值对象；
- Map接口下有的实现集合类有：
    - HashMap，用到了哈希码的算法，以便快速查找一个键；
    - TreeMap，其键按序存放
    - HashTable，是 Dictionary 的子类，与 HashMap 类似；

## HashMap

[reference](https://www.jianshu.com/p/c658df4f4c77)

- HashMap 中，其 hash 算法是在引用 Object 的 hash 方法后再对进行了额外的位运算：

```java
/**
* Computes key.hashCode() and spreads (XORs) higher bits of hash
* to lower.  Because the table uses power-of-two masking, sets of
* hashes that vary only in bits above the current mask will
* always collide. (Among known examples are sets of Float keys
* holding consecutive whole numbers in small tables.)  So we
* apply a transform that spreads the impact of higher bits
* downward. There is a tradeoff between speed, utility, and
* quality of bit-spreading. Because many common sets of hashes
* are already reasonably distributed (so don't benefit from
* spreading), and because we use trees to handle large sets of
* collisions in bins, we just XOR some shifted bits in the
* cheapest possible way to reduce systematic lossage, as well as
* to incorporate impact of the highest bits that would otherwise
* never be used in index calculations because of table bounds.
*/
static final int hash(Object key) {
    int h;
    return (key == null) ? 0 : (h = key.hashCode()) ^ (h >>> 16);
}
```

- 如此做法：将高 16 位的 hashCode 与 hashCode 低 16 位(同时高16位与16位0进行XOR,>>> 逻辑右移)进行 XOR (异或) 位运算。这样能保证在不同对象的 hash 值在高位不同而低位相同的情况下出现冲突,降低 hash 值的位数。
- HashTable 中直接对 HashCode 按 capacity 取模，而 HashMap 中其 threshold 并不单指总 Entry 数量阈值，在其初始化时指下一次 resize() capacity 的目标值。
- HashTable 严格按时 HashTable 数据结构来定义，使用 Object hashCode() 计算其 hash 值，并按桶数量取模放置各个值入 HashTable 中。而在 HashMap 中，其对 Key 的 hash 值进入了高 16 位与低 16 位的或运算作为最终 hash 值，同时，在放置 Node 时，也并不是直接取模，使用 `hash & (capacity - 1)` 得到 bucket 下标。
    - 之所以能使用此种位运算获取到 bucket 下标，因为其桶数量 capacity 始终是 2 的次冥 （`10000` 的形式），`(capacity - 1)` 就会是 `1111` 的形式，而其与 hash 值进行 `&` 运算就刚好得到 hash 的模。
    - 为了保持其 capacity 为 2 的多次冥的形式，HashMap 中使用方法专门做些事：

    ```java
    /**
     * Returns a power of two size for the given target capacity.
     */
    static final int tableSizeFor(int cap) {
        int n = cap - 1;
        n |= n >>> 1;
        n |= n >>> 2;
        n |= n >>> 4;
        n |= n >>> 8;
        n |= n >>> 16;
        return (n < 0) ? 1 : (n >= MAXIMUM_CAPACITY) ? MAXIMUM_CAPACITY : n + 1;
    }
    ```

### HashMap 的树化

- HashMap 在某个桶内哈希冲突节点超过 8 且总元素数量超过 64 会将链表树化为红黑树.
- 在树中新元素插入时以节点 key 的 hash 值作为排序依据插入,在桶内查找元素时也就可以以 key 的 hash 值作为查询条件进行查询,其时间复杂度为 log2(n).

## HashTable 与 HashMap 的区别

- 底层基本与 HashMap 一致，初始都是使用 HashTable 算法，使用链表来解决 Hash Collision 。但 HashMap 更高明的算法在于，其中链表长度超过阈值 8 就链表会转为红黑树，这也避免了整个 HashTable 退化成一个 链表。
- HashTable 为同步的，可以保证一定的线程安全。

## ConcurrentHashMap 如何保证线程安全

> Base on JDK8

- Collections 中提供的工具方法 Collections.synchronizedMap() 只提供了粗粒度的同步,在 Map 的 get put size 等方法上直接加上了 synchronized 在方法层面进行同步,在高并发情况下性能受影响.
- ConcurrentHashMap 在基础节点数据都使用 volatile 保证其多核可见性,使用 CAS 无锁技术添加替换值,同时只在各个桶粒度加 synchronized 同步.
- ConcurrentHashMap 的 put 方法实现:在其所在桶内无数据时直接使用 CAS 技术 casTabAt() 将新值添加进去,如果已存在值则进入到桶内,只对桶加同步机制,依次对比 key 是否相同直到链表尾.
- initTable() 方法初始化其数组时使用 sizeCtl 作为并发标识,进入初始化先使用 Unsafe 机制 U.compareAndSwap(this, SIZECTL, sc, -1) 更新 sizeCtl 值
    - 此方法中,this 表当前 ConcurrentHashMap 对象, SIZECTL 用来票房 sizeCtl 标记在类元数据中的偏移量.sc 是初始值, -1 是新值用以标识正在初始化数组.
