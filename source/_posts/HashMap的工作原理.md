---
title: HashMap的工作原理
date: 2022-11-03 23：08：46
tags: [面试]
categories: Java基础
---

Java 中的 HashMap 是以键值对 (key-value) 的形式存储元素的。HashMap 需要一个 hash 函数，它使用 hashCode()和 equals()方法来向集合 / 从集合添加和检索元素。当调用 put() 方法的时候，HashMap 会计算 key 的 hash 值，然后把键值对存储在集合中合适的索引上。 如果 key 已经存在了，value 会被更新成新值。 HashMap 的一些重要的特性是它的容量 (capacity)，负载因子 (load factor) 和扩容极限(threshold resizing)。

<!--more-->

## 1. HashMap的存储结构

数组+链表+红黑树，当链表长度大于8时，链表的数据将会以红黑树的形式进行存储，当链表长度降到6时为链表形式存储

## 2. 如何解决Hash冲突

如果发生Hash冲突，Hashmap通过链表将产生冲突的元素组织起来，在Java 8中，如果一个bucket中冲突的元素超过某个限制(默认是8)，则使用红黑树来替换链表，从而提高速度。

## 3. Hashmap 扩容

当 hashmap 中的元素个数超过数组大小 loadFactor 时，就会进行数组扩容， loadFactor 的默认值为 0。75，也就是说，默认情况下，数组大小为 16，那么当 hashmap 中元素个数超过 160。75=12 的时候，就把数组的大小扩展为 216=32，即扩大一倍，然后重新计算每个元素在数组中的位置，而这是一个非常消耗性能的操作，所以如果我们已经预知 hashmap 中元素的个数，那么预设元素的个数能够有效的提高 hashmap 的性能。比如说，我们有 1000 个元素 new HashMap(1000)，但是理论上来讲 new HashMap(1024) 更合适，不过上面 annegu 已经说过，即使是 1000，hashmap 也自动会将其设置为 1024。 但是 new HashMap(1024) 还不是更合适的，因为 0。75*1000 < 1000，也就是说为了让 0。75 * size > 1000，我们必须这样 new HashMap(2048) 才最合适，既考虑了 & 的问题，也避免了 resize 的问题。

## 4. Hashmap线程安全吗

Hashmap不是线程安全的。

HashTable和ConcuHashMap是线程安全的。

### 4.1 HashTable

 HashTable线程安全实现是直接在关键方法上加了synchronized，但是只有一把锁。多线程访问同一个 Hashtable 就会直接造成锁冲突。（冲突概率很大，频繁加锁解锁又要耗费大量资源）size 属性也是通过 synchronized 来控制同步，也是比较慢的。一旦触发扩容，就由该线程完成整个扩容过程。这个过程会涉及到大量的元素拷贝，效率会非常低。

###  4.2 ConcuHashMap

相比于 Hashtable 做出了一系列的改进和优化。

每个bucket一把锁。

以 Java1.8 为例

- 读操作没有加锁(但是使用了volatile 保证从内存读取结果)，只对写操作进行加锁（减少锁冲突概率）。

- 加锁的方式仍然 是synchronized， 但是不是锁整个对象，而是 "锁桶" (用每个链表的头结点作为锁对象)，大大降低了锁冲突的概率

- 充分利用 CAS 特性。比如 size 属性通过 CAS 来更新。避免出现重量级锁的情况。

- 优化了扩容方式： 化整为零发现需要扩容的线程，只需要创建一个新的数组，同时只搬几个元素过去。扩容期间，新老数组同时存在。后续每个来操作 ConcurrentHashMap 的线程，都会参与搬家的过程。每个操作负责搬运一小 部分元素。搬完最后一个元素再把老数组删掉。这个期间，插入只往新数组加。这个期间，查找需要同时查新数组和老数组。相当于将HashTable的单次扩容所消耗的大量时间平摊到多次put过程，减少cpu的平均负荷，优化了体验。

## 5. 总结

- HashMap： 线程不安全。key 允许为 null 。
- Hashtable： 线程安全。使用 synchronized 锁 Hashtable 对象，效率较低，key 不允许为 null。
- ConcurrentHashMap： 线程安全。使用 synchronized 锁每个链表头结点， 锁冲突概率低，充分利用 CAS 机制， 优化了扩容方式，key 不允许为 null。

