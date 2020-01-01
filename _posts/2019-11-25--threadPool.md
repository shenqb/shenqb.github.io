---
layout: post
title:  "线程池原理学习"
categories: 后端
tags:  后端 java
author: shenqb
---

* content
{:toc}


## 线程池参数

* corePoolSize：线程池中核心线程数的最大值；
*  maxPoolSize：线程池中能拥有最多线程数；
*  workQueue：用于缓存任务的阻塞队列；
*  keepAliveTime：表示空闲线程的存活时间。

这四个参数，决定了线程池对新任务的处理策略。

## 线程池原理

![线程池处理流程](https://raw.githubusercontent.com/shenqb/shenqb.github.io/master/img/ThreadPool.png)
* 如果把线程池比作一个单位的话，corePoolSize表示正式员工，线程可以表示一个员工。当向单位委派一项工作时，如果发现正式员工还没招满，单位就会招个正式员工来完成这项工作。随着向这个单位委派的工作增多，即使正式员工全部满了，工作还是干不完，那么单位只能按照新委派的工作先后顺序将他们找个地方搁置起来，这个地方就是 workQueue。等某个正式员工完成了手上的工作，就到放置任务的地方来领取新的任务。如果有大量的任务向这个单位委派，导致 workQueue已经没有空位来放置新的任务了，于是单位决定招临时工来完成任务。临时工不是想招多少就是多少，通过 maxPoolSize 规定了单位的人数最大值。
* 为了解释keepAliveTime的作用，我们在上述情况下做一种假设。假设线程池这个单位已经招了些临时工，但新任务没有继续增加，所以随着每个员工忙完手头的工作，都来workQueue领取新的任务（看看这个单位的员工多自觉啊）。随着各个员工齐心协力，任务越来越少，员工数没变，那么就必定有闲着没事干的员工。这样的话领导不乐意啦，但是又不能轻易fire没事干的员工，因为随时可能有新任务来，于是领导想了个办法，设定了keepAliveTime，当空闲的员工在keepAliveTime这段时间还没有找到事情干，就被辞退啦，毕竟地主家也没有余粮啊！当然辞退到corePoolSize个员工时就不再辞退了，领导也不想当光杆司令啊！**


##  四种拒绝策略

*  AbortPolicy(抛出一个异常，默认的)
*  DiscardPolicy(直接丢弃任务)
*  DiscardOldestPolicy（丢弃队列里最老的任务，将当前这个任务继续提交给线程池）
*  CallerRunsPolicy（交给线程池调用所在的线程进行处理)

##  五种任务队列

##### ArrayBlockingQueue
ArrayBlockingQueue（有界队列）是一个用数组实现的有界阻塞队列，按FIFO排序量。

##### LinkedBlockingQueue
LinkedBlockingQueue（可设置容量队列）基于链表结构的阻塞队列，按FIFO排序任务，容量可以选择进行设置，不设置的话，将是一个无边界的阻塞队列，最大长度为Integer.MAX_VALUE，吞吐量通常要高于ArrayBlockingQuene；newFixedThreadPool线程池使用了这个队列

##### DelayQueue
DelayQueue（延迟队列）是一个任务定时周期的延迟执行的队列。根据指定的执行时间从小到大排序，否则根据插入到队列的先后排序。newScheduledThreadPool线程池使用了这个队列。

##### PriorityBlockingQueue
PriorityBlockingQueue（优先级队列）是具有优先级的无界阻塞队列；

##### SynchronousQueue
SynchronousQueue（同步队列）一个不存储元素的阻塞队列，每个插入操作必须等到另一个线程调用移除操作，否则插入操作一直处于阻塞状态，吞吐量通常要高于LinkedBlockingQuene，newCachedThreadPool线程池使用了这个队列。

##  四种线程池类型
说明：基于四个核心参数，可搭配出四种类型的线程池：

*  newFixedThreadPool (固定数目线程的线程池)
*  newCachedThreadPool(可缓存线程的线程池)
*  newSingleThreadExecutor(单线程的线程池)
*  newScheduledThreadPool(定时及周期执行的线程池)

#### newFixedThreadPool
* 使用场景：
FixedThreadPool适用于处理CPU密集型的任务，确保CPU在长期被工作线程使用的情况下，尽可能的少的分配线程，即适用执行长期的任务。
* 线程池特点：
核心线程数和最大线程数大小一样
没有所谓的非空闲时间，即keepAliveTime为0
阻塞队列为无界队列LinkedBlockingQueue
使用无界队列的线程池会导致内存飙升吗？
答案 ：会的，newFixedThreadPool使用了无界的阻塞队列LinkedBlockingQueue，如果线程获取一个任务后，任务的执行时间比较长(比如，上面demo设置了10秒)，会导致队列的任务越积越多，导致机器内存使用不停飙升， 最终导致OOM。

#### newCachedThreadPool
* 使用场景：
用于并发执行大量短期的小任务。
* 线程池特点：
最大线程数为Integer.MAX_VALUE
阻塞队列是SynchronousQueue
非核心线程空闲存活时间为60秒
当提交任务的速度大于处理任务的速度时，每次提交一个任务，就必然会创建一个线程。极端情况下会创建过多的线程，耗尽 CPU 和内存资源。由于空闲 60 秒的线程会被终止，长时间保持空闲的 CachedThreadPool 不会占用任何资源。

#### newSingleThreadExecutor
* 使用场景：
适用于串行执行任务的场景，一个任务一个任务地执行。
* 线程池特点：
核心线程数为1
最大线程数也为1
阻塞队列是LinkedBlockingQueue
keepAliveTime为0

#### newScheduledThreadPool
* 使用场景：
周期性执行任务的场景，需要限制线程数量的场景。
* 线程池特点：
最大线程数为Integer.MAX_VALUE
阻塞队列是DelayedWorkQueue
keepAliveTime为0
scheduleAtFixedRate() ：按某种速率周期执行
scheduleWithFixedDelay()：在某个延迟后执行


引用：

<https://blog.51cto.com/14230003/2418026?source=dra>
<https://www.jianshu.com/p/125ccf0046f3>