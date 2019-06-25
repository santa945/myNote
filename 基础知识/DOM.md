DOM

​	DOM是Document Object Model（文档对象模型）的缩写，它是W3C国际组织的一套Web标准。是针对HTML和XML文档的一个API，它定义了访问HTML文档对象的一套属性、方法和事件。

### 了解DOM树(节点)

![1535339883248](C:\Users\ADMINI~1\AppData\Local\Temp\1535339883248.png)

```
 <div id="box" class="bb">bilibili</div>//此处4个节点，1元素，2属性，1文本
```

### 获取节点方法

#### 1.获取元素节点

```
 1. document.getElementById()  通过id获取元素，返回值为元素节点或者为null
     * 必须通过document调用
     * 速度最快
 2. getElementsByClassName() 通过类名获取元素,返回类数组，如果类名不存在返回空数组[]
 	* 元素节点均可调用
 3. getElementsByTagName() 通过标签名获取元素,返回类数组，如果类名不存在返回空数组[]
 	* 元素节点均可调用
 4. document.getElementsByName() 通过name获取元素,返回类数组，如果类名不存在返回空数组[]
	 *必须通过document调用
```

##### 如何获取body及html：

document.body获取body

document.documentElement获取html

#### 2.利用节点关系，获取所有节点

```
获取父级节点
	ele.parentNode 得到节点的父节点   
获取子节点
    ele.childNodes 得到ele元素的全部子节点列表（类数组） 
    ele.firstChild 获得ele元素的第一个子节点    
    ele.lastChild 获得ele元素的最后一个子节点   
获取兄弟节点
    ele.nextSibling 获得节点的下一个兄弟节点  
    ele.previousSibling 得到节点的上一个兄弟节点
```

//此时先插入知识点：节点的三个属性

#### 3.获取元素节点

##### 自行封装

    var Element = {
        getElement : function(nodes){
            var eleNodes = [];
            for(var i=0;i<nodes.length;i++){
                if(nodes[i].nodeType == 1){
                    eleNodes.push(nodes[i]);
                }
            }
            return eleNodes;//数组:所有的元素节点
        },
        getChildren: function(node){
            var childnodes = node.childNodes;
            return Element.getElement(childnodes);
        },
    
        getNextElement:function(){
            // 自己写
        },
        getPrevElement:function(){
    	}
    }


##### 利用元素关系，获取元素节点

```
获取父级节点元素
	parentElement 
获取子级元素节点
    children 获取元素的全部子元素
    firstElementChild 获取第一个子元素
    lastElementChild 获取最后一个子元素
获取兄弟元素节点
    previousElementSibling 获取前一个元素
    nextElementSibling 获取下一个元素
```

### 节点的三个属性

- nodeType  返回节点的类型。
  - 元素节点 <==> 1
  - 文本节点 <==> 3
  - 属性节点 <==> 2
- nodeName  返回节点的名称，根据其类型。
  - 元素节点 <==>  标签名字大写
  - 文本节点 <==>  #text
  - 属性节点 <==> 属性名
- nodeValue 返回节点的值（元素节点的nodeValue为null）
  - 元素节点 <==>  null
  - 文本节点 <==>  文本内容
  - 属性节点 <==> 属性值

### 节点的创建与插入方法

```
创建：
	document.createElement() 创建一个元素节点
	document.createTextNode() 创建一个文本节点(了解)
	document.createAttribute() 创建一个属性节点（了解）
插入：		
	parent.appendChild()  向节点的子节点列表的结尾添加新的子节点
	parent.insertBefore(new,node)  在指定的子节点node前插入新的子节点new。
	ele.setAttributeNode(attrNode) 在指定元素中插入一个属性节点（了解）
//以上parent表示父级元素，ele表示元素
```

##### 演示：利用创建节点方法，生成三种节点，并往元素节点插入内容及属性

```
//1.创建元素节点
var h3 = document.createElement("h3");
//2.创建文本节点，并往h3元素节点插入文本节点
var txt = document.createTextNode("啦啦啦");
h3.appendChild(txt);
//常用方法 =====> h3.innerHTML = "啦啦啦"；
//3.创建属性节点，并往h3元素节点插入属性节点
var title = document.createAttribute("title");
title.nodeValue = 'xxx';
h3.setAttributeNode(title)；
//常用方法 =====> h3.title = "xxx"
//4.将元素节点插入body里面
document.body.appendChild(h1);
```

##### 演示：insertBefore(),往子节点前面插入一个新节点 

	var h2 = document.createElement('h2');
	h2.innerHTML = '哈哈哈';
	document.body.insertBefore(h2,h3);

##### 案例:生成表格

	//1.获取文本框、按钮、及输出元素
	//2.封装表格函数（保证单一原则,即一个函数只做一件事）
		function createTable(r,c){
	        //2.1 创建table>tbody>tr>td,记得插入对应的父元素内
	        //2.2 将table返回出去
		}
	//3.点击按钮时
		//2.1 获取行列值
		//2.2 执行生成表格函数
		//2.3 将表格插入输出元素
		//2.4 注意事项：每次插入前，记得清空上一次插入的内容
			output.innerHTML = '';
			output.appendChild(table);

##### 案例：自动应答机器人

	// 1.获取输出元素ul、输入框及按钮
	// 3.设置应答消息，用数组方式存放
		var q = '你好,在吗,约吗,十年了还约吗'.split(',');
		var a = '他好我也好,不是本人,叔叔在哪约,滚'.split(',');
	// 2.点击按钮，发送信息（.active）
	btn.onclick = function(){
		//2.1 发送信息（即在ul内插入li，并设置类名）：
		//2.2 发送自动应答信息（设置延迟n秒，setTimeout）
	    // 注意：利用发送信息在q数组的索引，获取a数组中自动回复的内容
	    var idx = q.indexOf(_msg);
	    if(idx >= 0){
	    	aLi.innerHTML = a[idx];
	    }else{	
	    	aLi.innerHTML = '你说什么？风太大我听不到';
	    }	
	    //2.3 清空内容，自动聚焦
	    msg.value = '';
	    msg.focus();
	}   

### 节点的复制、删除、判断方法

```
复制:
	当前节点.cloneNode(boolean)  复制节点，为true是深复制。返回值为复制后的新节点
删除：
	父节点.removeChild(ele)  删除（并返回）当前节点parent的指定子节点ele。
判断：
	父节点.hasChildNodes() 判断当前节点是否拥有子节点,返回布尔值
```

##### 案例：表格删除、复制表格行

```
*添加行号
*添加删除按钮
*删除当前行（一定要书写在生成表格后）
	*获取所有按钮
	*分别给所有按钮绑定事件处理函数
for(var i=0;i<btnDels.length;i++){
	btnDels[i].onclick = function(){
		//console.log(i)//演示：不管点击哪个按钮，最终都会得到10（变量查找规则）
		//利用当前对象this，删除当前行tr、
	}
	//*添加复制功能，复制行
	btnCopies[i].onclick = function(){
        // 获取当前行
        var currentTr = this.parentNode.parentNode;
        // 复制tr
        var copyTr = currentTr.cloneNode(true);
        // ！！！问题：复制的tr没有事件，为什么，怎么解决？
        table.getElementsByTagName('tbody')[0].appendChild(copyTr);
    }
}
```

##### 演示：

```
1.判断父节点是否存在子节点
2.对已存在的元素进行插入操作的后果
```

### 元素节点的操作

#### 操作的是DOM节点属性

> 可以通过点语法或方括号访问

- tagName 获取元素元素的标签名
- id 设置/获取元素id属性
- name 设置/获取元素name属性
- style 设置/获取元素的内联样式
- className 设置/获取元素的class属性
- innerHTML 设置/获取元素的内容（包含html代码）
- outerHTML 设置或获取元素及其内容（包含html代码）
- innerText 设置或获取位于元素标签内的文本
- outerText 设置(包括标签)或获取(不包括标签)元素的文本

#### 操作html结构属性:

- ele.getAttribute(attr) //获取元素的属性值（自定义属性获取）
- ele.setAttribute(attr,val); //设置元素的属性
- ele.removeAttribute(attr) //删除属性attr
- ele.hasAttribute(attr) //判断是否存在属性attr

#### 演示：DOM节点及html元素的属性的关联及区别

```
<div id="box"></div>
<script>
	var box = document.getElementById('box');
	//(一)操作标准属性
	//1.操作dom节点属性，会影响到html元素。可通过控制台查看元素，多了[class]。
	box.className = 'mybox'; 
	//2.操作html元素属性，会影响到dom节点。可以通过ele.id查看对应的值。
	box.setAttribute("title","divBox");
	//（二）操作非标准属性
	box.man = 'lemon'; //不互相影响，可通过控制台查看元素，并没有任何改变
	box.setAttribute("love","you");//不互相影响，通过ele.love会发现值为undefined
</script>
```

#### 盒模型相关

```
offsetTop: 当前元素离<定位父级>元素顶部的距离。
offsetLeft: 当前元素离<定位父级>元素左边的距离。
	以上两个属性如果没定位的父级，则相对于根元素html的距离
offsetWidth: 当前元素的宽度（border + padding + content）
offsetHeight: 当前元素的高度（border + padding + content）
```

#### 获取css样式（非内联样式）

```
window.getComputedStyle(ele,pseudo) （标准）
	ele:要获取样式的元素
	pseudo:伪元素样式字符(可选)，可获取伪元素样式
ele.currentStyle （IE8-）
//注意事项：ie浏览器都不能直接获取css总属性的值
```

###### 封装获取css样式的方法

```
function getCss(ele,key){
	// 思路：判断浏览器是否支持这个方法
	if(window.getComputedStyle){
		// 标准浏览器
		return getComputedStyle(ele)[key]
	}else if(ele.currentStyle){
		// IE8-
		return ele.currentStyle[key]
	}else{
		// 
		return ele.style[key]
	}
}
```

案例：tab切换

```
1）初始化
* 高亮第一个tab
* 隐藏除第一张以外的图片
* 给每个tab添加idx保存索引
2）切换：鼠标点击tab（关键：获取点击的index值，利用this.idx）
* 高亮显示当前tab,去除其他高亮
* 切换相应的图片，隐藏其他图片
```

