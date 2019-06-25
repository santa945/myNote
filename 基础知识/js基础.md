# js历史

## JavaScript的诞生

JavaScript 诞生于 1995 年。由Netscape(网景公司)的程序员Brendan Eich(布兰登)与Sun公司联手开发一门脚本语言, 最初名字叫做Mocha，1995年9月改为LiveScript。12月，Netscape公司与Sun公司（Java语言的发明者）达成协议，后者允许将这种语言叫做JavaScript。这样一来，Netscape公司可以借助Java语言的声势。
1996年3月， Netscape公司的浏览器Navigator2.0浏览器正式内置了JavaScript脚本语言. 此后其他主流浏览器逐渐开始支持JavaScript.

## js版本

JavaScript这种语言的基本语法结构是由ECMAScript来标准化的, 所以我们说的JavaScript版本一般指的是ECMAScript版本.

· 1997年7月，ECMAScript 1.0发布。

· 1998年6月，ECMAScript 2.0版发布。

· 1999年12月，ECMAScript 3.0版发布。

· 2007年10月，ECMAScript 4.0版草案想要提交ECMA组织, 但由于4.0版的目标过于激进, 改动太大, 并且微软,谷歌等大公司极力反对；一直到2008年7月ECMA开会决定，中止ECMAScript 4.0的开发（即废除了这个版本）

· 2009年12月，ECMAScript 5.0版正式发布

· 2011年6月，ECMAscript 5.1版发布

· 2015年6月，ECMAScript 6正式发布，并且更名为“ECMAScript 2015”。

## js语言的组成

javascript = ECMAScript(核心) + BOM + DOM

> · 核心(ECMAScript)
>
> · 浏览器对象模型(BOM) browser
>
> · 文档对象模型(DOM) document

ECMA 欧洲电脑厂商联合会 制定ECMAScript规范。

W3c(万维网联盟)指定html及css规范

## JS和H5的关系

h5全称html5,不是单纯的html的第5个版本，而是一项最新的web标准，是html、css、javascript等技术的集合

# js语法

## 编写js代码的位置

1.作为html属性值

```
<a href="javascript:alert(100)"></a>
```

2.script标签

```
//(1)内部js
<script>
	//书写js代码
</script>

//(2)外部js
//[type="text/javascript"]文件类型可省略
//[src=""]引入js文件的路径
<script type="text/javascript" src="">
     //!!!带src属性的script标签内书写的js代码无效  
</script>
```

## js注释

    <script type="text/javascript">
    // 单行注释
    /*
       多行注释，不能嵌套多行注释
    */
    </script>
## 声明变量及赋值

> 变量名的命名规则：
>         * 不能以数字开头,包含字母、数字、_、$ 
>         * 不能使用关键字、保留字
>         * 严格区分大小写 
>         * 建议：
>              (1) 见名知意
>              (2) 驼峰命名 myUserName

```
// 1.声明变量,通过关键字var
var cup;
// 2.给变量赋值,通过=,将右边的值赋给左边的变量
cup = "orange";
// 3. 同时声明变量及赋值
var fruit = "lemon";
// 4. 同时声明多个变量,用逗号隔开
// var a,b,c;
// a = 10;
// b = 20;
// c = 30;
var a = 10,b = 20,c = 30;
```

> 代码习惯
>
> - 保持代码缩进
> - JS语句的末尾尽量写上分号;
> - 运算符两边都留一个空格, 如 `var n = 1 + 2`;

# 数据类型

## 基本数据类型

1. 数字类型number 

   * 普通数字
   * NaN：代表非数字
     * 不代表任何值，也不等于任何值，甚至自己都不等于自己
     * 任何数据与NaN运算都返回NaN
     * isNaN() 判断是不是非数字，是数字就返回false，其他值都返回true

   ```
   var a = 10;
   ```

2. 字符串类型 string

   * 有引号的值都是字符串类型

   ```
   var b = "apple";
   var c = 'orange';
   var e = 'What is your name?';
   //字符串中还存在引号？
   //- 将外层引号替换成另外一种
   //- 通过转义字符\
   var e = "What's your name?";
   var e = 'What\'s your name?';
   ```

3.  布尔类型 boolean
   * 只有两个值：true 、false

## 特殊数据类型（day2讲）

1. null
   * Null 类型只有一个特殊的值为 null,表示一个空对象引用(指针)。
   * typeof 操作符检测 null会返回object。
2. undefined
   * Undefined类型只有一个特殊的值为 undefined。
   * 通过var声明变量后,没有对变量进行赋值，这个变量的值就是undefined。

> 注意：
>
> underfine与not defined的区别：underfine是没有对变量进行初始化赋值，而not defined是报错，意思是该变量未定义、不存在，不能使用。

## 判断数据类型(day2)

```
 // 判断数据类型 typeof()
typeof(123); //"number"
typeof(NaN); //"number"
typeof(""); //"string"
typeof("AFDF"); //"string"
typeof(true); //"boolean"
typeof(false); //"boolean"
typeof(null); //"object" ！！！
typeof(undefined);//"undefined"
typeof(typeof(123));//"string"
```

# 输出

1. alert() 弹窗输出
2. document.write() 往body输出内容
3. console.log() 往控制台输出内容
4. 元素.innerHTML = “”;

```
//1. alert(具体的值||变量(不要加引号)) 
//2. document.write(具体的值||变量(不要加引号))
	//可以配合标签使用
//3. console.log(具体的值||变量(不要加引号)) 
var fruit = "apple";
alert(10);
alert("aa");
alert(true);
alert(fruit);
```

# 运算

## 算术运算

+, -, *, /, %:加,减,乘,除,取余(取模)

#### Ex:计算文本框的加减乘除

```
// 1.获取元素，用变量接收 document.getElementById("id名")
// 2.点击按钮时，执行函数。
//  * 1.给按钮添加点击事件 [onclick="函数名()"]
//  * 2.定义函数 function 函数名(){函数要做的事情}
//      * 获取元素的值
//      * 计算结果，赋给res.value
// 3.判断语句 if(条件){如果条件成立执行这里的代码}
var num1 = document.getElementById("num1");
var num2 = document.getElementById("num2");
var res = document.getElementById("res");
function calc(){
    var _num1 = num1.value;
    var _num2 = num2.value;
    res.value = _num1 * _num2;
}
```

#### 字符串拼接+

* +号两侧只要有一侧为字符串，就代表字符串拼接

#### 数据类型的转换

##### 隐式转换

若运算无法进行下去时，会尝试将数据类型进行隐式转换后，再运算。

##### 直接转换

```
 //1. Number() 转换成数字类型
     //* 字符串->数字： 空字符串转成数字为0.若可以转换成数字，返回值就是数字。若不能转成数字就是NaN。
     //* 布尔值->数字： true->1,false->0
 //2.String() 转换成字符串类型
	 //* 直接加引号
 //3. Boolean() 转换成布尔类型
	 //* 除了0、NaN、""、null、undefined转成false，其他都转成true。
Number("3");//3
Number("true");//NaN,非数字
Number(true);//1
Number(false);//0

String(124);//"124"
String(true);//"true"

Boolean(0);//false
Boolean(NaN);//false
```

#### Ex:字符串拼接引发的公司倒闭

#### Ex:计算抗洪时间

为抵抗洪水，战士连续作战89小时，编程计算共多少天零多少小时？

##### 常用的数学方法

> parseInt() 取整数 
> parseFloat() 取浮点数
> Math.round() 四舍五入
> Math.random() 获取0-1的随机数，包含0，不包含1
>
> number.toFixed(n) 将数字转化成带有n位小数的字符串（四舍五入）

## 赋值运算

 -= *= /= %=

- =  将右边的值赋值给左边的变量（不是等于的意思！！！）
- += 将右边的值追加给左边的变量

## 关系运算（可隐式转换）

· ==(等于), !=(不等于)

· <(小于)、>(大于)、<=(小于等于)、>=(大于等于)

· === 全等于，比较时要求值和类型都相等（不会进行类型隐式转换）

· !== 不全等于

> · 关系运算符的比较规则: 
> \1. 数字和数字比较, 直接比较大小
> \2. 数字和字符串比较, 字符串转换为数字后再比较
> \3. 字符串和字符串比较, 进行字符的ASCII码值比较
>
> http://ascii.911cha.com/

#### 案例：猜数字游戏

思考：两个文本框的数字如何比大小？Number()

## 逻辑运算

· &&: 逻辑与

· ||：逻辑或

· !: 逻辑非

```
// 1. && 与运算。运算两边返回值都为true，才返回true
//      * && 运算左边返回值为false，不再执行右边的代码
//      * && 运算左边返回值为true，右边代码直接返回对应的值
//          * 2<3 && 'aa' //'aa'
// 2. || 或运算。运算两边返回值都为false，才返回false
//      * || 运算左边返回值为true，不再执行右边的代码
//      * || 运算左边返回值为false，右边代码直接返回对应的值
// 3. ! 非运算，取反。
```

#### 案例：闰年平年

能被4整除但不能被100整除，或者能被400整除的才叫闰年

## 一元运算

++  对变量进行加1运算

- ++变量 : 先对变量进行加1运算，再将变量新的值返回出去
- 变量++ : 先将变量的值返回出去，再对变量进行加1运算

--   对变量进行减1运算

-  --变量： 先对变量进行减1运算，再将变量新的值返回出去
- 变量-- : 先将变量的值返回出去，再对变量进行减1运算

##  操作符优先级

| 运算符                                 | 描述                                     |
| -------------------------------------- | ---------------------------------------- |
| ., [], ()                              | 对象成员存取、数组下标、函数调用、分组等 |
| ++, –, ~, !, delete, new, typeof, void | 一元运算符                               |
| *, /, %                                | 乘法、除法、取模                         |
| +, -                                   | 加法、减法、字符串连接                   |
| <, <=, >, >=, instanceof               | 关系比较、检测类实例                     |
| ==, !=, ===, !==                       | 等于、恒等(全等)                         |
| &&                                     | 逻辑与                                   |
| `||`                                   | 逻辑或                                   |
| ?:                                     | 三元运算条件                             |
| =, +=, -=, *=, /=, %=                  | 赋值、运算赋值                           |
| ,                                      | 多重赋值、数组元素                       |

## 进制转换

1. 二进制 var num = 0b10101101;
2. 八进制 var num = 0o0123
3. 十进制  var num = 100;
4. 十六进制 var num = 0xef9302;

相互转换

```
//十进制转其他 number.toString(n) 
var number = 20;
number.toString(2); //转成2进制
//其他转十进制 paserInt(str,n)
var x='110';  
parseInt(x,16);   //16进制转成10进制
//其他转其他
先用parseInt转成十进制再用toString转到目标进制
```

<http://www.softwhy.com/article-9229-1.html>

# 参考

### 关键字

|          |          |            |        |
| -------- | -------- | ---------- | ------ |
| break    | do       | instanceof | typeof |
| case     | else     | new        | var    |
| catch    | finally  | return     | void   |
| continue | for      | switch     | while  |
| debugger | function | this       | with   |
| default  | if       | throw      | delete |
| in       | try      |            |        |

### 保留字

|       |        |         |       |
| ----- | ------ | ------- | ----- |
| class | enum   | extends | super |
| const | export | import  |       |

### 