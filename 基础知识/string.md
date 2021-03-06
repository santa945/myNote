# 字符串

字符串就是一串字符，由双（单）引号括起来。 

## 创建字符串

```
//方式一：字面量（推荐）
var str = '城市套路深，我想回农村';

//方式二：构造函数
//PS：用new产生的变量都是引用类型的变量，也叫对象
var str = new String('我不是黄蓉，我不会武功');
```

### 字符串的属性和方法

#### 属性

- length: 表示字符串的长度，只读（只能读取）

#### 获取值方法

- charAt(3) //获取下标为3的字符
- 备注：es5可以通过索引获取字符，例如 str[3]

#### 查找索引方法

- indexOf/lastIndexOf(keyword [,startIndex]) 从开头/尾部向后查找字符串`keyword`第一次出现的位置,如果没找到返回-1

##### 案例：输出所有1-100之间7的倍数和包含7的数字

```
for(var num=1;num<=100;num++){
    if(num%7===0 || String(num).indexOf(7)>=0){
    	document.write(num + ', ')
    }
}
```

##### 案例：16进制随机背景色

	var str = '0123456789abcdef';//15
	var res = '#';
	var randomIdx；
	for(var i=0;i<6;i++){
		randomIdx = parseInt(Math.random()*str.length）；
		res += str[randomIdx];
	}
	//封装成函数
#### 字符串分割成数组方法

- split(分割符)：根据分割字符，把字符串拆分成数组

#### 字符串替换方法

- replace(str|regExp,’‘) 替换字符串

  > 字符串的替换只能执行一次，不能够进行全局匹配，如果需要全局匹配，则应使用正则表达式

##### 案例：过滤不文明用语

	//1.获取元素
	//2.点击按钮，获取输入信息，将输入信息添加到数组，渲染到页面上。
	//3.定义敏感字符，用数组存放（可以先写成字符串，转成数组）
	//4.将输入信息添加到数组前，先替换不文明用语
	arr_mingan.forEach(function(item){
		//初级写法： _msg = _msg.replace(item,'**');
		// * 不能过滤大小写
		// * 不能过滤多个
		// 解决办法：利用构造函数创建正则表达式
		var reg = new RegExp(item,'ig');
		_msg = _msg.replace(reg,'**');
	})
#### 字符串的截取方法

- substring(start[,end])：不包括end所在字符，end省略表示截取到最后
- substr(start[,len])：支持负数，len为截取的数量
- slice(start[,end]) ：截取start到end(不包括end)的字符串，支持负数

##### 案例：获取url参数

	var url = 'https://www.baidu.com/s?name=laoxie&age=18&sex=male&';
	//1. 查找?号所在的索引值
	var idx = url.indexOf('?');
	//2.截取到参数字符串
	var params = url.slice(idx+1,-1);//name=laoxie&age=18&sex=male
	//3.生成参数数组
	params = params.split('&');//['name=laoxie','age=18','sex=male']
	// 4.遍历数组，获取所有属性/属性值，并组成一个对象
	var obj = {};//{key:value}
	params.forEach(function(kv){
		var arr = kv.split('=');
		obj[arr[0]] = arr[1]；
	});
#### 字符串大小写转换

- toLowerCase()：转换成小写
- toUpperCase()：转换成大写

#### ECMAscript5新增

- str[3]//通过下标获取
- trim()：删除前后所有空格，返回新的字符串

## ASCII码和字符集

- 字符串.charCodeAt(3) //获取下标为3的字符的编码
- String.fromCharCode(97) //编码转换成字符

# 了解正则表达式(后面再深入了解)

```
//创建方式：直接量
var reg = //gi;
//构造函数的方式
var reg = new RegExp('');
```

- g:表示匹配所有
- i:不区分大小写