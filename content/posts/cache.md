---
title: "Cache Note"
date: 2023-04-14T18:40:46+08:00
draft: false
taps: [CSAPP, CS61c, cache]
categories: ["Computer system"]
author: ["Yeelight"]
showtoc: true
math: true
---

*资料来源: cs61c*

> cache可以说是计算机技术革命中最伟大的想法了

想一个问题：在我们的电脑里，指令是怎么控制内存里的东西的？因为我们要运行电脑除了CPU以外我们要向外`拿取`数据才能执行一系列的指令，这样电脑才算运行起来。

让我们来看下面的这张图，这是十分完整的计算机组成结构：

##  Components of a Computer

![Components of a Computer](https://s2.loli.net/2023/04/24/Fzb3uHQBLTlqOgD.png)

我们可以从中看到在CPU需要运行一个进程的时候，首先会将指令告诉主存（main memory）,然后开始在主存中找地址（Address）找到后加载到在CPU内部通用寄存器（register）然后开始执行
执行完后再写入主存中。
> 在这里面还有一个步骤，memory要先向disk中读取数据

其实现实中，CPU通用寄存器的速度和主存之间存在着太大的差异。两者之间的速度大致如下关系：
![memory-steep](https://s2.loli.net/2023/04/24/KtvMSR7QmrXpjbl.png)

Oh!!!它们相差1,000倍左右，这是无法想像的，就比如当我前1ns的时候CPU已经做完了，而我还要等1000ns的memory的时间，因此在我们看来CPU此时是空闲的，大大的浪费了。

因此，如果我们可以提升主存的速度，那么对于系统来说将会获得很大的性能提升。但我们试图提升主存的速度和容量，又期望其成本很低，这就有点难为人了。因此，我们有一种折中的方法，那就是制作一块速度极快但是容量极小的存储设备。那么其成本也不会太高。这块存储设备我们称之为`cache`。在硬件上，我们将`cache`放置在`CPU`和`主存`之间，作为主存数据的缓存。 当CPU试图从主存中load/store数据的时候， CPU会首先从cache中查找对应地址的数据是否缓存在cache 中。如果其数据缓存在cache中，直接从cache中拿到数据并返回给CPU。

![add-cache.png](https://s2.loli.net/2023/04/24/qLhOmsTV2IE9S8p.png)


其实类比的话，我蛮喜欢CS61c里面的Library Analogy，而我自己的想法是有点像现在的物流运输：对一些物品都有一个`主要的仓库`，而也有一些`本地仓`，当我要送东西的时候我先去看看`本地仓`有没有，没有就再去`主仓`去看看，但时间上就没有本地仓的快

> [[2. Areas/01 Blog/03-ComputerSystems/cs61c/SRAM vs. DRAM vs. Disk]]

## Memory Hierarchy

好的现在我们知道了`cache`的出现了，而下面的图是说明了对于不同的内存级别
![Cache-line.png](https://s2.loli.net/2023/04/24/3anw1UgNoWDZBsq.png)

## cache

### cache的级别
> 每一级的cache就是每一个下级内存的副本

cahe的速度在一定程度上同样影响着系统的性能.当cache中没有缓存我们想要的数据的时候，依然需要漫长的等待从主存中load数据。为了进一步提升性能，引入多级cache。前面提到的cache，称之为L1 cache（第一级cache）。我们在L1 cache 后面连接L2 cache，在L2 cache 和主存之间连接L3 cache。等级越高，速度越慢，容量越大。

### Temporal Locality(时间局部性)
> If a memory location is referenced then it will tend to be referenced again soon

比如说我用过一次这个地址,我保存起来以防我下次使用

### Spatial Locality(空间局部性)
> If a memory location is referenced, the locations with nearby addresses will
   tend to be referenced soon

比如一个数组，在我读取的时候它会把数组左右的都读取了

### Cache Hit vs Cache Miss

在我要对数据进行查找的时候会出现两种情况`Cache Hit` &`Cache Miss`.

#### cache hit
你要查找的数据`在缓存中` 从缓存中检索数据并将其带到处理器.

#### cache miss
你要查找的数据`不在缓存中` 去内存中找数据，把数据放到缓存中，带到处理器中

## Cache的工作原理

现在我们来继续说一些快取的工作原理, 在此之前先来说一下的一些名词
	什么是 `line/tag/index/offset/valid`

- **line:** 我们将cache平均分成相等的很多块，每一个块大小称之为`cache line`也可以叫`cache block`，其大小是`cache line size`。
- **tag:** Used to identify the data(用于识别数据)。每条Cache Line前都会有一个独立分配的内存来存tag，其就是内存地址的前Nbits。
$$ addressbits -  offsetbits $$
- **offset:** Identifies the byte offset(标识字节偏移量)。一般是低位后几位。
$$ offset = log_2(line size) $$
- **index:** 内存地址后续的bits则是在这--Way的是Cache Line索引，可以索引Cache Line。
- **Valid bit:** Tells you if the data stored at a given cache line is valid(告诉您存储在给定缓存行中的数据是否有效)


> 一个地址访问要映射到 Cache 中，地址被分成三个字段：tag，set index, block offset。这样，通过一个物理地址就可以获取数据或指令在缓存中的位置(set, way, byte))


![line-block.png](https://s2.loli.net/2023/04/24/mHdMoveGWXkiNL4.png)

### Direct mapped cache(直接映射缓存)

优点：直接映射缓存在硬件设计上会更加简单，因此成本上也会较低。
> 一句话, 我一个个的加载进入cache,当我的cache满了我就转头再来一遍

>只适合于**大容量**Cache

缺点: 继续访问下面的地址时，依然会cache缺失。这就相当于每次访问数据都要从主存中读取，所以cache的存在并没有对性能提升有效,有`cache颠簸`(每个主存块只有一个固定位置可存放，容易产生冲突)

![Direct Mapped.png](https://s2.loli.net/2023/04/24/xeav7mlIDAyOwqK.png)

![Direct Mapped-cache.png](https://s2.loli.net/2023/04/24/4EYI7Va1S5lKgow.png)

### Two-way set associative cache(两路组相连缓存)
**Cache分了2组**

优点:减少cache颠簸出现频率
> 组相联映射实际上是直接映射和全相联映射的折中方案

缺点: 增加硬件设计复杂读、成本较高(需要比较多个cache line的TAG)

![set associative-cache.png](https://s2.loli.net/2023/04/24/yXE8J6RMo9F3Vxq.png)


### Fully Associative Cache(全相连缓存)

优点:最大程度的降低cache颠簸的频率
>只适合于**小容量**Cache

缺点: 增加硬件设计复杂读、成本较高(需要比较多个cache line的TAG)

扩展：[[More Eviction Policies]]

![Fully Associative.png](https://s2.loli.net/2023/04/24/76uSATyrPno1eYf.png)

寻找 Hit 的电路

![Required to Check for Hit](https://s2.loli.net/2023/04/24/3VYzGo9dkgHwcrS.png)

[[Types of Misses]]

### Comparisons

三个 cache 的区别之分
![Comparisons](https://s2.loli.net/2023/04/24/iICnWkpOMcFtKZH.png)
**需补充**
