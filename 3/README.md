## [目录](../README.md)


## 第3章　树
## [3.1　树和二叉树](#tree_subtree)
### [3.1.1　什么是树](#tree)
### [3.1.2　什么是二叉树](#subtree)
### [3.1.3　二叉树的应用](#subtree1)
## [3.2　二叉树的遍历](#subtree_bian)
### [3.2.1　为什么要研究遍历](#subtree_bian1)
### [3.2.2　深度优先遍历](#subtree_bians)
### [3.2.3　广度优先遍历](#subtree_biang)
## [3.3　什么是二叉堆](#dui)
### [3.3.1　初识二叉堆](#dui1)
### [3.3.2　二叉堆的自我调整](#dui2)
### [3.3.3　二叉堆的代码实现](#dui3)
## [3.4　什么是优先队列](#list)
### [3.4.1　优先队列的特点](#list1)
### [3.4.2　优先队列的实现](#list2)

****

<a name="tree_subtree"></a>
### 3.1　树和二叉树　
<a name="tree"></a>
#### 3.1.1　什么是树

在数据结构中， 树的定义如下。
树（tree） 是n（n≥0） 个节点的有限集。 当n=0时， 称为空树。 在任
意一个非空树中， 有如下特点。

1. 有且仅有一个特定的称为根的节点。
2. 当n>1时， 其余节点可分为m（m>0） 个互不相交的有限集， 每一
个集合本身又是一个树， 并称为根的子树。

树的最大层级数， 被称为树的高度或深度。 

<a name="subtree"></a>
#### 3.1.2　什么是二叉树

二叉， 顾名思义， 这
种树的每个节点最多有2个孩子节点。 
此外， 二叉树还有两种特殊形式， 一个叫作满二叉树， 另一个叫作
完全二叉树。

一个二叉树的所有非叶子节点都存在左右孩子， 并且所有叶子节点
都在同一层级上， 那么这个树就是满二叉树。

对一个有n个节点的二叉树， 按层级顺序编号， 则所有节点的编号
为从1到n。 如果这个树所有节点和同样深度的满二叉树的编号为从1到n
的节点位置相同， 则这个二叉树为完全二叉树。

完全二叉树的条件没有满二叉树那么苛刻： 满二叉树要求所有分支
都是满的； 而完全二叉树只需保证最后一个节点之前的节点都齐全即
可。

<a name="subtree1"></a>
#### 3.1.3　二叉树的应用

二叉查找树在二叉树的基础上增加了以下几个条件。

- 如果左子树不为空， 则左子树上所有节点的值均小于根节点的值
- 如果右子树不为空， 则右子树上所有节点的值均大于根节点的值
- 左、 右子树也都是二叉查找树
O(logn)

<a name="subtree_bian"></a>
### 3.2　二叉树的遍历　
<a name="subtree_bian1"></a>
#### 3.2.1　为什么要研究遍历

二叉树的遍历分为4种。

1. 前序遍历。
2. 中序遍历。
3. 后序遍历。
4. 层序遍历。

从更宏观的角度来看， 二叉树的遍历归结为两大类。

1. 深度优先遍历（前序遍历、 中序遍历、 后序遍历） 。
2. 广度优先遍历（层序遍历） 。

<a name="subtree_bians"></a>
#### 3.2.2　深度优先遍历

1. 前序遍历

二叉树的前序遍历， 输出顺序是根节点、 左子树、 右子树。


2. 中序遍历

二叉树的中序遍历， 输出顺序是左子树、 根节点、 右子树。

3. 后序遍历

二叉树的后序遍历， 输出顺序是左子树、 右子树、 根节点。

<a name="subtree_biang"></a>
#### 3.2.3　广度优先遍历

广度优先遍历： 先在各个方向上各走出1步， 再在各个方向上走出第2
步、 第3步……一直到各个方向全部走完。

<a name="dui"></a>
### 3.3　什么是二叉堆　
<a name="dui1"></a>
#### 3.3.1　初识二叉堆

二叉堆本质上是一种完全二叉树， 它分为两个类型。

1. 最大堆。
最大堆的任何一个父节点的值， 都大于或等于它
左、 右孩子节点的值。

2. 最小堆。
最小堆的任何一个父节点的值， 都小于或等于它
左、 右孩子节点的值。

最大堆和最小堆的特点决定了： 最大堆的堆顶是整个堆中的最大元
素； 最小堆的堆顶是整个堆中的最小元素。

<a name="dui2"></a>
#### 3.3.2　二叉堆的自我调整

关于堆的插入和删除操作， 时间复杂度确实是O(logn)。 构建堆的时间复杂度是O(n)。 
<a name="dui3"></a>
#### 3.3.3　二叉堆的代码实现



 	/**
	* “上浮”调整
	* @param array 待调整的堆
 	*/

	function upAdjust(&$array) {
		$childIndex = count($array)-1;
 		$parentIndex = ($childIndex-1)/2;
 		// temp 保存插入的叶子节点值， 用于最后的赋值
 		$temp = $array[$childIndex];
 		while ($childIndex > 0 && $temp < $array[$parentIndex])
 		{
 		//无须真正交换， 单向赋值即可
 			$array[$childIndex] = $array[$parentIndex];
 			$childIndex = $parentIndex;
 			$parentIndex = ($parentIndex-1) / 2;
 		}
 		$array[$childIndex] = $temp;
 	}

 	/**
 	* “下沉”调整
 	* @param array 待调整的堆
 	* @param parentIndex 要“下沉”的父节点
 	* @param length 堆的有效大小
 	*/
	function downAdjust(&$array, $parentIndex,$length) {
 	// temp 保存父节点值， 用于最后的赋值
 		$temp = $array[$parentIndex];
 		$childIndex = 2 * $parentIndex + 1;
 		while ($childIndex < $length) {
 		// 如果有右孩子， 且右孩子小于左孩子的值， 则定位到右孩子
 		if ($childIndex + 1 < $length && $array[$childIndex + 1] <$array[$childIndex]) {
 			$childIndex++;
 		}
 		// 如果父节点小于任何一个孩子的值， 则直接跳出
 		if ($temp <= $array[$childIndex])
 			break;
 			//无须真正交换， 单向赋值即可
 			$array[$parentIndex] = $array[$childIndex];
 			$parentIndex = $childIndex;
 			$childIndex = 2 * $childIndex + 1;
 		}
 		$array[$parentIndex] = $temp;
 	}

    /**
 	* 构建堆
 	* @param array 待调整的堆
 	*/
 	function buildHeap(&$array) {
 	// 从最后一个非叶子节点开始， 依次做“下沉”调整
		$len = count($array);
	 	for ($i = ($len-2)/2; $i>=0; $i--) {
			downAdjust($array, $i, $len);
	 	}
 	}


 	$array = array(1,3,2,6,5,7,8,9,10,0);
 	upAdjust($array);
	var_dump($array);

 	$array = array(7,1,3,10,5,2,8,9,6);
 	buildHeap($array);
 	var_dump($array);


<a name="list"></a>
### 3.4　什么是优先队列　
<a name="list1"></a>
#### 3.4.1　优先队列的特点

优先队列不再遵循先入先出的原则， 而是分为两种情况。

- 最大优先队列， 无论入队顺序如何， 都是当前最大的元素优先出
队
- 最小优先队列， 无论入队顺序如何， 都是当前最小的元素优先出
队
<a name="list2"></a>
#### 3.4.2　优先队列的实现

优先队列入队和出队的时间复杂度也
是O(logn)

	/**
	* 入队
	* @param key 入队元素
	*/
 	function enQueue(&$array, $key) {
		array_push($array, $key);
 		upAdjust($array);
 	}

 	/**
 	* 出队
 	*/
 	function deQueue(&$array){
	 	$len = count($array);
	 	if($len <= 0){
		 	return "the queue is empty !";
	 	}
 		//获取堆顶元素
 		$head = $array[0];

 		//让最后一个元素移动到堆顶

	 	$last = array_pop($array);
	 	if ($len > 1) {
	 	 	$array[0] = $last;
 		 	downAdjust($array);
	 	}

 		return $head;
 	}
 	/**
 	* “上浮”调整
 	*/
 	function upAdjust(&$array) {
 		$childIndex = count($array)-1;
 		$parentIndex = ($childIndex-1)/2;
 		// temp 保存插入的叶子节点值， 用于最后的赋值
 		$temp = $array[$childIndex];
 		while ($childIndex > 0 && $temp > $array[$parentIndex])
 		{
 		//无须真正交换， 单向赋值即可
 			$array[$childIndex] = $array[$parentIndex];
 			$childIndex = $parentIndex;
 			$parentIndex = $parentIndex / 2;
 		}
 		$array[$childIndex] = $temp;
	}
 	/**
	* “下沉”调整
 	*/
 	function downAdjust(&$array) {
 	// temp 保存父节点的值， 用于最后的赋值
 		$len = count($array);
 		$parentIndex = 0;
 		$temp = $len ? $array[$parentIndex] : '';

	 	if ($temp === '') {
	 		return false;
	 	}
 		$childIndex = 1;
 		while ($childIndex < $len) {
 		// 如果有右孩子， 且右孩子大于左孩子的值， 则定位到右孩子
 			if ($childIndex + 1 < $len && $array[$childIndex + 1] >$array[$childIndex]) {
 				$childIndex++;
		 	}
	 		// 如果父节点大于任何一个孩子的值， 直接跳出
 			if ($temp >= $array[$childIndex])
 				break;
 			//无须真正交换， 单向赋值即可
 			$array[$parentIndex] = $array[$childIndex];
 			$parentIndex = $childIndex;
 			$childIndex = 2 * $childIndex + 1;
 		}
		$array[$parentIndex] = $temp;
 	}



	$priorityQueue = array();
	enQueue($priorityQueue,3);
	enQueue($priorityQueue,5);
	enQueue($priorityQueue,10);
	enQueue($priorityQueue,2);
	enQueue($priorityQueue,7);
	var_dump($priorityQueue);
	var_dump(" 出队元素： " .deQueue($priorityQueue));
	var_dump(" 出队元素： " .deQueue($priorityQueue));