---
title: 线程池
tags: [Java,ThreadPool,Executor]
date: 2021-04-01 15:51:33
---

## 线程池参数

- corePoolSize 线程池核心线程数量,默认情况下任务来时才会开始创建,不过可以修改为创建线程池时就创建核心数量线程
- maximumPoolSize 线程池最大线程数量,当任务队列满同时线程执行任务线程数量未达到此最大值时就会开始增加线程直到达到此最大值.达到最大值时会根据拒绝策略响应,默认报出 RejectException.
- keepAliveTime 超出核心线程数量的线程空闲时长.当超出核心线程数量的线程的等待最大时间.与之对应的是时间单位:unit
- workQueue 任务队列.当执行任务的线程数量超过 corePoolSize 时,任务会放入此阻塞队列.
- threadFactory 线程工厂, 生产线程的工厂.
- handler 任务拒绝处理器 RejectedExecutionHandler,当达到线程最大数量且任务队列已经满了时处理策略分为(默认使用 AbortPolicy):
    - AbortPolicy 直接报出异常
    - DiscardPolicy 直接将被拒绝的任务抛弃.
    - DiscardOldestPolicy 抛弃最老任务
    - CallerRunsPolicy 调用者执行

## 内置线程池

> 在 JDK Executors 中内置好的几种常用线程池工厂

- newFixedThreadPool(int nThreads) 固定线程数量,适合执行时长长量少的任务
    - corePoolSize 与 maximumPoolSize 设置为相等 nThread.
    - keepAliveTime 为 0
    - workQueue 为 new LinkedBlockingQueue<Runnable>() 无界队列
- newCachedThreadPool() 适合执行时长较短的大量任务 
    - corePoolSize 0 核心线程数量为 0,不会维持线程在线程池中.
    - maximumPoolSize Integer.MAX_VALUE 不限制最大线程数量
    - keepAliveTime 60, unit TimeUnit.Seconds, 最大等待时长 60s
    - workQueue SynchronousQueue 阻塞任务队列长度为 0,任何任务提交都会立即产生新的线程执行
- newSingleThreadExecutor() 单线程执行,适合串行执行的任务
    - corePoolSize 1 
    - maximumPoolSize 1
    - keepAliveTime 0 零等待
    - workQueue new LinkedBlockingQueue<>() 无界阻塞队列
- newScheduledThreadPool(int corePoolSize) 任务编排型线程池,适合周期型执行任务
    - corePoolSize corePoolSize 核心线程数
    - maximumPoolSize Integer.MAX_VALUE
    - keepAliveTime 0
    - workQueue new DelayedQueue() 延迟队列