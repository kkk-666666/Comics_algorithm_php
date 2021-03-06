## [目录](../README.md)

## 第1章　算法概述
## [1.1　算法和数据结构](#suanfa)　
### [1.1.1　什么是算法](#suanfa1)　
### [1.1.2　什么是数据结构](#jiegou)　
## [1.2　时间复杂度](#shijian)　
### [1.2.1　算法的好与坏](#hao)　
### [1.2.2　渐进时间复杂度](#jian)　
## [1.3　空间复杂度](#kong)
### [1.3.1　什么是空间复杂度](#kong_fuza)
### [1.3.2　空间复杂度的计算](#kong_jisuan)



****

<a name="suanfa"></a>
### 1.1　算法和数据结构　
<a name="suanfa1"></a>
#### 1.1.1　什么是算法

在计算机领域里， 算法（algorithm）是一系列程序指令， 用于处理特定的运算和
逻辑问题。
衡量算法优劣的主要标准是时间复杂度和空间复杂度。

算法可以应用在很多不同的领域中， 其应用场景更是多种多样， 例
如下面这些。1. 运算 2. 查找 3. 排序 4. 最优决策 

<a name="jiegou"></a>
#### 1.1.2　什么是数据结构

数据结构， 对应的英文单词是data structure， 是数据的组织、 管理
和存储格式， 其使用目的是为了高效地访问和修改数据。

数据结构都有哪些组成方式呢？

1. 线性结构
线性结构是最简单的数据结构， 包括数组、 链表， 以及由它们衍生
出来的栈、 队列、 哈希表。

2. 树
树是相对复杂的数据结构， 其中比较有代表性的是二叉树， 由它又
衍生出了二叉堆之类的数据结构。

3. 图
图是更为复杂的数据结构， 因为在图中会呈现出多对多的关联关
系。

4. 其他数据结构
如跳表、 哈希链表、 位图等。

<a name="shijian"></a>
### 1.2　时间复杂度　
<a name="hao"></a>
#### 1.2.1　算法的好与坏

1. 运行时间长
2. 占用空间大

<a name="jian"></a>
#### 1.2.2　渐进时间复杂度

为了解决时间分析的难题， 有了渐进时间复杂度
（asymptotic time complexity） 的概念， 其官方定义如下。
若存在函数f(n)， 使得当n趋近于无穷大时， T(n)/f(n)的极限值为不
等于零的常数， 则称f(n)是T(n)的同数量级函数。 记作T(n)=O(f(n))， 称为O(f(n))， O为算法的渐进时间复杂度， 简称为时间复杂度。
因为渐进时间复杂度用大写O来表示， 所以也被称为大O表示法。

如何推导出时间复杂度呢？ 有如下几个原则。
- 如果运行时间是常数量级， 则用常数1表示
- 只保留时间函数中的最高阶项
- 如果最高阶项存在， 则省去最高阶项前面的系数

O(1)<O(logn)<O(n)<O(n2)

<a name="kong"></a>
### 1.3　空间复杂度
<a name="kong_fuza"></a>
#### 1.3.1　什么是空间复杂度

空间复杂度（space
complexity） 。
和时间复杂度类似， 空间复杂度是对一个算法在运行过程中临时占
用存储空间大小的量度， 它同样使用了大O表示法。
程序占用空间大小的计算公式记作S(n)=O(f(n))， 其中n为问题的规
模， f(n)为算法所占存储空间的函数。

<a name="kong_jisuan"></a>
#### 1.3.2　空间复杂度的计算

常见的空间复杂度有下面几种情形。

1. 常量空间 O(1)
2. 线性空间 O(n)
3. 二维空间 O(n2)
4. 递归空间 递归算法的空间复杂度和递归深度成正比
