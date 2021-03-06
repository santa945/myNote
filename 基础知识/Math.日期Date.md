# Math

一个保存数学公式和信息的对象，一般用于执行数学任务

### 属性

Math.PI //3.1415926

### 方法

round(3.6) //四舍五入取整
ceil(11.3) //12 向上取整
floor(11.8) //11 向下取整
random() //返回0-1之间的随机数（不包括1）
max(num1, num2) //返回较大的数
min(num1, num2) //返回较小的数
abs(num) //绝对值
pow(x,y) //x的y次方
sqrt(num) //开平方根

### 相关数学知识

##### 三角函数复习

sin(radian)
cos(radian)
tan(radian)

##### 角度与弧度的转换

弧度=角度*Math.PI/180

##### 勾股定理复习

在直角三角形中，斜边的平方等于直角边的平方和

##### 曲线方程复习(一元二次方程)

##### 案例：表盘刻度

#  日期Date

### 了解时间

- GMT：格林尼治标准时(Greenwich Mean Time)，俗称“天文学时间”
- UTC：协调世界时(Universal Time Coordinated)，“原子物理时间”，它更加精确,50亿年才误差1秒
- 时区：为了克服时间上的混乱，1884年在华盛顿召开的一次国际经度会议（又称国际子午线会议[1]）上，规定将全球划分为24个时区（东、西各12个时区）。规定英国（格林尼治天文台旧址）为中时区（零时区）、东1-12区，西1-12区。每个时区横跨经度15度，时间正好是1小时
- 闰年：四年一闰，百年不闰，四百年再闰
- 纪元时间(UNIX TIME)：1970-1-1零时

### 创建日期时间对象

```
构造函数
    * 不带参数：得到的是代码执行时的时间（本地时间）
    * 带参数
        * 字符串：指定日期
        * 数字：指定毫秒数
//1）创建当前时间的日期和时间
var d = new Date();

//2）创建指定日期的时间和日期
var d = new Date("2015/08/22");
var d = new Date(56521211021);//参数为距1970-1-1零时的毫秒数
```

### 日期时间对象的方法

##### 获取年月日

getFullYear()/setFullYear(2014)
getMonth()/setMonth(8)注意：获取月份是从0开始的
getDate()/setDate(25)

##### 获取星期

getDay() 0-6:星期天-星期六

##### 获取时分秒

getHours()/setHours()
getMinutes()/setMinutes()
getSeconds()/setSeconds()

##### 设置毫秒数

getTime()/setTime()：获取/修改某个日期自1970年1月1日0时以来的毫秒数

###### 案例：将当前日期格式化输出为 “2018年08月24 星期五”格式

	var d = new Date();
	// 1.获取年份、月份、日、星期
	var year = d.getFullYear();
	var month = d.getMonth()+1;
	var date = d.getDate();
	var day = d.getDay();//0-6代表星期日到星期六
	var week = '天，一，二，三，四，五，六'.split('，');
	output.innerHTML = '当前时间为：' + year + '年' + month + '月' + date + '日 星期' + week[day];
	//2.使用定时器，每秒执行一次上面的代码（封装成函数）。setInterval(fn,1000);
	//3.一开始渲染页面时，要先执行一次函数。（因为定时器设置1秒的间隔）
	//4.优化：当时分秒小于10的时候，补0操作

###### 案例：时间设置： 日期date的n天后的日期

	function afterDate(date,n){
	    // 指定日期创建对象
	    var d = new Date(date);
	    d.setDate(d.getDate() + n);
	    return d;
	}
##### 日期处理

- toLocaleDateString(); 以特定地区格式显示年、月、日
- toUTCString(); 转换成UTC时间

###### 案例：判断两个日期相差的天数

```
1.把开始时间和结束时间转成毫秒数
2.两个毫秒数相减，得到时间差
3.把时间差转成天数
```

### ES5方法

Date.parse(“2015-08-24”)//返回指定日期距1970-1-1零时的毫秒数

> PS：转换格式默认支持2015-08-24或2015/08/24

Date.now();//返回执行这行代码时距1970-1-1零时的毫秒数

### 延迟与定时器

- setTimeout(fn,200)：两百毫秒后执行fn这个函数（只执行一次）,返回一个id标识
- clearTimeout(timeoutID)：清除指定id标识的延迟操作
- setInterval(fn,30)：每隔30毫秒执行一次fn这个函数,返回一个id标识
- clearInterval(intervalID)：清除指定id标识的定时器操作

```
var timer = setTimeout(function(){
	//2s后执行这里的代码
},2000);

//清除
clearTimeout(timer);
```

###### 案例：实现进度条

	// 1.进度条变量初始宽度
	var width = 0;
	// 2.进度条变量宽度每隔一段时间，递增一定的值
	var timer = setInterval(function(){
		width += 13;
		// 3.当进度条变量宽度大于浏览器可视区域宽度，停止定时器，赋值为浏览器可视区域的宽度
		if(width >= window.innerWidth){//333,366
	        width = window.innerWidth;
	        clearInterval(timer)
		}
	    // 4.将进度条变量的值，赋值给进度条样式
	    bar.style.width = width + 'px';
	},30);
###### 案例：实现倒计时

```
1）指定结束时间
var end = '2018-3-1 14:50:40';					
2）定时器，每秒拿当前时间跟结束时间对比，计算差值
	var offset = Date.parse(end) - Date.now();
3）把差值转换成《剩余时间》
	offset = Math.floor(offset/1000);
	var sec = offset%60;//50,5
    var min = Math.floor(offset/60)%60;
    var hour = Math.floor(offset/60/60)%24;
    var day = Math.floor(offset/60/60/24);
    // 补0操作
4）拼接时间格式，写入页面
5）倒计时结束时（应写在步骤3前面，不再呈现在页面上）
    if(offset <= 0){
         // 停止定时器
         // 替换购买按钮
		// *隐藏倒计时
	}
```

