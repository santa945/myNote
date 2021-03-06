# ECMAScript5(ES5)Array新增方法

### 迭代（遍历）方法

#### （1）forEach(fn)

遍历方法，for循环没有太大差别，比for循环方便

```
arr.forEach(function(item){
	console.log(item)
});
```

#### （2）map(fn)

返回一个数量相等的数组，内容是什么取决于在fn中返回的值

- 案例：把数组中的每一个数字都增加20%，并返回新的数组
- 案例：利用map()快速生成商品列表

#### （3）filter(fn)

返回一个数组，存放执行fn后返回true的数组元素，利用这个方法可对数组元素进行过滤筛选

- 案例：找出数组中小于10的元素，组成新数组

```
var res = nums.filter(function(item){
	return item<10;
});	
```

#### （4）some(fn)

如果该函数对任何一项返回 true，则返回true

#### （5）every(fn)

如果该函数每一项都返回 true，才返回true

> 以上方法都对数组中的每一项运行给定函数fn,，函数中有三个形参分别为
> \- item：数组中的每一项,
> \- index：遍历过程中对应的索引值,
> \- array：对数组的引用

### 归并方法

> 这两个方法都会迭代数组中的所有项，然后生成一个最终返回值。

#### （1）reduce(fn,initVal)

#### （2）reduceRight(fn,initVal)

- fn(prev,cur,index,array): fn是每一项调用的函数，函数接受4个参数分别是
  - prev：函数上一次的返回值。（第一次的值参考initVal）
  - cur：当前值，
  - index：索引值，
  - array：当前数组，
- 函数需要返回一个值，这个值会在下一次迭代中作为prev的值
- initVal: 迭代初始值（可省略），如果缺省，prev的值为数组第一项

```
//对数组求和
var res = arr.reduce(function(prev,current,idx,arr){
	return prev+current;
},0);
```
### 静态方法

#### Array.isArray()

判断是否为数组，返回true/false

### 索引方法

> 区别就是一个从前往后找，一个从后往前找

#### indexOf/lastIndexOf(keyword [,startIndex])

- keyword: 要查找的项，
- startIndex：查找起点位置的索引，该参数可选，默认0

方法返回keyword所在数组中的索引值，如果数组不存在keyword，则返回-1

#### *应用：判断数组中是否存在某个值

```
arr.indexOf(key) != -1 //(1)
arr.indexOf(key) >= 0 //(2)
some() //(3)
```

### 案例：

1. 随机生成一个五位验证码，然后输出该验证码，每位分别是什么

```
//(1)用数组存放五位验证码
var res = [];
for(var i=0;i<5;i++){
	var num = parseInt(Math.random()*10);
	res.push(num);
}
console.log('该随机数是'+res.join(""));
//(2)得出每位分别是什么
var show = res.map(function(item,idx){
	return  '第'+(idx+1)+'位：' + item;
});
console.log(show.join());

```

2.以上数组的最大值，最小值和平均值，并输出他们的索引

	//1. 最大值及对应索引，同理最小值
	var maxIdx = 0;
	var max = arr[0];
	arr.forEach(function(item,idx){
	    if(item>max){
	        max = item;
	        maxIdx = idx;
	    }
	});
	//2.归并获取总数，求平均值
	var total = arr.reduce(function(prev,current){
	return prev+current;
	},0);
	var avg = total/arr.length;
3.显示10条最新消息