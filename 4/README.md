## [目录](../README.md)


## 第4章　排序算法
## [4.1　引言](#y)
## [4.2　什么是冒泡排序](#mao)
### [4.2.1　初识冒泡排序](#mao1)
### [4.2.2　冒泡排序的优化](#mao2)
### [4.2.3　鸡尾酒排序](#jwj)
## [4.3　什么是快速排序](#kuai)
### [4.3.1　初识快速排序](#kuai1)
### [4.3.2　基准元素的选择](#xuan)
### [4.3.3　双边循环法](#huan)
### [4.3.4　单边循环法](#dan)
### [4.3.5　非递归实现](#fei)
## [4.4　什么是堆排序](#dui)
### [4.4.1　传说中的堆排序](#dui1)
### [4.4.2　堆排序的代码实现](#dui2)
## [4.5　计数排序和桶排序](#ji)
### [4.5.1　线性时间的排序](#ji1)
### [4.5.2　初识计数排序](#ji2)
### [4.5.3　计数排序的优化](#ji3)
### [4.5.4　桶排序](#tong)


****

<a name="y"></a>
### 4.1　引言　

根据时间复杂度的不同， 主流的排序算法可以分为3大类。

1. 时间复杂度为O(n2)的排序算法

- 冒泡排序
- 选择排序
- 插入排序
- 希尔排序（希尔排序比较特殊， 它的性能略优于O(n2)， 但又比不上
O(nlogn)， 姑且把它归入本类）

2. 时间复杂度为O(nlogn)的排序算法

- 快速排序
- 归并排序
- 堆排序

3. 时间复杂度为线性的排序算法

- 计数排序
- 桶排序
- 基数排序

<a name="mao"></a>
### 4.2　什么是冒泡排序　

<a name="mao1"></a>
#### 4.2.1　初识冒泡排序　

按照冒泡排序的思想， 我们要把相邻的元素两两比较， 当一个元
素大于右侧相邻元素时， 交换它们的位置； 当一个元素小于或等于右
侧相邻元素时， 位置不变。 平均时间复杂度是O(n2)


	function bubble_sort(&$array){
		$len = count($array);
		 for($i = 0; $i < $len - 1; $i++)
		 {
			 for($j = 0; $j < $len - $i - 1; $j++)
			 {
				 $tmp = 0;
				 if($array[$j] > $array[$j+1])
				 { 
					 $tmp = $array[$j];
					 $array[$j] = $array[$j+1];
					 $array[$j+1] = $tmp;
				 }
			 }
		 }
	}


	$array = [5,8,6,3,9,2,1,7];
	bubble_sort($array);
	var_dump($array);


<a name="mao2"></a>
#### 4.2.2　冒泡排序的优化　

	 function bubble_sort(&$array)
	 {
	 	//记录最后一次交换的位置
	 	$lastExchangeIndex = 0;
	 	$len = count($array);
		//无序数列的边界， 每次比较只需要比到这里为止
		$sortBorder = $len - 1;
	 	for($i = 0; $i < $len - 1; $i++)
	 	{
	 		//有序标记， 每一轮的初始值都是true
	 		$isSorted = true;
	 		for($j = 0; $j < $sortBorder; $j++)
	 		{
				$tmp = 0;
				if($array[$j] > $array[$j+1]){
	 				$tmp = $array[$j];
	 				$array[$j] = $array[$j+1];
	 				$array[$j+1] = $tmp;
	 				// 因为有元素进行交换， 所以不是有序的， 标记变为false
	 				$isSorted = false;
	 				// 更新为最后一次交换元素的位置
	 				$lastExchangeIndex = $j;
				 }
	 		}
	 		$sortBorder = $lastExchangeIndex;
	 		if($isSorted){
	 			break;
	 		}
	 	}
	 }

	 $array = [3,4,2,1,5,6,7,8];
	 bubble_sort($array);
	 var_dump($array);


<a name="jwj"></a>
#### 4.2.3　鸡尾酒排序　

鸡尾酒排序的优点是能够在特定条件
下， 减少排序的回合数； 而缺点也很明显， 就是代码量几乎增加了
1倍。


 	function jwj_sort(&$array)
 	{
		$len = count($array);
		$tmp = 0;
		for($i=0; $i<$len/2; $i++){
 			//有序标记， 每一轮的初始值都是true
 			$isSorted = true;
 			//奇数轮， 从左向右比较和交换
 			for($j=$i; $j<$len-$i-1; $j++){
 				if($array[$j] > $array[$j+1]){
 					$tmp = $array[$j];
 					$array[$j] = $array[$j+1];
 					$array[$j+1] = $tmp;
 					// 有元素交换， 所以不是有序的， 标记变为false
 					$isSorted = false;
 				}
 			}
 			if($isSorted){
 				break;
 			}
			// 在偶数轮之前， 将isSorted重新标记为true
 			$isSorted = true;
 			//偶数轮， 从右向左比较和交换
 			for($j=$len-$i-1; $j>$i; $j--){
 				if($array[$j] < $array[$j-1]){
 					$tmp = $array[$j];
 					$array[$j] = $array[$j-1];
 					$array[$j-1] = $tmp;
  					// 因为有元素进行交换， 所以不是有序的， 标记变为false
 					$isSorted = false;
 				}
 			}
 			if($isSorted){
 				break;
 			}
 		}
 	}


	$array = [2,3,4,5,6,7,8,1];
	jwj_sort($array);
	var_dump($array);


<a name="kuai"></a>
### 4.3　什么是快速排序　

<a name="kuai1"></a>
#### 4.3.1　初识快速排序

同冒泡排序一样， 快速排序也属于交换排序， 通过元素之间的比较
和交换位置来达到排序的目的。
不同的是， 冒泡排序在每一轮中只把1个元素冒泡到数列的一端，
而快速排序则在每一轮挑选一个基准元素， 并让其他比它大的元素移
动到数列一边， 比它小的元素移动到数列的另一边， 从而把数列拆解
成两个部分。这种思路就叫作分治法。总体的平均时间复杂度
是O(nlogn)

<a name="xuan"></a>
#### 4.3.2　基准元素的选择

最简单的方式是选择数列的第1个元素。或者随机选择一个元素作为基准元素， 并且让基准元素和数列首元素交换位置。

<a name="huan"></a>
#### 4.3.3　双边循环法

双边循环法

	function quickSort(&$arr, $startIndex,$endIndex) {
	// 递归结束条件： startIndex大于或等于endIndex时
		if ($startIndex >= $endIndex) {
 			return;
 		}
 		// 得到基准元素位置
 		$pivotIndex = partition($arr, $startIndex, $endIndex);

		// 根据基准元素， 分成两部分进行递归排序
 		quickSort($arr, $startIndex, $pivotIndex - 1);
 		quickSort($arr, $pivotIndex + 1, $endIndex);
 	}

	 /**
	 * 分治（双边循环法）
	 * @param arr 待交换的数组
	 * @param startIndex 起始下标
	 * @param endIndex 结束下标
	 */
 	function partition(&$arr, $startIndex,$endIndex) {
	 	// 取第1个位置（也可以选择随机位置） 的元素作为基准元素
	 	$pivot = $arr[$startIndex];
	 	$left = $startIndex;
	 	$right = $endIndex;

 		while( $left != $right) {
 			//控制right 指针比较并左移
 			while($left<$right && $arr[$right] > $pivot){
 				$right--;
 			}
 			//控制left指针比较并右移
			while( $left<$right && $arr[$left] <= $pivot) {
 				$left++;
 			}
 			//交换left和right 指针所指向的元素
 			if($left<$right) {
 				$p = $arr[$left];
 				$arr[$left] = $arr[$right];
 				$arr[$right] = $p;
 			}
 		}

 		//pivot 和指针重合点交换
 		$arr[$startIndex] = $arr[$left];
 		$arr[$left] = $pivot;

 		return $left;
 	}


	$arr = [4,4,6,5,3,2,8,1];
	$len = count($arr);
	quickSort($arr, 0, $len-1);
	var_dump($arr);


<a name="dan"></a>
#### 4.3.4　单边循环法


 	function quickSort(&$arr, $startIndex, $endIndex) {
 	// 递归结束条件： startIndex大于或等于endIndex时
 		if ($startIndex >= $endIndex) {
 			return;
 		}
 		// 得到基准元素位置
 		$pivotIndex = partition($arr, $startIndex, $endIndex);
 		// 根据基准元素， 分成两部分进行递归排序
 		quickSort($arr, $startIndex, $pivotIndex - 1);
 		quickSort($arr, $pivotIndex + 1, $endIndex);
 	}

	 /**
	 * 分治（单边循环法）
	 * @param arr 待交换的数组
	 * @param startIndex 起始下标
	 * @param endIndex 结束下标
	 */
 	function partition(&$arr, $startIndex,$endIndex) {
 	// 取第1个位置（也可以选择随机位置） 的元素作为基准元素
 		$pivot = $arr[$startIndex];
 		$mark = $startIndex;

 		for($i=$startIndex+1; $i<=$endIndex; $i++){
 			if($arr[$i]<$pivot){
 				$mark ++;
 				$p = $arr[$mark];
 				$arr[$mark] = $arr[$i];
 				$arr[$i] = $p;
 			} 
		}

 		$arr[$startIndex] = $arr[$mark];
 		$arr[$mark] = $pivot;
 		return $mark;
 	}


	$arr = [4,4,6,5,3,2,8,1];
	$len = count($arr);
	quickSort($arr, 0, $len-1);
	var_dump($arr);

<a name="fei"></a>
#### 4.3.5　非递归实现


 	function quickSort(&$arr, $startIndex,$endIndex) {
 		// 用一个集合栈来代替递归的函数栈
	 	$quickSortStack = [];
 		// 整个数列的起止下标， 以哈希的形式入栈
	 
		$rootParam = ['startIndex'=>$startIndex, 'endIndex'=>$endIndex ];
		array_push($quickSortStack, $rootParam);

 		// 循环结束条件： 栈为空时
 		while (count($quickSortStack)) {
 		// 栈顶元素出栈， 得到起止下标
	 		$param = array_pop($quickSortStack);
 		// 得到基准元素位置
	  		$pivotIndex = partition($arr, $param["startIndex"],$param["endIndex"]);
 		// 根据基准元素分成两部分, 把每一部分的起止下标入栈
	  		if($param["startIndex"] < $pivotIndex -1){
 				$leftParam = ["startIndex"=>$param["startIndex"], "endIndex"=>$pivotIndex-1];
 				array_push($quickSortStack, $leftParam);
 			}
 			if($pivotIndex + 1 < $param["endIndex"]){
	  			$rightParam = ["startIndex"=>$pivotIndex + 1, "endIndex"=>$param["endIndex"]];
  				array_push($quickSortStack, $rightParam);
 			}
 		}
 	}

	 /**
	 * 分治（单边循环法）
	 * @param arr 待交换的数组
	 * @param startIndex 起始下标
	 * @param endIndex 结束下标
	 */
	 function partition(&$arr, $startIndex, $endIndex) {
 		// 取第1个位置（也可以选择随机位置） 的元素作为基准元素
 		$pivot = $arr[$startIndex];
 		$mark = $startIndex;

 		for($i=$startIndex+1; $i<=$endIndex; $i++){
 			if($arr[$i]<$pivot){
 				$mark ++;
 				$p = $arr[$mark];
 				$arr[$mark] = $arr[$i];
 				$arr[$i] = $p;
 			}
 		}

 		$arr[$startIndex] = $arr[$mark];
 		$arr[$mark] = $pivot;
 		return $mark;
 	}

	$arr = [4,7,6,5,3,2,8,1];
	$len = count($arr);
	quickSort($arr, 0, $len-1);
	var_dump($arr);


<a name="dui"></a>
### 4.4　什么是堆排序　

<a name="dui1"></a>
#### 4.4.1　传说中的堆排序

堆排序算法的步骤。

1. 把无序数组构建成二叉堆。 需要从小到大排序， 则构建成最大
堆； 需要从大到小排序， 则构建成最小堆。
2. 循环删除堆顶元素， 替换到二叉堆的末尾， 调整堆产生新的堆
顶。


<a name="dui2"></a>
#### 4.4.2　堆排序的代码实现

整体的时间复杂度是O(nlogn)

	 /**
	 * “下沉”调整
	 * @param array 待调整的堆
	 * @param parentIndex 要“下沉”的父节点
	 * @param length 堆的有效大小
	 */
	 function downAdjust(&$array, $parentIndex, $length) {
 		// temp 保存父节点值， 用于最后的赋值
 		$temp = $array[$parentIndex];
 		$childIndex = 2 * $parentIndex + 1;
 		while ($childIndex < $length) {
 		// 如果有右孩子， 且右孩子大于左孩子的值， 则定位到右孩子
	 		if ($childIndex + 1 < $length && $array[$childIndex + 1] > $array[$childIndex]) {
 				$childIndex++;
 			}
 			// 如果父节点大于任何一个孩子的值， 则直接跳出
 			if ($temp >= $array[$childIndex])
 				break;
 			//无须真正交换， 单向赋值即可
 			$array[$parentIndex] = $array[$childIndex];
 			$parentIndex = $childIndex;
 			$childIndex = 2 * $childIndex + 1;
 		}
 		$array[$parentIndex] = $temp;
 	}


	 /**
	 * 堆排序（升序）
	 * @param array 待调整的堆
	 */
	 function heapSort(&$array) {
 		// 1. 把无序数组构建成最大堆
 		$len = count($array);
 		for ($i = ($len-2)/2; $i >= 0; $i--) {
 			downAdjust($array, $i, $len);
 		}

 		// 2. 循环删除堆顶元素， 移到集合尾部， 调整堆产生新的堆顶
 		for ($i = $len - 1; $i > 0; $i--) {
 			// 最后1个元素和第1个元素进行交换
 			$temp = $array[$i];
 			$array[$i] = $array[0];
 			$array[0] = $temp;
 			// “下沉”调整最大堆
 			downAdjust($array, 0, $i);
 		}
 	}

	$arr = [1,3,2,6,5,7,8,9,10,0];
	heapSort($arr);
	var_dump($arr);



<a name="ji"></a>
### 4.5　计数排序和桶排序　

<a name="ji1"></a>
#### 4.5.1　线性时间的排序

以计数排序来说， 这种排序算法是利
用数组下标来确定元素的正确位置的。

<a name="ji2"></a>
#### 4.5.2　初识计数排序

	function countSort($array) {
 	//1.得到数列的最大值
 		$max = $array[0];
 		$len = count($array);
 		for($i=1; $i<$len; $i++){
 			if($array[$i] > $max){
 				$max = $array[$i];
 			}
 		}
 		//2.根据数列最大值确定统计数组的长度

		$max1 = $max + 1 ;
 		$countArray = [];
 		$countArray = array_pad($countArray, $max1, 0);
	
 		//3.遍历数列， 填充统计数组
 		for($i=0; $i<$len; $i++){

 			$countArray[$array[$i]]++;
 		}

		 //4.遍历统计数组， 输出结果
		 $index = 0;
		 $sortedArray = [];
		 $countlen = count($countArray);
 		for($i=0; $i<$countlen; $i++){
 			for($j=0; $j<$countArray[$i]; $j++){
 				$sortedArray[$index++] = $i;
 			}
 		}
 		return $sortedArray;
 	}

	$array = [4,4,6,5,3,2,8,1,7,5,6,0,10];
	$sortedArray = countSort($array);
	var_dump($sortedArray);

<a name="ji3"></a>
#### 4.5.3　计数排序的优化

 	function countSort($array) {
 		//1.得到数列的最大值和最小值， 并算出差值d
 		$max = $array[0];
 		$min = $array[0];
 		$len = count($array);
 		for($i=1; $i<$len; $i++) {
 			if($array[$i] > $max) {
 				$max = $array[$i];
 			}
 			if($array[$i] < $min) {
 				$min = $array[$i];
 			}
 		}
 		$d = $max - $min;
 		//2.创建统计数组并统计对应元素的个数

 		$d1 = $d + 1;
 		$countArray = [];
		$countArray = array_pad($countArray, $d1, 0);

	 
		 for($i=0; $i<$len; $i++) {
		 	$countArray[$array[$i]-$min]++;
		 }

 		$countlen = count($countArray);
 		//3.统计数组做变形， 后面的元素等于前面的元素之和
 		for($i=1;$i<$countlen;$i++) {

 			$countArray[$i] += $countArray[$i-1];
 		}
 		//4.倒序遍历原始数列， 从统计数组找到正确位置， 输出到结果数组


	  	$sortedArray = [];
		$sortedArray = array_pad($sortedArray, $len, 0);
	 
 		for($i=$len-1;$i>=0;$i--) {
 			$sortedArray[$countArray[$array[$i]-$min]-1]=$array[$i];$countArray[$array[$i]-$min]--;
 		}
 		return $sortedArray;
 	}

	$array = [95,94,91,98,99,90,99,93,91,92];
	$sortedArray = countSort($array);
	var_dump($sortedArray);


计数排序有它的局限性， 主要表
现为如下两点。

1. 当数列最大和最小值差距过大时， 并不适合用计数排序。
2. 当数列元素不是整数时， 也不适合用计数排序。

<a name="tong"></a>
#### 4.5.4　桶排序

桶排序的总体时间复杂度为O(n)。
空间复杂度， 同样是O(n)


 	function bucketSort($array){

 		//1.得到数列的最大值和最小值， 并算出差值d
		 $max = $array[0];
		 $min = $array[0];
		 $bucketNum = count($array);
 		for($i=1; $i<$bucketNum; $i++) {
 			if($array[$i] > $max) {
 				$max = $array[$i];
 			}
 			if($array[$i] < $min) {
 				$min = $array[$i];
 			}
 		}
		$d = $max - $min;

 		//2.初始化桶

	 	$bucketList = [];
 		for($i = 0; $i < $bucketNum; $i++){
	 		array_push($bucketList,[]);
 		}

 		//3.遍历原始数组， 将每个元素放入桶中
 		for($i = 0; $i < $bucketNum; $i++){
 			$num = (($array[$i] - $min) * ($bucketNum- 1) / $d);
	  		array_push($bucketList[$num],$array[$i]);
 		}

 		$len = count($bucketList);
 		//4.对每个桶内部进行排序
 		for($i = 0; $i < $len; $i++){
	 		sort($bucketList[$i]);
 		}

 		//5.输出全部元素
	 	$sortedArray = [];

 		$index = 0;

		 foreach($bucketList as $v) {
		 	foreach($v as $v1){
				$sortedArray[$index] = $v1;
		 		$index++;
			}
		 }
		 return $sortedArray;
 	}

	$array = [4.12,6.421,0.0023,3.0,2.123,8.122,4.12, 10.09];
	$sortedArray = bucketSort($array);
	var_dump($sortedArray);