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
## [5.7　删去k个数字后的最小值](#shank)
### [5.7.1　解题思路](#shank1)
## [5.8　如何实现大整数相加](#dashu)
### [5.8.1　解题思路](#dashu1)
## [5.9　如何求解金矿问题](#jin)
### [5.9.1　解题思路](#jin1)
## [5.10　寻找缺失的整数](#yi)
## [5.10.1　问题扩展](#yi1)



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


<a name="shank"></a>
### 5.7　删去k个数字后的最小值

<a name="shank1"></a>
#### 5.7.1　解题思路

1.贪心算法,时间复杂度是O（kn）


	 /**
	 * 删除整数的k个数字， 获得删除后的最小值
	 * @param num 原整数
	 * @param k 删除数量
	 */
	 function removeKDigits($num, $k) {
 		$numNew = $num;
 		for($i=0; $i<$k; $i++){
 			$hasCut = false;
 			//从左向右遍历， 找到比自己右侧数字大的数字并删除
 			for($j=0; $j<strlen($numNew)-1;$j++){
 				if($numNew[$j] > $numNew[$j+1]){
 					$numNew = substr($numNew, 0, $j) . substr($numNew, $j+1,strlen($numNew));
 					$hasCut = true;
 					break;
 				}
 			}
 			//如果没有找到要删除的数字， 则删除最后一个数字
 			if(!$hasCut){
 				$numNew = substr($numNew, 0, strlen($numNew)-1);
 			}
 			//清除整数左侧的数字0
 			$numNew = removeZero($numNew);
 		}
 		//如果整数的所有数字都被删除了， 直接返回0
 		if(strlen($numNew) == 0){
 			return "0";
 		}
 		return $numNew;
 	}
 
 

 	function removeZero($num){
 		for($i=0; $i<strlen($num)-1; $i++){
 			if($num[0] != '0'){
 				break;
 			}
 			$num = substr($num, 1, strlen($num)) ;
 		}
 		return $num;
 	}



	var_dump(removeKDigits("1593212",3));
	var_dump(removeKDigits("30200",1));
	var_dump(removeKDigits("10",2));
	var_dump(removeKDigits("541270936",3));

2.栈,时间复杂度是O(n),空间复杂度是O(n)

	 /**
	 * 删除整数的k个数字， 获得删除后的最小值
	 * @param num 原整数
	 * @param k 删除数量
	 */
	 function removeKDigits($num, $k) {
 	//新整数的最终长度 = 原整数长度-k
 		$len = strlen($num);
 		$newLength = $len - $k;
 		//创建一个栈， 用于接收所有的数字
 		$stack = array();
 		$top = 0;
 		for ($i = 0; $i < $len; ++$i) {
 			//遍历当前数字
 			$c = $num[$i];
 			//当栈顶数字大于遍历到的当前数字时， 栈顶数字出栈（相当于删除数字）
 			while ($top > 0 && $stack[$top-1] > $c && $k > 0) {
 				$top -= 1;
 				$k -= 1;
 			}
 			//遍历到的当前数字入栈
 			$stack[$top++] = $c;
 		}
 		// 找到栈中第1个非零数字的位置， 以此构建新的整数字符串
 		$offset = 0;
 		while ($offset < $newLength && $stack[$offset] == '0') {
 			$offset++;
 		}
 		return $offset == $newLength? "0": join('',array_slice($stack,$offset,$newLength - $offset));
 	}



	var_dump(removeKDigits("1593212",3));
	var_dump(removeKDigits("30200",1));
	var_dump(removeKDigits("10",2));
	var_dump(removeKDigits("541270936",3));


<a name="dashu"></a>
### 5.8　如何实现大整数相加

<a name="dashu1"></a>
#### 5.8.1　解题思路

大整数相加的详细步骤:

第1步， 创建两个整型数组， 数组长度是较大整数的位数+1。 把每
一个整数倒序存储到数组中， 整数的个位存于数组下标为0的位置， 最
高位存于数组的尾部。 之所以倒序存储， 是因为这样更符合从左到右访
问数组的习惯。

第2步， 创建结果数组， 结果数组的长度同样是较大整数的位数
+1， +1的目的很明显， 是给最高位进位预留的。

第3步， 遍历两个数组， 从左到右按照对应下标把元素两两相加，
就像小学生计算竖式一样。

时间复杂度是O(n)

	 /**
	 * 大整数求和
	 * @param bigNumberA 大整数A
	 * @param bigNumberB 大整数B
	 */
	 function bigNumberSum($bigNumberA, $bigNumberB) {
	 //1.把两个大整数用数组逆序存储， 数组长度等于较大整数位数+1
 		$lena = strlen($bigNumberA);
 		$lenb = strlen($bigNumberB);
 		$maxLength = $lena > $lenb ? $lena : $lenb;

 
 		$arrayA = array();
 		for($i=0; $i<$lena; $i++){
 			$arrayA[$i] = $bigNumberA[$lena-1-$i];
 		}
 
 		$arrayB = array();
 		for($i=0; $i< $lenb; $i++){
 			$arrayB[$i] = $bigNumberB[$lenb-1-$i];
 		}
 

 		//2.构建result数组， 数组长度等于较大整数位数+1
 		$result = array();
 		//3.遍历数组， 按位相加
 		for($i=0; $i<$maxLength; $i++){
 			$temp = isset($result[$i]) ? $result[$i] : 0;
 			$temp += isset($arrayA[$i]) ? $arrayA[$i] : 0;
 			$temp += isset($arrayB[$i]) ? $arrayB[$i] : 0;

 			//判断是否进位
 			if($temp >= 10){
 				$temp = $temp-10;
 				$result[$i+1] = 1;
 			}
 			$result[$i] = $temp;
 		} 

		//4.把result数组再次逆序并转成String
	 
		krsort($result);
		return join('', $result);

 	}


	var_dump(bigNumberSum("426709752318", "95481253129"));


2.我们可以把大整数的每9位作为数组的一个元素， 进行加法运算。

	 /**
	 * 大整数求和
	 * @param bigNumberA 大整数A
	 * @param bigNumberB 大整数B
	 */
	 function bigNumberSum1($bigNumberA, $bigNumberB) {
 		//1.把两个大整数转为9位数的小数组
 		$snum = 9;
		$lena = strlen($bigNumberA);
 		$lenb = strlen($bigNumberB);
 
 		$lena1 = ceil($lena/$snum);
 		$lenb1 = ceil($lenb/$snum);
 		$maxLength = $lena1 > $lenb1 ? $lena1 : $lenb1;

 
 		$arrayA = c($bigNumberA, $snum, $lena1);
 		$arrayB = c($bigNumberB, $snum, $lenb1);

		 //2.构建result数组， 数组长度等于较大整数位数+1
		 $result = array();
 		//3.遍历数组， 按对应下标相加
 		for($i=0; $i<$maxLength; $i++){
			 $temp = isset($result[$i]) ? $result[$i] : 0;
			 $temp += isset($arrayA[$i]) ? $arrayA[$i] : 0;
			 $temp += isset($arrayB[$i]) ? $arrayB[$i] : 0;

 			//判断是否进位
 			if($temp >= pow(10, $snum)){
 				$temp = $temp-pow(10, $snum);
 				$result[$i+1] = 1;
 			}
 			$result[$i] = $temp;
 		} 
 
 		//4.把result数组再次逆序并转成String
		krsort($result);

		return join('', $result);

 	}
 
	 function c($bigNum, $snum, $t){
	     $array = array();
	     
	     $numlen = strlen($bigNum);
	     for($i=0; $i<$t; $i++){
	     
	         $len = $numlen-$i*$snum;
	         $len1 = $len >= $snum ? $snum : $len;
	         
	         $start = $len >= $snum ? $len-($i+1)*$snum: 0;
	    
	         $array[$i] = substr($bigNum, $start, $len1);
	     }
	     
	     return $array;
	 }


	var_dump(bigNumberSum1("426709752318", "95481253129"));



<a name="jin"></a>
### 5.9　如何求解金矿问题

<a name="jin1"></a>
#### 5.9.1　解题思路

题目
很久很久以前， 有一位国王拥有5座金矿， 每座金矿的黄金储量不
同， 需要参与挖掘的工人人数也不同。 例如有的金矿储量是500kg黄
金， 需要5个工人来挖掘； 有的金矿储量是200kg黄金， 需要3个工人来
挖掘……
如果参与挖矿的工人的总数是10。 每座金矿要么全挖， 要么不挖，
不能派出一半人挖取一半的金矿。 要求用程序求出， 要想得到尽可能多
的黄金， 应该选择挖取哪几座金矿？


动态规划题目
所谓动态规
划， 就是把复杂的问题简化成规模较小的子问题， 再从简单的子问
题自底向上一步一步递推， 最终得到复杂问题的最优解。

动态规划的要点： 确定全局最
优解和最优子结构之间的关系， 以及问题的边界。 这个关系用数学
公式来表达的话， 就叫作状态转移方程式。

时间复杂度O(nw),空间复杂度O(n)

	 /**
	 * 获得金矿最优收益
	 * @param w 工人数量
	 * @param p 金矿开采所需的工人数量
	 * @param g 金矿储量
	*/
	function getBestGoldMiningV3($w, $p, $g){
 		//创建当前结果
 
 		$results = array();
 		$results = array_pad($results, $w+1, 0);
 		$len = count($p);
 		//填充一维数组
 		for($i=1; $i<=$len; $i++){
 			for($j=$w; $j>=1; $j--){
     			if($j>=$p[$i-1]){
 					$results[$j] = max($results[$j], $results[$j-$p[$i-1]]+ $g[$i-1]);
 				}
 			}
 		}
 		//返回最后1个格子的值
 		return $results[$w];
 	}


	$w = 10;
	$p = array(5, 5, 3, 4 ,3);
	$g = array(400, 500, 200, 300 ,350);
	var_dump(" 最优收益： " . getBestGoldMiningV3($w, $p, $g));



<a name="yi"></a>
### 5.10　寻找缺失的整数

<a name="yi1"></a>
#### 5.10.1　问题扩展


1.在一个无序数组里有99个不重复的正整数， 范围是1～100， 唯独缺
少1个1～100中的整数。 如何找出这个缺失的整数？

这是一个很简单也很高效的方法， 先算出1+2+3+…+100的和， 然
后依次减去数组里的元素， 最后得到的差值， 就是那个缺失的整数。
假设数组长度是n， 那么该解法的时间复杂度是O(n)， 空间复杂度是O(1)。


2.第1次扩展：
一个无序数组里有若干个正整数， 范围是1～100， 其中99个整数都
出现了偶数次， 只有1个整数出现了奇数次， 如何找到这个出现奇数次
的整数？

解法：
遍历整个数组， 依次做异或运算。 由于异或运算在进行位运算时，
相同为0， 不同为1， 因此所有出现偶数次的整数都会相互抵消变成0，
只有唯一出现奇数次的整数会被留下。


3.第2次扩展：
假设一个无序数组里有若干个正整数， 范围是1～100， 其中有98个
整数出现了偶数次， 只有2个整数出现了奇数次， 如何找到这2个出现奇
数次的整数？

解法：
把2个出现了奇数次的整数命名为A和B。 遍历整个数组， 然后依次
做异或运算， 进行异或运算的最终结果， 等同于A和B进行异或运算的
结果。 在这个结果中， 至少会有一个二进制位是1（如果都是0， 说明A
和B相等， 和题目不相符） 。


	function findLostNum($array) {
 	//用于存储2个出现奇数次的整数
 		$result = array();
  		$result = array_pad($result, 2, 0);
 		//第1次进行整体异或运算
 		$xorResult = 0;
 		$len = count($array);
 		for($i=0;$i<$len;$i++){
 			$xorResult^=$array[$i];
	    }
 		//如果进行异或运算的结果为0， 则说明输入的数组不符合题目要求
 		if($xorResult == 0){
 			return null;
 		}
 		//确定2个整数的不同位， 以此来做分组
 		$separator = 1;
 		while (0==($xorResult&$separator)){
 			$separator<<=1;
 		}
 		//第2次分组进行异或运算
 		for($i=0;$i<$len;$i++){
 			if(0==($array[$i]&$separator)){
 				$result[0]^=$array[$i];
 			}else {
 				$result[1]^=$array[$i];
 			}
 		}

 		return $result;
 	}


	$array = array(4,1,2,2,5,1,4,3);
	$result = findLostNum($array);
	
	var_dump($result[0]. ",".$result[1]);

