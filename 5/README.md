## [目录](../README.md)


## 第5章　面试中的算法
## [5.1　如何判断链表有环](#huan)
### [5.1.1　解题思路](#huan1)
### [5.1.2　问题扩展](#huan2)
## [5.2　最小栈的实现](#xiaozhan)
### [5.2.1　解题思路](#xiaozhan1)
## [5.3　如何求出最大公约数](#gys)
### [5.3.1　解题思路](#gys1)
## [5.4　如何判断一个数是否为2的整数次幂](#two)
### [5.4.1　解题思路](#two1)
## [5.5　无序数组排序后的最大相邻差](#wuxu)
### [5.5.1　解题思路](#wuxu1)
## [5.6　寻找全排列的下一个数](#pai)
### [5.6.1　解题思路](#pai1)
## 5.7　删去k个数字后的最小值
### 5.7.1　又是一道关于数字的题目
### 5.7.2　解题思路
## 5.8　如何实现大整数相加
### 5.8.1　加法，你会不会
### 5.8.2　解题思路
## 5.9　如何求解金矿问题
### 5.9.1　一个关于财富自由的问题
### 5.9.2　解题思路
## 5.10　寻找缺失的整数
## 5.10.1 “五行”缺一个整数
## 5.10.2　问题扩展



****

<a name="huan"></a>
### 5.1　如何判断链表有环　


<a name="huan1"></a>
#### 5.1.1　解题思路　

怎么能够
更高效地判断一个链表是否有环

首先创建两个指针p1和p2 ， 让它们
同时指向这个链表的头节点。 然后开始一个大循环， 在循环体中， 让指
针p1每次向后移动1个节点， 让指针p2每次向后移动2个节点， 然后比较
两个指针指向的节点是否相同。 如果相同， 则可以判断出链表有环， 如
果不同， 则继续下一次循环。

时间复杂度为O(n)，空间复杂度是O(1)


	 /**
	 * 判断是否有环
	 * @param head 链表头节点
	 */
	 function isCycle($head) {
	    $p1 = $head->next;
	    $p2 = $head->next->next;
	    
	    while ($p1 !== null && $p2 !== null) {
	        if ($p1 === $p2) {
	            return true;
	        }
	        $p1 = $p1->next;
	        $p2 = $p2->next->next;
	    }
	    return false;
	 }

	 /**
	 * 链表节点
	 */
	class Node {
	    public $next = null;
	    public $data;
	    public function __construct($data) {
	        $this->data = $data;
	    }
	}


	$node1 = new Node(5);
	$node2 = new Node(3);
	$node3 = new Node(7);
	$node4 = new Node(2);
	$node5 = new Node(6);
	
	$node1->next = $node2;
	$node2->next = $node3;
	$node3->next = $node4;
	$node4->next = $node5;
	$node5->next = $node2;
	
	var_dump(isCycle($node1));


<a name="huan2"></a>
#### 5.1.2　问题扩展　

问题1：
如果链表有环， 如何求出环的长度？

当两个指针首次相遇， 证明链表有环的时候， 让两个指针从相遇点
继续循环前进， 并统计前进的循环次数， 直到两个指针第2次相遇。 此
时， 统计出来的前进次数就是环长。
因为指针p1每次走1步， 指针p2每次走2步， 两者的速度差是1步。
当两个指针再次相遇时， p2比p1多走了整整1圈。
因此， 环长 = 每一次速度差 × 前进次数 = 前进次数。


	 /**
	 * 获取环的长度
	 * @param head 链表头节点
	 */
	 function cycleLength($head) {
	    $p1 = $head->next;
	    $p2 = $head->next->next;
	    
	    $length = 0;
	    
	    while ($p1 !== null && $p2 !== null) {
	        if ($p1 === $p2) {
	            
	            while($p1 !== null && $p2 !== null) {
	                $p1 = $p1->next;
	                $p2 = $p2->next->next;
	                $length++;
	                
	                if ($p1 === $p2) {
	                    return $length;
	                }
	  
	            }
	            
	            break;
	        }
	        $p1 = $p1->next;
	        $p2 = $p2->next->next;
	    }
	    return $length;
	 }
 


	 /**
	 * 链表节点
	 */
	class Node {
	    public $next = null;
	    public $data;
	    public function __construct($data) {
	        $this->data = $data;
	    }
	}
	
	
	
	$node1 = new Node(5);
	$node2 = new Node(3);
	$node3 = new Node(7);
	$node4 = new Node(2);
	$node5 = new Node(6);
	
	$node1->next = $node2;
	$node2->next = $node3;
	$node3->next = $node4;
	$node4->next = $node5;
	$node5->next = $node2;
	
	var_dump(isCycle($node1));

问题2：
如果链表有环， 如何求出入环节点？

从链表头结点到入环点的距离， 等于从首次相遇点绕环
n-1圈再回到入环点的距离。
这样一来， 只要把其中一个指针放回到头节点位置， 另一个指针保
持在首次相遇点， 两个指针都是每次向前走1步。 那么， 它们最终相遇
的节点， 就是入环节点。


	 /**
	 * 获取环的入口点
	 * @param head 链表头节点
	 */
	 function findEntry($head) {
	    $p1 = $head->next;
	    $p2 = $head->next->next;
	    
	    $entry = null;
	    
	    while ($p1 !== null && $p2 !== null) {
	        if ($p1 === $p2) {
	            $p2 = $head;
	            while($p1 !== null && $p2 !== null) {
	                
	                if ($p1 === $p2) {
	                    return $p1->data;
	                }
	                
	                $p1 = $p1->next;
	                $p2 = $p2->next;
	  
	            }
	            
	            break;
	        }
	        $p1 = $p1->next;
	        $p2 = $p2->next->next;
	    }
	    return $entry;
	 }
 

	 /**
	 * 链表节点
	 */
	 class Node {
	    public $next = null;
	    public $data;
	    public function __construct($data) {
	        $this->data = $data;
	    }
	 }


	$node1 = new Node(5);
	$node2 = new Node(3);
	$node3 = new Node(7);
	$node4 = new Node(2);
	$node5 = new Node(6);
	
	$node1->next = $node2;
	$node2->next = $node3;
	$node3->next = $node4;
	$node4->next = $node5;
	$node5->next = $node2;
	
	var_dump(findEntry($node1));


<a name="xiaozhan"></a>
### 5.2　最小栈的实现　

<a name="xiaozhan1"></a>
#### 5.2.1　解题思路　

详细的解法步骤如下。

1. 设原有的栈叫作栈A， 此时创建一个额外的“备胎”栈B， 用于辅
助栈A。
2. 当第1个元素进入栈A时， 让新元素也进入栈B。 这个唯一的元素
是栈A的当前最小值。
3. 之后， 每当新元素进入栈A时， 比较新元素和栈A当前最小值的
大小， 如果小于栈A当前最小值， 则让新元素进入栈B， 此时栈B的栈顶
元素就是栈A当前最小值。
4. 每当栈A有元素出栈时， 如果出栈元素是栈A当前最小值， 则让
栈B的栈顶元素也出栈。 此时栈B余下的栈顶元素所指向的， 是栈A当中
原本第2小的元素， 代替刚才的出栈元素成为栈A的当前最小值。 （备
胎转正。 ）
5. 当调用getMin方法时， 返回栈B的栈顶所存储的值， 这也是栈A
的最小值。

显然， 这个解法中进栈、 出栈、 取最小值的时间复杂度都是O(1)，
最坏情况空间复杂度是O(n)。


	 /**
	 * 入栈操作
	 * @param element 入栈的元素
	 */
	 function push(&$mainStack, &$minStack, $element) {
		 array_push($mainStack, $element);
		 $minlen  = count($minStack);
		 //如果辅助栈为空， 或者新元素小于或等于辅助栈栈顶， 则将新元素压入辅助栈
		 if (!$minlen || $minlen && $element <= $minStack[$minlen-1]) {
		 	array_push($minStack, $element);
		 }
	 }

	 /**
	 * 出栈操作
	 */
	 function pop(&$mainStack, &$minStack) {
	
	      $minlen  = count($minStack);
	      $mainlen  = count($mainStack);
	     //如果出栈元素和辅助栈栈顶元素值相等， 辅助栈出栈
	 	  if ($mainStack[$mainlen-1] == $minStack[$minlen-1]) {
	 		array_pop($minStack);
	 	  }
	 	  return array_pop($mainStack);
	 }

	 /**
	 * 获取栈的最小元素
	 */
	 function getMin(&$mainStack, &$minStack) {
	     $mainlen = count($mainStack);
	     $minlen = count($minStack);
		 if (!$mainlen) {
		   return 'not data';
		 }
	
	 	 return $minStack[$minlen-1];
	 }


	push($mainStack, $minStack, 4);
	push($mainStack, $minStack, 9);
	push($mainStack, $minStack, 7);
	push($mainStack, $minStack, 3);
	push($mainStack, $minStack, 8);
	push($mainStack, $minStack, 5);
	
	var_dump(getMin($mainStack, $minStack));
	pop($mainStack, $minStack);
	pop($mainStack, $minStack);
	pop($mainStack, $minStack);
	var_dump(getMin($mainStack, $minStack));



<a name="gys"></a>
### 5.3　如何求出最大公约数　

<a name="gys1"></a>
#### 5.3.1　解题思路　

1.辗转相除法， 又名欧几里得算法（Euclidean algorithm） ， 该算法
的目的是求出两个正整数的最大公约数。 

这条算法基于一个定理： 两个正整数a和b（a>b） ， 它们的最大公
约数等于a除以b的余数c和b之间的最大公约数。时间复杂度可以近似为O(log(max(a,
b)))， 但是取模运算性能较差。

	function getGreatestCommonDivisorV2($a, $b)
	{
	 $big = $a>$b ? $a:$b;
	 $small = $a<$b ? $a:$b;
	 if($big%$small == 0){
	    return $small;
	 }
	    return getGreatestCommonDivisorV2($big%$small, $small);
	}


	var_dump(getGreatestCommonDivisorV2(25, 5));
	var_dump(getGreatestCommonDivisorV2(100, 80));
	var_dump(getGreatestCommonDivisorV2(27, 14));


2.更相减损术， 出自中国古代的《九章算术》 ， 也是一种求最大公约
数的算法。 它的原理更加简单： 两个正整数a和b（a>b） ， 它们的最大公约数
等于a-b的差值c和较小数b的最大公约数。算法性能不稳定， 最坏时间
复杂度为O(max(a, b))。

	function getGreatestCommonDivisorV3($a, $b){
	    if($a == $b){
	        return $a;
	    }
	    $big = $a>$b ? $a:$b;
	    $small = $a<$b ? $a:$b;
	    return getGreatestCommonDivisorV3($big-$small, $small);
	}
	
	
	
	
	var_dump(getGreatestCommonDivisorV3(25, 5));
	var_dump(getGreatestCommonDivisorV3(100, 80));
	var_dump(getGreatestCommonDivisorV3(27, 14));

3.最优方法： 把辗转
相除法和更相减损术的优势结合起来， 在更相减损术的基础上使用
移位运算。算法性
能稳定， 时间复杂度为O(log(max(a, b)))。

	function gcd($a, $b){
	     if($a == $b){
	         return $a;
	     }
	     if(($a&1)==0 && ($b&1)==0){
	        return gcd($a>>1, $b>>1)<<1;
	     } else if(($a&1)==0 && ($b&1)!=0){
	        return gcd($a>>1, $b);
	     } else if(($a&1)!=0 && ($b&1)==0){
	        return gcd($a, $b>>1);
	     } else {
	         $big = $a>$b ? $a:$b;
	         $small = $a<$b ? $a:$b;
	         return gcd($big-$small, $small);
	     }
	 }



	var_dump(gcd(25, 5));
	var_dump(gcd(100, 80));
	var_dump(gcd(27, 14));


<a name="two"></a>
### 5.4　如何判断一个数是否为2的整数次幂　

<a name="two1"></a>
#### 5.4.1　解题思路　
O（1） 的解法:对于一个整数n， 只需要计
算n&(n-1)的结果是不是0。

	 function isPowerOf2($num) {
	    return ($num&($num-1)) == 0;
	 }

	 var_dump(isPowerOf2(3));


<a name="wuxu"></a>
### 5.5　无序数组排序后的最大相邻差　

<a name="wuxu1"></a>
#### 5.5.1　解题思路
时间复杂度稳定在O(n)

 	function getMaxSortedDistance($array){

 	//1.得到数列的最大值和最小值
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
 		//如果max 和min 相等， 说明数组所有元素都相等， 返回0
 		if($d == 0){
 			return 0;
 		}

 		//2.初始化桶
 		$bucketNum = $len;
		$buckets = array();
 		for($i = 0; $i < $bucketNum; $i++){
 			$buckets[$i] = array('min'=>null, 'max'=>null);
 		}

 		//3.遍历原始数组， 确定每个桶的最大最小值
 		for($i = 0; $i < $len; $i++){
 			//确定数组元素所归属的桶下标
 			$index = (($array[$i] - $min) * ($bucketNum-1) / $d);
 			if($buckets[$index]['min']==null || $buckets[$index]['min']>$array[$i]){
 				$buckets[$index]['min'] = $array[$i];
 			}
 			if($buckets[$index]['max']==null || $buckets[$index]['max']<$array[$i]){
 				$buckets[$index]['max'] = $array[$i];
 			}
 		}

 		//4.遍历桶， 找到最大差值
		$leftMax = $buckets[0]['max'];
 		$maxDistance = 0;
 		for ($i=1; $i<$bucketNum; $i++) {
 			if ($buckets[$i]['min'] == null) {
 				continue;
 			}
 			if ($buckets[$i]['min'] - $leftMax > $maxDistance) {
 				$maxDistance = $buckets[$i]['min'] - $leftMax;
 			}
 			$leftMax = $buckets[$i]['max'];
 		}

 		return $maxDistance;
 	}


	$array = array(2,6,3,4,5,10,9);
	var_dump(getMaxSortedDistance($array));


<a name="pai"></a>
### 5.6　寻找全排列的下一个数

<a name="pai1"></a>
#### 5.6.1　解题思路

获得全排列下一个数的3个步骤。

1. 从后向前查看逆序区域， 找到逆序区域的前一位， 也就是数字置
换的边界。
2. 让逆序区域的前一位和逆序区域中大于它的最小的数字交换位
置。
3. 把原来的逆序区域转为顺序状态 。

这种解法拥有一个“高大上”的名字： 字典序算法。整体时间复杂度是O(n)

	function findNearestNumber(&$numbers){
 		//1. 从后向前查看逆序区域， 找到逆序区域的前一位， 也就是数字置换的边界
		$index = findTransferPoint($numbers);
 		// 如果数字置换边界是0， 说明整个数组已经逆序， 无法得到更大的相同数
 		// 字组成的整数， 返回null
 		if($index == 0){
 			return null;
 		}
 		//2.把逆序区域的前一位和逆序区域中刚刚大于它的数字交换位置
 		//复制并入参， 避免直接修改入参
 		$numbersCopy = $numbers;
 		exchangeHead($numbersCopy, $index);
 		//3.把原来的逆序区域转为顺序
 		reverse($numbersCopy, $index);
 		return $numbersCopy;
 	}

 	function findTransferPoint($numbers){
     	$len = count($numbers);
 		for($i=$len-1; $i>0; $i--){
 			if($numbers[$i] > $numbers[$i-1]){
 				return $i;
 			}
 		}
 		return 0;
 	}

	function exchangeHead(&$numbers, $index){
    	$head = $numbers[$index-1];
    	$len = count($numbers);
 		for($i=$len-1; $i>0; $i--){
 			if($head < $numbers[$i]){
 				$numbers[$index-1] = $numbers[$i];
 				$numbers[$i] = $head;
 				break;
 			}
 		}
 		return $numbers;
 	}

	function  reverse(&$num, $index){
    	$len = count($num);
 		for($i=$index,$j=$len-1; $i<$j; $i++,$j--){
 			$temp = $num[$i];
 			$num[$i] = $num[$j];
 			$num[$j] = $temp;
 		}
 		return $num;
 	}


 
	  $numbers = array(1,2,3,4,5);
	 //打印12345 之后的10个全排列整数
	 for($i=0; $i<5;$i++){
		 $numbers = findNearestNumber($numbers);
		 var_dump($numbers);
	 }