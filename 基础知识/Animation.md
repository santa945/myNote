# Animation

## （一）运动原理

不断改变对象的属性产生动画的效果

## （二）动画分类

#### 匀速运动

速度保持不变的运动

##### 案例：飞翔的小鸟

```
//1.获取元素及初始化速度变量
let bird = document.querySelector('img');
let speed = 7;
//2.设置定时器
let timer = setInterval(()=>{
	//2.1 获取元素当前位置
	let left = bird.offsetLeft;
    //2.3 边界处理
    if(left >= window.innerWidth - bird.offsetWidth){
        left = window.innerWidth - bird.offsetWidth -speed;
        clearInterval(timer);
    }
    //2.2 将当前位置+速度变量值，更新赋值给当前元素的位置样式
    bird.style.left = left + speed + 'px';
},20);	
```

##### 案例：圆周运动

	//1.获取太阳、地球半径
	var sun_r = 155;
	var earth_r = 15;
	//2.设置太阳居中,太阳左上角的定位
	sun.style.left = (window.innerWidth-sun.offsetWidth)/2 +'px';
	sun.style.top = (window.innerHeight-sun.offsetHeight)/2 +'px';
	//3.设置地球初始角度
	var deg = 0;
	getPos(deg);
	//4.设置定时器，实现角度改变
	setInterval(function(){
	    deg +=5;
	    getPos(deg);
	},30)
	//设置地球左上角的位置
	function getPos(deg){
	    var rad = toRad(deg);
	    var a = sun_r*Math.sin(rad);
	    var b = sun_r*Math.cos(rad);
	    var x = window.innerWidth/2+a-earth_r;
	    var y = window.innerHeight/2-b-earth_r;
	    earth.style.left = x +'px';
	    earth.style.top = y +'px';
	}
	//角度转弧度
	function toRad(deg){
	    return deg*Math.PI/180;
	}

#### 加速运动

速度不断增加的运动

##### 案例：自由落体

```
//其实就是比匀速运动多了速度变量更新而已！！！
//1.获取元素及初始化速度变量	
let ball = document.querySelector(".ball");
let speed = 1; 
//2.设置定时器
let timer = setInterval(()=>{
	//2.1 获取元素当前位置
    let top  = ball.offsetTop;
    //2.2 更新速度变量
    speed++;
    //2.4 边界处理
    if(top >= window.innerHeight-ball.offsetHeight){
        top = window.innerHeight - ball.offsetHeight-speed;
        clearInterval(timer);
    }
    //2.3 将当前位置+速度变量值，更新赋值给当前元素的位置样式
	ball.style.top = top + speed + 'px';
}, 20)
```

#### 减速运动

速度不断减小的运动

##### 案例：刹车运动

```
//减速运动边界处理时，更多判断的是速度！！！
//1.获取元素及初始化速度变量
var car = document.querySelector('.car');
var speed = 60;
//2. 设置定时器
var timer = setInterval(()=>{
	//2.1 获取元素当前位置
    var left = car.offsetLeft;
    //2.2 更新速度变量
    speed -= 3;
   //2.3 速度边界处理
    if(speed<=0){
        speed = 0;
        clearInterval(timer);
    }
	//2.4 将位置更新赋值给当前元素的样式
	car.style.left = left + speed + 'px';
},50);
```

#### 抛物线运动

水平方向速度不断减小，垂直方向速度不断增加

##### 案例: 抛球不反弹

```
//垂直方向加速与水平方向减速的结合
//1.获取元素及初始化速度变量
var basketball = document.querySelector('.basketball');
var xspeed = 20;
var yspeed = 0;
//2. 设置定时器
var timer = setInterval(()=>{
	//2.1 获取元素当前位置
    var left = basketball.offsetLeft;
    var top = basketball.offsetTop;
	//2.2 更新速度变量
    yspeed++;
	xspeed -= 0.01;
	//2.3 边界判断
    if(top >= window.innerHeight - basketball.offsetHeight){
    	top = window.innerHeight - basketball.offsetHeight - yspeed;
        clearInterval(timer);
    }
	if(xspeed <= 0){
    	xspeed = 0;
    }
	//2.4 将位置更新赋值给当前元素的样式
    basketball.style.left = left + xspeed + 'px';
    basketball.style.top = top + yspeed + 'px';
},30);
```

##### 案例:抛物线的重力回弹

#### 缓冲运动

一开始速度很快，然后慢下来，直到停止

 缓冲运动的关键：动态计算速度（一般都跟目标值-当前值相关）

##### 案例：点击返回顶部

```
//1.window.onscroll事件，实现当window.scrollY的值大于一定的值时，出现点击返回顶部按钮
window.onscroll = ()=>{}
//2.给按钮绑定点击事件（事件触发定时器，一般都要先清除上一次的定时器）
totop.onclick = ()=>{
	clearInterval(timer);
    //2.2 开启定时器
    var timer = setInterval(()=>{
    	//2.2.1 获取元素当前位置
		var scrollTop = window.scrollY;
        //2.2.2 根据当前位置，更新速度变量值
        var speed = parseInt(scrollTop/10);
        // 2.2.3 边界判断
        if(speed<=10){
            speed = 10;
            clearInterval(timer);
        }
        //2.2.4 滚动到位置更新的位置
		window.scrollBy(0,-speed);
	},30);
}
```

##### 案例：图片展示上移效果

	for(let i=0;i<cols.length;i++){
		cols[i].onmouseenter = function(){
	        clearInterval(this.timer);
	        // 1.获取当前a标签及设置目标值
	        let link = this.children[1];
	        let target = 0;
	        //2.鼠标移入（不想冒泡就用enter）,设置定时器拿到当前top及设置缓冲速度。
	        this.timer = setInterval(()=>{
	            let current = link.offsetTop;
	            let speed = Math.ceil((current-target)/10);
	            current -= speed;
	            if(current <= target){
	                clearInterval(this.timer);
	                current = target;
	            }
	            link.style.top = current + 'px';
	        },30);
	    }
	}
	//鼠标移出，回复原来的位置。
#### 透明度变换

##### 案例：图片的淡入淡出

	box.onmouseover = function(){
		//4.多次进入事件，清楚上一次的定时器
	    clearInterval(timer);
	    timer = setInterval(function(){
	    	//1.鼠标移入，设置定时器，获取当前透明度*100.
	        var cur = getComputedStyle(img).opacity*100;
	        //2.具体的change看你想做什么运动。缓冲运动
	        var change = Math.ceil((100-cur)/10);
	        cur += change;
			//3.当获取到的当前透明度+change值>=100时，设置当前透明度的值。清除定时器
	        if(cur >= 100){
	            cur = 100;
	            clearInterval(timer);
	        }
	        //3.透明度改变的值除于100，给元素赋值样式
	        img.style.opacity = cur/100;
	    },100)
	}
	//鼠标移出，回复半透明效果。

## （三）动画的封装

    // 缓冲运动原理，利用(target-current)/10得到速度
    function animate(ele,attr,target){
        clearInterval(ele.timer);
        if(attr == "opacity"){target *= 100;}
        //1.定时器:
        //(1)获取ele的attr属性值，可能存在px、deg、target获取单位及数值
        //(2)设置缓冲速度
        //(3)设置当前属性值，判断达到目标值，清除定时器
        //(4)设置给元素作为样式
        //2.考虑透明度
        //3.函数一进来，先清除定时器，避免事件触发重复添加
        ele.timer = setInterval(function(){
            var current = getComputedStyle(ele)[attr];
            var unit = current.match(/[a-z]+$/i);
            unit = unit ? unit[0]: "";
            current = parseFloat(current);
            if(attr == "opacity"){current *= 100;}
            var speed = (target-current)/10;
            speed = speed>0? Math.ceil(speed) : Math.floor(speed);
            current += speed;
            if(current == target){
               clearInterval(ele.timer);
            }
            if(attr == "opacity"){current /= 100;}
         	ele.style[attr] = current + unit;
    	},100)
    }


​        
