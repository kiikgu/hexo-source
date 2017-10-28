---
title: HashMap详解
date: 2017-10-19 07:24:57
categories: Java
tags: [Java, Map]
toc: true
---

HashMap是我们在编程中常用的数据结构，本篇主要从HashMap的几个特性出发，分析其内部实现原理

* capacity和load_factor
* put/get
* keySet方法
* fast-fail

<!--more-->

### 概述
HashMap是我们在编程中常用的数据结构，本篇主要从HashMap的几个特性出发，深入分析其内部实现原理。

* capacity和load_factor
* put/get
* keySet方法
* fast-fail

#### capacity和load_factor
capacity和load_factor分别表示HashMap的容量和负载因子，简单来说，容量是指HashMap能装多少元素，load_factor指HashMap所能承受的负载有多大，就好比一个能装20L水的桶，全部装满会的话会增加我们操作的难度。同样，如果把HashMap装满的话，会降低HashMap的性能，所以当元素个数达到负载因子规定的阈值时，HashMap会进行扩容，容量扩大一倍。<br>

**HashMap结构**

为了探究capcacity和load_factor的含义，我们来看下HashMap的结构。HashMap内部实现是Hash表：<br>
![hash表结构图](/img/20171018/hashmap结构图.png)

图中可以看出，hash表是由数组和链表组成，数组的每个元素是链表的头结点，这样一个链表叫做一个桶，桶的个数就是HashMap的capacity，即是数组的长度；当hash表中的元素个数超过capacity * load\_factor时，hash表就要扩容。根据经验值load\_factor=0.75时，HashMap的性能和空间使用率最优。

**指定capacity和load_factor**

![HashMap构造函数](/img/20171018/hashMap构造函数1.png)

可以通过指定capacity和load_factor来构造HashMap。为了方便，HashMap的capacity取值总是2的n次方。这里用了一个很有意思的算法，求大于一个给定数值的最小2的n次方数。

![tableSizeFor方法](/img/20171018/hashMap_tableSizeFor.png)

![tableSizeFor方法原理图](/img/20171018/hashMap_tableSizeFor_detail.png)

**resize**

当HashMap元素个数大于阈值时，HashMap的容量会扩大一倍，部分元素会重新hash到新的桶中。
![HashMap resize重新hash算法](/img/20171018/hashMap_resize.png)

* 首先创建新的hash表
* 遍历旧hash表，将每个桶中的元素重新hash到新的hash表中

新的hash表的桶数是旧hash表的一倍，旧hash表中同一个桶的所有元素会被划分到newTable[n]和newTable[n+oldCap]这两个桶中，如上图hash表扩容后：

![重新hash](/img/20171018/hashMap_resize_detail.png)

（TreeNode就不在此处展开讨论了）

#### put/get

#### keySet方法

#### fast-fail





### 参考

[http://hllvm.group.iteye.com/group/topic/45517](http://hllvm.group.iteye.com/group/topic/45517)

[https://stackoverflow.com/questions/1313922/step-through-jdk-source-code-in-intellij-idea](https://stackoverflow.com/questions/1313922/step-through-jdk-source-code-in-intellij-idea)
