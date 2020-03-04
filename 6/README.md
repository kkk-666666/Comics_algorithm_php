## [目录](../README.md)


## 第6章　算法的实际应用
## [6.1　Bitmap的巧用](#bit)
### [6.1.1　一个关于用户标签的需求](#bit1)
## [6.2　LRU算法的应用](#lru)
### [6.2.1　用算法解决问题](#lru1)
## [6.3　如何实现红包算法](#hong)
### [6.3.1　用算法解决问题](#hong1)


****

<a name="bit"></a>
### 6.1　Bitmap的巧用　


<a name="bit1"></a>
#### 6.1.1　一个关于用户标签的需求　

Bitmap算法

 该算法主要用于对大量整数做去重和查询操作。

第1步， 建立用户名和用户ID的映射。

第2步， 让每一个标签存储包含此标签的所有用户ID， 每一个标签
都是一个独立的Bitmap。


[bitmap代码](https://github.com/fyibmsd/bitmap-php/blob/master/src/Bitmap.php "bitmap代码")


<a name="lru"></a>
### 6.2　LRU算法的应用　


<a name="lru1"></a>
#### 6.2.1　用算法解决问题　

这个算法基于一种假设： 长期不被使
用的数据， 在未来被用到的几率也不大。 因此， 当数据所占内存达
到一定阈值时， 我们要移除掉最近最少被使用的数据。


[lru代码](https://github.com/rogeriopvl/php-lrucache/blob/master/src/LRUCache/LRUCache.php "lru代码")



<a name="hong"></a>
### 6.3　如何实现红包算法　


<a name="hong1"></a>
#### 6.3.1　用算法解决问题　

方法1： 二倍均值法
假设剩余红包金额为m元， 剩余人数为n， 那么有如下公式。
每次抢到的金额 = 随机区间 [0.01， m /n × 2 - 0.01]元


	/**
	 * 拆分红包
	 * @param totalAmount 总金额（以分为单位）
	 * @param totalPeopleNum 总人数
	 */
	 function divideRedPackage($totalAmount, $totalPeopleNum){
		 $amountList = array();
		 $restAmount = $totalAmount;
		 $restPeopleNum = $totalPeopleNum;

 		 for($i=0; $i<$totalPeopleNum-1; $i++){
			 //随机范围： [1， 剩余人均金额的2倍-1] 分
			 $amount = rand(0,$restAmount / $restPeopleNum * 2 - 1) + 1;
			 $restAmount -= $amount;
			 $restPeopleNum --;
	
	 		 array_push($amountList, $amount);
 		}
 		array_push($amountList, $restAmount);

 		return $amountList;
 	}


	$amountList = divideRedPackage(1000, 10);
	
	var_dump($amountList);


方法2： 线段切割法


	function divideRedPackage($money, $people)
	{
	    //获取切割处
	    $temp = array();
	    while (count($temp) < $people - 1) {
        	$number = mt_rand(1, $money - 1);
        	$temp[$number] = 1;
    	}
 
	    //补头补尾
	    $temp[0] = 1;
	    $temp[$money] = 1;
	    $temp = array_keys($temp);
	    sort($temp);
 
	    //循环分配
	    $red_packet = array();
	    for ($i = 0; $i < $people; $i++) {
        	$red_packet[] = $temp[$i + 1] - $temp[$i];
    	}
	    //返回结果
	    return $red_packet;
	}



	$amountList = divideRedPackage(1000, 10);
	
	var_dump($amountList);