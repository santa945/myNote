# 前端面试题

> https://coding.imooc.com/class/562.html

[TOC]



## 一、数据结构和算法

> https://github.com/santa945/algorithm

## 二、前端基础知识

#### Ajax-Fetch-Axios三者有什么区别

```markdown
三者都用于网络请求，但是不同维度
	* Ajax ( Asynchronous Javascript and xML), 一种技术统称
 	* Fetch 一个具体的浏览器原生API，和XMLHttpRequest是一个级别的，更加简洁，支持Promise
 	* Axios，随着vue而火爆起来的第三方库 https://axios-http.com/
```

```js
// 使用XMLHttpRequest实现Ajax
function ajax1(urlsuccessFn){
	const xhr = new XMLHttpRequest();
  xhr.open("GET",url,false);
	xhr.onreadystatechange = function(){
    if(xhr.readyState==4){
			if (xhr.status == 200) {
				successFn(xhr.responseText)
      }
    }
  }
	xhr.send(null)
}
// 使用fetch实现Ajax
function ajax2(url) {
	return fetch(url).then(res=> res.json())
}
```



#### 防抖和节流有什么区别，分别用于什么场景-防抖

* 防抖：控制函数触发的时机，当停顿达到设定的时间时，才触发一次，和坐电梯一样
  * 例如，一个搜索输入框，等输入停止之后，再触发搜索
  * 理解：你进电梯，如果两秒内外面一直有人按电梯，电梯就不会上楼，两秒内按一百次都电梯都不上楼，如果2秒内没人按，两秒后电梯就上楼
* 节流：控制函数触发的频率，在设定的时间内只触发一次函数，和坐公交一样
  * 例如：在拖拽过程中需要频繁触发函数，这里控制每100ms只触发一次
  * 理解：公交车不是每分每秒都到站的，而是20分钟到一次，这20分钟内只接收一次乘客，所以车到站到下一次车到站的时间就是设定时间，这时间内只接收一次乘客
* 区别：鼠标快速抖动1分钟，打印鼠标位置，防抖和节流都是设置1秒钟，那防抖只会触发一次打印（只在1分钟后才停下超过1秒钟），而节流会触发60次（1分钟内有60个1秒钟，每1秒打印一次）

```js
// 防抖函数
const debounce = (fn, delay = 200) => {
  let timer = 0;
  return function () {
    if (timer) clearTimeout(timer) // 刚才的清除掉，重新来过
    timer = setTimeout(() => {
      fn.apply(this, arguments)
      timer = 0;
    }, delay);
  }
}

// 节流函数
const throttle = (fn, delay = 100) => {
  let timer = 0;
  return function () {
    if (timer) return; // 这次的不要了，等刚才的执行完
    timer = setTimeout(() => {
      fn.apply(this, arguments)
      timer = 0;
    }, delay);
  }
}
```

#### px/%/em/rem/vw-vh有什么区别

```markdown
* px: 绝对单位，其他的都是相对单位
* %: 相对于父元素的宽度比例
* em: 相对于当前元素的`font-size`
* rem: 相对于根节点的`font-size`(rem=> root的em)
* vw/vh: 屏幕宽度/高度的1%；（vmin/vmax: 两者中的最小/最大值，在横屏和竖屏中）
```

```html
<!-- em的解释与使用场景 -->
<div class="dad" style="font-size:20px;">
	<p style="text-ident:2em; font-size:40px;">首行缩进(2em=40px*2=80px;)</p>
  <p style="tex-ident: 2em;">首行缩进(2em=20px*2=40px)</p>
</div>
```



#### 什么时候不能使用箭头函数

```markdown
* 没有`arguments`;
* 无法通过`call``apply``bind`来改变this
* 某些省略return的写法很难阅读`const fn = (a,b)=> b === undefined ? b=>a*b : a*b`
* 不适用于对象里的方法(也不适用于原型方法)
* 箭头函数不能当作构造函数
* 不适用于vue的生命周期和methods（vue实例本身是个对象，用箭头函数里面的this指向有问题）
	* React的class组件可以，因为不是个对象，而是个class
```

```js
function fn1() {
  console.log(this); // {x: 1}
  console.log(arguments) // [100,200]
}
const fn2 = () => {
  console.log(this); // window
  console.log(arguments); // arguments is not defined
}

fn1(100, 200)
fn2(100, 200)

fn1.call({ x: 1 })
fn2.call({ x: 1 })
// ----------------------------------------------------
const obj = {
  name: 'santa',
  getName: function () {
    return this.name
  }
}
console.log(obj.getName()); // santa
const obj2 = {
  name: 'santa',
  getName: () => {
    return this.name
  }
}
console.log(obj2.getName()); // 空字符串
class Man {
  constructor(name, city) {
    this.name = name;
    this.city = city;
  }

  getName = () => {
    return this.name;
  }
}
const haha = new Man('santa')
console.log(haha.getName()); // santa
//-----------------------------------------------
const Man = function (name, year) {
  this.name = name
  this.year = year
}
const santa = new Man('santa', 99)
console.log(santa); // 正常，是个对象

const Student = (name, year) => {
  this.name = name
  this.year = year
}
const joy = new Student('santa', 99)
console.log(joy); // 报错，Student is not a constructor
```



#### 请描述 TCP 三次握手和四次挥手

```markdown
* 三次握手- 建立连接
	1. Client发包，Server接收。Server: 有Client要找我
	2. Server发包，Client接收。Client: Server已经收到信息了
	3. Client发包，Server接收。Server: Client要准备发送了	
* 数据发送/接收的过程
* 四次挥手-关闭连接
	1. Client发包，Server接收。Server: Client已请求结束
	2. Server发包，Client接收。Client: Server已收到，我等待它关闭(中间可能还有正在返回的数据)
	3. Server发包，Client接收。Client: Server此时可以关闭连接了(所有数据都返回了)
	4. Client发包，Server接收。Server: 可以关闭了 然后关闭连接
```



#### for-in和for-of有什么区别

* `for in`用于**可枚举**的数据，如对象，数组，字符串（属性特性中`enumber`为true）
  * `Object.getOwnPropertyDescriptors({a:1})`可以看到a的`enumerable`为true
* `for of`用于**可迭代**的数据，如数组，字符串，Map，Set（有`Symbol.iterator`,里面有`next`方法）
  * `[1,2,3][Symbol.iterator]()`可以看到原型里有`next`方法

```markdown
* `for in`遍历key, `for of`遍历value
* `for in`可以遍历对象，`for of`不可以遍历对象
* `for of`可以遍历`Map``Set``generator`, `for in`不可以遍历`Map``Set``generator`
```

```js
const obj = {
  name: 'santa',
  year: 99
}
const set1 = new Set([100, 200, 300, '444'])
const map1 = new Map([['x', 200], ['y', 300]])
function* generator1() {
  yield 10;
  yield 20;
  yield 30
}

// Map Set generator不能用for in----------------
for (const key in obj) {
  console.log(key); // 正常打印
}
for (const key in set1) {
  console.log(key); // 不报错，也不打印
}
for (const key in map1) {
  console.log(key); // 不报错，也不打印
}
for (const key in generator1()) {
  console.log(key); // 不报错，也不打印
}
// 对象不能用for of ------------------------------
for (const val of set1) {
  console.log(val); // 正常打印
}
for (const val of map1) {
  console.log(val); // 正常打印
}
for (const val of generator1()) {
  console.log(val); // 正常打印
}
for (const val of obj) {
  console.log(val); // obj is not iterable
}
```



#### for-await-of有什么作用

* 相当于`promise.all`的效果
* 用在promise有很多的情况，要打印出值而不是promise对象

```js
const createPromise = (val) => {
  return new Promise((resolve) => {
    setTimeout(() => {
      resolve(val)
    }, 100);
  })
}

(async function () {
  const p1 = createPromise(100)
  const p2 = createPromise(200)
  const p3 = createPromise(300)

  const res1 = await p1
  console.log(res1);
  const res2 = await p2
  console.log(res2);
  const res3 = await p3
  console.log(res3);

  const list = [p1, p2, p3]
  Promise.all(list).then(res => {
    console.log(res);
  })

  for (const val of list) {
    console.log(val); // 会打印出Promise对象
  }
  for await (const val of list) {
    console.log(val); // 正常打印
  }
})()
```



#### offsetHeight / scrollHeight / clientHeight有什么区别

* 计算规则
  * **offsetHeight/offsetWidth**: border + padding+content
  * **clientHeight/clientWidth**: padding+content
  * **scrollHeight/scrollWidth**: padding+实际内容尺寸(子元素更大时就是子元素尺寸)

#### HTMLCollection和NodeList有什么区别

* `HTMLCollection`是Element的集合，`NodeList`是Node的集合
* 获取`Node`和获取`Element`返回结果可能不一样，如`dom.children`与`dom.childNodes`
* `Node`包含`Text`和`Comment`节点，`Element`不包含
* HTMLCollection和NodeList都不是数组，是类数组
  * 把类数组变成数组的方式
    * const arr = Array.from(NodeList)
    *  const arr = Array.prototype.slice.call(NodeList)
    * const arr = [...NodeList]

```html
<body>
    <div id="div1"><b>加粗</b>VS<em>em标签</em><i>i标签</i><!-- 注释 -->
    </div>
    <script>
        const dom = document.getElementById('div1')
        console.log(dom.children); // HTMLCollection[b,em,i]
        console.log(dom.childNodes); // NodeList[b, text, em, i, comment, text]
        console.log(dom.tagName);
        console.log(dom.nodeName);
      
        // Node 和 Element
        // DOM是一棵树，所有节点都是Node
        // Node是Element的基类
        // Element是其他HTML元素的基类，如HTMLDivElement
        class Node {}
        // document
        class Document extends Node {}
        class DocumentFragment extends Node {}

        // 文本和注释
        class CharacterData extends Node {}
        class Comment extends CharacterData {}
        class Text extends CharacterData {}
        // elem
        class Element extends Node{}
        class HTMLElement extends Element {}
        class HTMLDivElement extends HTMLElement {}
        class HTMLInputElement extends HTMLElement{}
        // ......
    </script>
</body>
```

#### Vue中computed和watch有什么区别

* computed是根据data或者computed中的其他属性来通过计算返回一个新的数据（多个数据影响一个）
* watch是监听现有数据的变化，来判断要做什么事（一个数据影响多个）
* computed和method相比，computed是有缓存的，影响因素不变就不会重新计算，method是触发就重新计算


#### Vue组件通讯有几种方式

* props/$emits自定义事件
* $attrs/$parents
* $refs
* Provide/inject
* eventbus
* Vuex


#### Vuex中action和mutation有什么区别

* Mutation: 原子操作，必须是同步的代码
* Action: 可包含异步操作，可以包含多个mutation


#### JS严格模式有什么特点

* 全局变量必须先声明
* 禁止使用with（会变得难以阅读）
* 创建eval作用域
* 禁止this指向window
* 函数形参不能重名

```js
'use strict'
n = 10 // n is not defined
const obj = {
  x: 100,
  y: 200
}
with (obj) {
  console.log(x); //Strict mode code may not include a with statement
}
var num = 10
eval(`var num = 20; console.log('eval', num)`)
console.log(num); // 不会被eval里的nun=20污染掉

function fn(x, x, y) { // Duplicate parameter name not allowed in this context
  console.log('this', this); // undefined，无法指向window
}
fn(10, 10, 20)
```

#### HTTP跨域时为何要发送options请求

* options请求，是跨域请求之前的预检查，用于询问支持的请求类型
* 浏览器自行发起的，无需我们干预
* 不会影响实际的功能

## 三、知识深度 - 原理和源码

#### JS内存垃圾回收用什么算法

> 什么是垃圾回收？函数执行完了后，那些用不到的对象和数据
>
> 什么是内存泄露？非预期的数据，垃圾回收不了

* 引用计数法（之前）
  * 看自己有没有被引用，缺陷是循环引用清除不了
* 标记消除法（现在）
  * 从根(window)上开始遍历，看能不能得到它，得不到，就清除

```js
// 引用计数------------------------------------
let obj = { a: 100 } // 对象被obj引用1次
let a1 = obj;// 对象被obj和a1各引用1次
obj = '' // 对象被a1引用1次
a1 = null // 对象没有被任何变量引用

// 缺陷
function fn() {
  const a = {}
  const b = {}
  b.a = a
  a.b = b
}
fn();// 执行完后a和b一直被引用，没法清除
// 标记清除-------------------------------------
function fn2() {
  const a = {}
  const b = {}
  b.a = a
  a.b = b
}
fn2();// 执行完后从window开始找，a和b两个变量被消除了，所以跟在后面的{}也找不到了
```

#### JS闭包是内存泄漏吗？

* 闭包不是内存泄漏，因为闭包里面的数据是要用的，属于预期数据，非预期的数据(不需要用的)才算内存泄露
* 闭包里的数据不能被回收（但不代表它是内存泄露，只不过占内存而已）


##### 如何检测JS内存泄漏？

* 使用浏览器中的`Performance`, 勾选`Memory`,按左上角record（黑色圆点），点击stop，查看堆(HEAP)的变化，正常状态是锯齿状(创建后销毁)，一开始高，然后垃圾回收后变低；而内存泄漏会一直变高，堆(HEAP)里越来越多

##### JS内存泄漏的场景有哪些

* vue
  * 被全局变量、函数引用，组件销毁时未清除
  * 被全局事件、定时器引用，组件销毁时未清除 
  * 被自定义事件引用，组件销消毁时未清除

```vue
<script>
  export default {
    name: 'Memory Leak Demo',
    data() {
      return {
        arr: [1, 2, 3],
        intervalId: 0
      }
    },
    methods: {
      printArr() {
        console.log(this.arr);
      }
    },
    mounted() {
      // 被全局变量引用
      window.arr = this.arr;
      // 被全局函数引用
      window.printArr = () => {
        console.log(this.arr)
      }
      // 被全局事件引用(回调函数提到methods，不能直接把匿名函数塞进去，因为到时候需要把同个函数塞进removeEventListener)
      window.addEventListener('resize', this.printArr)
      // 被全局定时器引用
      this.intervalId = setInterval(() => {
        console.log(this.arr);
      }, 100);
    },
    // 在 beforeDestroy 或 beforeUnmount中清楚，否则会一直挂在全局window上
    beforeUnmount() {
      window.arr = null;
      window.printArr = null;
      window.removeEventListener('resize', this.printArr);
      if (this.intervalId) {
        clearInterval(this.intervalId);
      }
    }
  }
</script>
```



#### WeakMap和WeakSet

* 弱引用的关系，我引用你，但是如果你销毁了，我也一样找不到
* 不影响垃圾回收，只要外部的引用消失，所对应的键值对就会自动被垃圾回收清除
* 使用场景：
  * 关联dom的元数据，节点从DOM树中被删除了，引用解除
  * 自己写的深拷贝函数，数据缓存，又不想管理缓存
  * 部署私有属性（实例销毁，引用解除）

```js
const map = new Map()
function fn1() {
  const obj = { x: 100 } // 执行结束后，对象{x: 100}会被留着，因为外面的Map引用着
  map.set('a', obj);
}
fn1();
console.log(map.get('a')); // {x: 100}，就是刚刚的那个对象

const wMap = new WeakMap()// 弱引用 
function fn2() {
  const obj = { x: 100 } // 执行结束了，这个对象会被销毁，外部的是弱引用
  wMap.set(obj, 100)// weakMap 的 key只能是引用类型
}
fn2();
console.log(wMap.get({ x: 100 })); // undefined, 这里的{ x: 100 }已经不是刚才set进去的那个了，key都找不到了
```



#### 浏览器和nodejs事件循环（Event Loop）有什么区别？

> 两者基本上是相同的，只不过nodejs给宏任务和微任务分了不同的类型和优先级，而浏览器的宏任务微任务没有优先级

* **浏览器的Event Loop**（执行宏任务后，先清空微任务队列，再渲染，然后从宏任务队列取出一个，重复操作）
  * 从`<script>`标签(也算一个宏任务)开始同步执行函数，当遇到宏任务(如`setTimeOut`)则先推进宏任务队列，当遇到微任务(如`Promise`的then)，则推进微任务队列
  * 当前宏任务执行结束，查看微任务队列是否有待执行的，有，则一个个执行直至清空，然后执行渲染；
  * 当微任务队列清空后，查看宏任务队列是否有待执行的，有，则取出一个宏任务，执行，执行过程中，如果有遇到其他宏任务或微任务，按上述操作继续推进宏任务队列或微任务队列
* **nodejs的Event Loop**(分不同的类型，不同的优先级来执行)
  * 先执行同步代码
  * 执行微任务（其中process.nextTick优先级更高，队列中放在普通微任务前面）
  * 按类型执行6个类型的宏任务
    1. timer：`setTimerOut`和`setInterval`
    2. I/0 callback:  处理网络，流，TCP的错误回调
    3. Idle prepare：闲置状态（nodejs内部使用）
    4. Poll轮询：执行Poll中的I/O队列
    5. check检查：存储`setImmediate`回调
    6. close callbacks:  关闭回调，如`socket.on('close')`

```js
console.info('start') // 1
setImmediate(() => {
	console.info('setImmediate') // 6
})
setTimeout(() => {
	console.info('timeout') // 5
})
Promise.resolve().then(() => {
	console.info('promise then') // 4
})
process.nextTick(() => {
	console.info('nextTick') // 3
})I
console.info('end') // 2
// nodejs的执行顺序['start','end','nextTick','promise then','timeout','setImmediate']
```



#### 虚拟DOM（vdom）真的很快吗

* vdom并不快，JS直接操作 DOM才是最快的 
* 但“数据驱动视图”要有合适的技术方案，不能全部DOM重建，diff算法会部分dom重建，在整个开发链条来说很快
* vdom就是目前最合适的技术方案(并不是因为它快，而是合适)

#### 遍历一个数组用for和forEach哪个更快(JS底层原理问题)

* for会比forEach更快
* forEach每次都要创建一个函数来调用，而for不会创建函数
* 函数会有独立的作用域，会有额外的开销

```js
const arr = []
for (let i = 0; i<100 * 10000; i++) {
	arr.push(i)
}
const length =arr.length;

console.time('for) let n1 =0
for (let i = 0; i<length; i++) {
	n1++
}
console.timeEnd(for)//3.7ms

console.time('forEach') 
let n2 =0
arr.forEach(() => n2++) I 
console.timeEnd("forEach)//15.1ms
```

#### nodejs如何开启多进程，进程如何通讯

##### 进程和线程的区别

* 进程(process)，OS进行**资源分配**和调度的最小单位，有独立内存空间（计算机会分配一个堆）
  * 例如：一个抖音的进程，一个微信的进程，两个进程的内存中的堆和栈不共用
  * 默认独立，想要交换数据，就得想办法通讯
* 线程(thread)，OS进行**运算调度**的最小单位，共享进程内存空间
  * 例如：同一个资源做并发的计算
* js是单线程的，但是可以开启多进程执行，如WebWorker
  * 什么叫单线程呢，就是一个进程里(js进程)里只有一个线程来计算
  * 假设js是进程p4，里面只有一个线程t1，现在使用WebWorker来开启多进程，实际上是开启了进程p5，里面还是只有一个线程t1，此时进行进程间的通讯，这就是js线程开启多进程的一个说法
* 扩展：java，c++是多线程的，所以在单例模式中需要考虑线程之间的互斥，而js就不需要考虑这么多

##### 为什么要开启多进程

* 现在的cpu大多是多核的，很适合处理多进程

* 单进程有内存上限（通常2G）
* 多进程内存更大，可以更好利用计算机的内存

##### nodejs如何开启多进程

* 使用child_process.fork方式

```js
// process-fork.js 主进程
const http = require('http')
const fork = require('child_process').fork

const server = http.createServer((req, res) => {
    if (req.url === '/get-sum') {
        console.info('主进程 id', process.pid)

        // 开启子进程
        const computeProcess = fork('./compute.js')
        computeProcess.send('开始计算')

        computeProcess.on('message', data => {
            console.info('主进程接受到的信息：', data)
            res.end('sum is ' + data)
        })

        computeProcess.on('close', () => {
            console.info('子进程因报错而退出')
            computeProcess.kill()
            res.end('error')
        })
    }
})
server.listen(3000, () => {
    console.info('localhost: 3000')
})

// compute.js 计算进程--------------------------------------------------
function getSum() {
    let sum = 0
    for (let i = 0; i < 10000; i++) {
        sum += i
    }
    return sum
}

process.on('message', data => {
    console.log('子进程 id', process.pid)
    console.log('子进程接受到的信息: ', data)

    const sum = getSum()

    // 发送消息给主进程
    process.send(sum)
})
```

  * 使用cluster.fork方式（集群）

```js
const http = require('http')
const cpuCoreLength = require('os').cpus().length
const cluster = require('cluster')

if (cluster.isMaster) {
    for (let i = 0; i < cpuCoreLength; i++) {
        cluster.fork() // 开启子进程
    }

    cluster.on('exit', worker => {
        console.log('子进程退出')
        cluster.fork() // 进程守护
    })
} else {
    // 多个子进程会共享一个 TCP 连接，提供一份网络服务
    const server = http.createServer((req, res) => {
        res.writeHead(200)
        res.end('done')
    })
    server.listen(3000)
}
```



#### 请描述js-bridge的实现原理

##### 什么是 JS Bridge

* JS 无法直接调用nativeAPI
* 需要通过一些特定的“格式”来调用
* 这些“格式”就统称JS-Bridge，例如: 微信JSSDK

##### 实现方式

* 注册全局API：不适合异步
* URL Scheme：制造一个协议标准，参考【微信Scheme】
  * 例如：`chrome://version/` 可以直接查看版本
  * 例如：`chrome://dino/`可以玩一个chrome小游戏

#### requestIdleCallback和requestAnimationFrame有什么区别

* requestAnimationFrame每次渲染完都会执行，高优
* requestIdleCallback空闲时才会执行，低优
* 两者都是宏任务，都是要等待dom渲染完后才执行

#### Vue每个生命周期都做了什么

* beforeCreate
  
  * 创建一个空白的Vue实例，data、methods还没初始化，不能使用
* created
  * Vue实例初始化完成，完成了响应式绑定
  * data和methods初始化完成，可调用
  * 尚未开始渲染模版
* beforeMount
  * 编译模版，调用render生成vdom
  * 还没开始渲染DOM
* mounted
  * 完成DOM的渲染
  * 组件创建完成
  * 开始由**创建阶段** 进入**运行阶段**
  * Ajax适合在这个位置发起，虽然也可以在created发起，但是created到mounted时间间隔很短，可能就10ms，这里可以拿到比较多的东西，放在created还会导致并行，如果使用`async created()`可能出现mounted已经执行结束了，created才执行的问题，不好追述
* beforeUpdate
  
  * data发生变化，准备更新DOM（只是准备，没有更新）
* updated
  * data发生变化，DOM更新完成
  * 不要在updated修改data，可能会导致死循环
* beforeUnmount
  * 组件进入销毁阶段（尚未销毁，还能使用）
  * 可移除解绑一些全局事件，自定义事件
* unmounted
  * 组件被销毁了
  * 所有子组件也被销毁了
* onActivated：缓存组件被激活（进入页面）
* onDeActivated: 缓存组件被隐藏（离开页面）

#### Vue在什么时候操作DOM比较合适

* mounted和updated都不能保证子组件全部挂载完成
* 使用`$nextTick`渲染DOM

```js
mounted() {
  this.$nextTick(()=>{
    // 仅在整个视图都被渲染完成之后才会执行的代码
  })
}
```

#### Vue3的Composition API生命周期有何区别

* 使用`setup`代替了beforeCreate和created
* 使用Hocks函数形式，例如mounted改为了onMounted()

#### Vue2和Vue3和React三者的diff 算法有什么区别

* React diff：仅右移
* Vue2 diff：双端比较（头头，头尾，尾尾，尾头，四个指针比较，然后不断往中间移动直到相遇）
* Vue3 diff：最长递增子系列（例如: [3,5,7,1,2,8]的最长递增子系列为[3,5,7,8])，所以就是中间那块最长不变，其他的改变

#### Vue和React为何循环时必须使用key

* vdom diff算法会根据key判断是否要删除
* 匹配了key，只移动元素，性能较好
* 未匹配key，则要删除重建，性能较差

#### Vue-router的MemoryHistory是什么

##### Vue-router是三种模式

* Hash: 基于location.hash 来实现
  * url中有“#”号
  * hash值（“#”后的值）不会被包含在http请求中，改变hash值不会引起页面的重新加载。
  * hash改变会触发hashChange事件，会被浏览器记录下来，可以使用浏览器的前进和后退。
  * hash兼容到IE8以上
  * 会创建hashHistory对象，在访问不同的路由的时候,会发⽣两件事
    * HashHistory.push()将新的路由添加到浏览器访问的历史的栈顶
    * HasHistory.replace()替换到当前栈
* WebHistory：HTML5 提供了 History API 来实现 URL 的变化（history.pushState() 和 history.repalceState()）
  * url不带参数
  * history 兼容 IE10 以上
  * history 模式改变地址栏会重新发送请求引起页面重新加载
* MemoryHistory (Vue Router V4之前叫做 abstract history)： 地址栏什么都没有(使用localstorage存储)，适合非浏览器如app

## 四、知识广度 - 从前端到全栈

#### 移动端H5点击Click有300ms延迟，该如何解决

> 背景：用手机打开网页，发现文字太小了，可以双击放大，这时候点击第一次的时候不执行，等300ms之内如果有第二次点击，证明这是一次双击

* 初期解决方式：使用FastClick第三方库

  * 原理：监听`touchend`s事件，`touchstart`和`touchend`会优于click触发，这样在300ms到来之前就触发了，然后使用自定义DOM事件模拟click事件，把默认的click事件(会延迟300ms)给禁止掉

* 现代浏览器的改进：使用`viewport`配置`width=device-width`

  * width=device-width ：表示宽度是设备屏幕的宽度

  ```html
  /!-- 有了这个，手机就会认为你已经做了300ms延迟的适应 --/
  <meta name="viewport" content="width=device-width">
  ```

   

#### Retina 屏幕的 1px 像素，如何实现

> https://blog.csdn.net/sisteran/article/details/119529729

| 方案                   | 优点                   | 缺点                     |
| ---------------------- | ---------------------- | ------------------------ |
| 0.5px实现              | 代码简单，使用css即可  | IOS及Android老设备不支持 |
| border-image实现       | 兼容目前所有机型       | 修改颜色不方便           |
| viewport + rem 实现    | 一套代码，所有页面     | IOS及Android老设备不支持 |
| 伪元素 + transform实现 | 兼容所有机型           | 不支持圆角               |
| svg 实现               | 实现简单，可以实现圆角 | 需要学习 svg 语法        |



#### HTTP请求中token和cookie有什么区别

##### cookie

* HTTP是无状态的，每次请求都要携带cookie，以帮助识别身份
* 服务端可以向客户端`set-cookie`· ，大小限制为4kb
* 默认有跨域限制，cookie不能跨域共享或跨域传递
  * 共享：同个页面里iframe一个网页，cookie不能共享
  * 传递：8081端口的网页请求8083的接口时，不会携带cookie给服务端（解决方法：前端在axios配置**withCredentails**，后端也需要配置）
* 现代浏览器开始禁止第三方js设置cookie（内嵌广告a.com如果可以设置cookie，那下次用户登陆a.com就可以用cookie分析用户习惯了）

##### session

* 存储在服务端，存储用户详细信息，和cookie信息一一对应
* cookie+seccion是常见的登录验证解决方案

##### token（JWT）

* 前端发起登录，后端验证成功之后，返回一个加密的token
* 前端自行存储这个token (其中包含了用户信息，加密了) 
* 以后访问服务端接口，都带着这个token，作为用户信息，由后端解密

##### 区别

* cookie是HTTP标准，有跨域限制，需要配合session使用，cookie只存了一个key，体积小，真实信息存在服务端的session里
* token无标准，无跨域限制，包含用户的全部信息，直接发给服务端全部信息，由服务端解密

#### session和JWT哪个更好

* session是统一存储用户信息，因此可以快速封禁其中的用户
* session存储在进程中，因为存在硬盘disk中不够快，所以存在进程中数据不共享问题，这时候需要引用第三方缓存**redis**
* session默认有跨域限制
* JWT所有信息都在token中，所以不占用服务器内存
* JWT多进程多服务器不受影响，因为都是浏览器把所有信息带给他的
* JWT没有跨域限制
* JWT信息存储在客户端，无法快速封禁其中的用户（建立黑名单来封禁）
* 万一服务器密钥泄漏，则用户信息全部丢失

##### 答案

* 如果有严格管理用户信息的要求，则使用session
* 如没有特殊要求，则使用JWT（创业初期的网站使用JWT，减少很多服务端的存储和成本）

#### 如何实现SSO单点登录

##### cookie共享

* cookie默认不可跨域共享，但有些情况可设置共享
  * 主域名一致时（如music.baidu.com和image.baidu.com）,可设置cookie domain为`.baidu.com`，即可共享cookie

##### sso（第三方独立的，统一身份认证）

* 当主域名不一致时，则需要使用sso的方式
* 过程如下：
  * 访问a.com时发现没有登录，此时重定向到sso登录页，登录后sso页面签发凭证给客户端，客户端存储token
  * 客户端拿到sso返回的凭证，根据凭证去访问a.com，a.com询问sso凭证是否有效，有效则访问成功
  * 之后，客户端访问b.com，此时也会带上之前访问a.com签发的凭证，b.com直接根据带过来的凭证去询问sso是否有效，有效则访问成功
  * 如上，访问b.com时就是不需要进入登录页面的，同时询问sso该凭证是否有效通常是定时询问，大约5分钟询问一次

##### OAuth 2.0

* 例如：通过微博、微信登录的方式

* 过程如下：
  * 首先打开a.com，页面提示可以微信扫码登录
  * 微信扫码登录后，返回了一个token
  * 再次访问，此时携带token访问a.com，则a.com拿着token去询问微信，是否登录
  * 如上：和sso相似

#### HTTP协议和UDP协议有什么区别

* HTTP协议在应用层
* TCP和UDP协议在传输层
* TCP协议有传输（三次握手），有断开（四次挥手），稳定传输
* UDP协议无传输，无断开，不稳定传输，但效率高（如视频会议，语音通话，不管对面能不能收到，会不会断开，有时候不稳定就断开了）
* UDP协议对于前端这种以js为主的来说，不常见，视频会议，语音通话这种通常需要native

#### HTTP协议1.0和1.1和2.0有什么区别

* HTTP1.0
  * 最基础的HTTP协议
  * 只实现了GET和POST请求
* HTTP1.1（目前最常用的）
  * 支持缓存策略`cache-control`和`E-tag`等
  * 支持长连接`Connection: keep-alive`，一次TCP连接多次请求
    * 关掉的话，每次请求都要经历三次握手四次挥手
    * 有个时间设定，例如5秒内的http请求都用同一个tcp协议
  * 支持断点续传，状态码206
  * 支持新的方法PUT和DELETE等，可用于 Resful API
* HTTP2.0（最新）
  * 可压缩header，减少体积
  * 多路复用，一次TCP连接可以支持多个HTTP**并发**请求
  * 服务端推送（用得少，基本上用的是websoket）

#### 什么是HTTPS中间人攻击，如何预防
##### 背景：

* 对称加密：有个密钥，加密和解密都是使用同一个密钥，那么密钥存在服务端，需要传递给客户端，不安全也没意义

* 非对称加密：客户端和服务端都有各自的公钥和私钥，公钥加密，私钥解密，客户端将公钥a传递给服务端，服务端使用客户端传递过来的公钥a加密后传递给客户端，客户端使用私钥解密，同理，服务端也将自己的公钥b传递给客户端，客户端使用公钥b加密后将数据传递给服务端，服务端自己用私钥解密

* https同时使用了对称加密和非对称加密，既简洁又安全，还快速

  * 首先服务端存了公钥和私钥，将公钥传递给服务端
  * 客户端取出公钥，生成一个随机码key，使用公钥对随机码进行加密
  * 客户端将加密过的随机码key发送给服务端，作为接下来对称加密的密钥
  * 服务端接受到加密后的随机码key，之后使用随机码key对数据进行加密，传递给客户端

  * 以上步骤，前面部分是非对称加密，比较安全，后面部分是对称加密，比较简洁快速

##### 什么是中间人攻击（黑客伪造证书）

* 在https传输的前半部分，传递公钥的过程中，黑客将公钥替换成自己的，然后等待客户端使用假的公钥加密后，将数据传输给黑客，黑客用自己的私钥解密，从而获得客户端发送的随机码key

##### 如何预防

* CA数字证书，数字证书包含很多信息，包括颁发机构信息，域名等，证书的颁发机构通常和浏览器有合作，浏览器能认识这些证书并且会解析证书，判断证书的合法性
* 如果证书是第三方的不正规的证书，浏览器会有https警告，可以选择通过或者不通过，当然，黑客攻击也会报警告，这就是人为日常维护中不能通过这些
* 所以预防的方法通常是，服务端使用正规的证书，客户端发现https警告时不让通过不去访问

#### script标签的defer和async有什么区别

* 正常情况下：遇到<script>标签，页面停止解析，等待js下载结束，执行这个js，执行结束后再接着解析html

* defer：延迟，意味着遇到js时，并行下载这个js，等待html解析结束后，在执行这个js（效果相当于把<script>都写在body后面，但是更快）
* async：异步，意味着遇到js时，并行下载这个js，等待这个js下载结束后，不管html有没有解析结束，都要立刻执行这个js，

#### prefetch和dns-prefetch分别是什么

* dns-prefetch: DNS预查询，和preconnect有关
  * preconnect：DNS预连接
  * 提前查询ip后提前连接，打开的速度更快一些
  * 为什么要做DNS查询？
    * ip是一串数字，不好记，如1234.567.89.23这种
    * ip不是不变的，例如百度，国内外，不同省份可能真实连接的ip不同，因为是一个集群，而且不同时间可能ip也不同
* prefetch：资源预获取，指资源在未来页面使用，空闲时加载，和preload有关
  
  * preload：资源在当前页面使用，要优先加载

```html
<head>
	<!-- preload 当前页面使用，优先加载-->
	<link rel="preload" href="style.css" as="style">
  <link rel="preload" href="main.js" as="script">
  
	<!-- prefetch 未来页面使用，空闲时加载-->
	<link rel="prefetch" href="other.js" as="script">
  
 	<!-- dns-prefetch: DNS预查询 --> 
  <link rel="dns-prefetch" href="https://fonts.gsta atic.com/"/>
  <!-- preconnect: DNS预连接 --> 
	<link rel="preconnect" h ref="https://fonts.gstati c.com/" crossorigin>
  
	<!-- 引用 css -->
	<link rel="stylesheet" href="style.css">
</head>
```



#### 前端攻击手段有哪些，该如何预防

##### XSS（跨站脚本攻击）

* 手段：黑客将js插入到网页中，渲染时执行js代码
* 预防：特殊字符替换，例如前端的富文本中将左右尖括号替换成实体字符`&lt;`和`&gt;`
* Vue的{{}}会自动防范xss攻击，不需要单独处理，除非你用v-html
* React的{}也一样，除非你用dangeriouslyInnerHTML

##### CSRF（跨站请求伪造）

* 手段：黑客诱导用户去访问另一个网站的接口，伪造请求
* CSRF 详细过程：例如打开了邮箱里的广告
  * 用户登录了A网站，有了cookie
  * 黑客诱导用户到B网站，并发起A网站的请求
  * A网站的API发现有cookie，认为是用户自己操作的
* 预防：严格的跨域限制+验证码机制
  * 服务端加限制，判断referer请求来源
  * 为cookie设置samesite，禁止跨域传递cookie
  * 关键接口加短信验证码

##### 点击劫持

* 手段：诱导界面上蒙一个透明的iframe，诱导用户点击
* 场景：用户登陆的A网站，存起来的cookie，之后打开B网站，这个B网站上面蒙了一层透明的A网站，写着免费领取优惠券之类的信息诱导用户点击，此时真正点击的是A网站的按钮，由于用户已经登录过A网站，所以可以成功
* 预防：让iframe不能跨域加载
  * 检查上层是不是自己的网站
  * 在response header中设置`X-Frame-Options: sameorign`,设置了这个后，这个网页不能被其他网站以iframe形式加载

```js
if (top.location.hostname !== self.location.hostname){
	alert("您正在访问不安全的页面，即将跳转到安全页面!")
	top.location.href = self.location.href 
}
```

##### DDos（分布书拒绝服务）

* 手段：分布式，大规模的流量访问，使服务器瘫痪【和前端有点不相干了】
* 预防：软件层不好做，需要硬件预防（如：阿里云WAF网站防火墙）

##### SQL注入

* 手段：和xss类似，用户提交内容写入SQL语句，破坏数据库
* 预防：和xss类似，处理输入内容，替换特殊字符

#### WebSocket和HTTP协议有什么区别

* Websocket: 
  * 长连接，支持端对端通讯，可以由服务端发起，主要用于聊天室，消息通知，直播间讨论区，多人协同等
  * 先要发起一个http请求，成功后再升级到websocket协议，再通讯（打开控制台ws栏，可以看到`Connection: Upgrade`, 状态吗101）
* 区别：
  * 协议名不一样：websocket是`ws://`，可以双端发起请求，而http是`http://`只能客户端发起
  *  websocket没有跨域限制
  * websocket通过`send`和`onmessage`通讯，http通过`req`和`res`
  * 实际项目推荐使用`socket.io`，api比较简洁 

* 聊天室的实际情况：
  * 在服务端建立一个set，将每个ws实例add进去，当要send的时候遍历这个set，除了自己以外，其他的都send

#### WebSocket和HTTP长轮询的区别

* HTTP长轮询：客户端发起请求，服务端等待，不会立即返回（客户端不发消息，服务端就没返回）
  
  * 注意处理timeout机制，timeout之后要重新发起请求
* WebSocket：客户端发起请求，服务端也可以发起请求 （就算客户端不发消息，服务端也可以主动发消息）

#### 从输入URL 到网页显示的完整过程

* 网络请求

  * DNS查询，获取到ip，然后建立TCP连接
  * 浏览器发起HTTP请求
  * 收到请求相应，得到HTML源代 码（字符串）

* 解析（字符串->结构化数据）

  > 解析过程会继续请求静态资源，如图片，css，js等（如果有强缓存【200 from memory cache 】，则不发请求）
  >
  > 解析过程很复杂，不知到css是来自<style>还是<link>，js是内嵌还是外链，img是base64还是外链，所以不用深究

  * HTML构建DOM树
  * CSS构建style tree
  * 两者结合形成render树

* 渲染（render tree绘制到页面）

  * 计算各个dom的尺寸和定位，最后绘制到页面
  * 遇到js可能会执行
  * 异步css和img可能会触发重新渲染

#### 网页重绘repaint和重排reflow有什么区别

* 重绘：
  * 元素的外观改变，如颜色，背景色
  * 元素的尺寸，定位不变，不会影响其他元素的位置
* 重排：
  
  * 重新计算尺寸和布局，可能会影响其他元素的位置
* 减少重排
  * 集中修改样式，一次修改比多次修改效果更好
  * 修改前先设置display:none,脱离文档流
  * 利用BFC特性，不影响其他元素位置
  * 频繁触发（resize scroll）使用防抖和节流
* BFC触发的条件
  * 根结点<html>
  * `float: left/right`
  * `overflow: auto/scroll/hidden`
  * `display: inline-block/table/table-cell`
  * `display: flex/grid`的直接子元素
  * `position: absolute/fixed`

#### 如何实现网页多标签tab通讯（浏览器开了多个tab，要通讯）

* 使用websocket通讯，成本大
* 同域的网页，可以使用localstorage通讯（可以使用`window.addEventListener('storage', e=>{console.log(e)})`）
* 使用**SharedWorker**通讯
  * shardworker是webworker的一种，可单独开启一个进程用于同域页面的通讯

#### 如何实现网页和iframe之间的通讯

* 使用postMessage通讯
* 注意跨域限制和判断

```js
// index.html
const domain = 不限制? '*' ：'http://123.com'
window.iframe1.contentWindow.postMessage('信息',domain)
window.addEventListener('message', callback)

// iframe.html
 window.parant.postMessage('子页面信息', '*')
window.addEventListener('message', callback)
```

#### 请描述koa2的洋葱圈模型

##### Koa1和Koa2的区别

* Koa1是使用generator、yield的模式。
* Koa2使用的是async/await + Promise的模式。

##### 什么是洋葱模型

* Koa的洋葱模型是以next()函数为分割点，先由外到内执行Request的逻辑，然后再由内到外执行Response的逻辑
* 这里的request的逻辑，我们可以理解为是next之前的内容，response的逻辑是next函数之后的内容
* 也可以说每一个中间件都有两次处理时机，核心原理主要是借助compose方法。

##### 为什么要使用洋葱模型

* 很多时候，在一个app里面有很多个中间件，有些中间件需要依赖其他中间件的结果，洋葱模型可以保证执行的顺序，如果没有洋葱模型，执行顺序可能出乎我们的预期。

### 五、实际工作经验 - 是否做过真实项目
#### H5页面如何进行首屏优化
#### 后端一次性返回10w条数据，你该如何渲染
#### 扩展：文字超出省略
#### 前端常用的设计模式和使用场景
#### 观察者模式和发布订阅模式的区别
#### 在实际工作中，你对Vue做过哪些优化
#### 你在使用Vue过程中遇到过哪些坑
#### 在实际工作中，你对React做过哪些优化-上集
#### 在实际工作中，你对React做过哪些优化-下集
#### 你在使用React时遇到过哪些坑
#### 如何统一监听Vue组件报错
#### 如何统一监听React组件报错
#### 如果一个H5很慢，如何排查性能问题-通过Chrome Performance分析
#### 如果一个H5很慢，如何排查性能问题-使用lighthouse分析
#### 工作中遇到过哪些项目难点，是如何解决的
#### 扩展：处理沟通冲突
## 六、编写高质量代码 - 正确，完整，清晰，鲁棒

#### 手写一个JS函数，实现数组扁平化Array Flatten
#### 手写一个JS函数，实现数组深度扁平化
#### 手写一个getType函数，获取详细的数据类型
#### new一个对象的过程是什么，手写代码表示
#### 深度优先遍历一个DOM树
#### 广度优先遍历一个DOM树
#### 深度优先遍历可以不用递归吗
#### 手写一个LazyMan，实现sleep机制
#### 手写curry函数，实现函数柯里化
#### instanceof原理是什么，请写代码表示
#### 手写函数bind功能
#### 手写函数call和apply功能
#### 手写EventBus自定义事件-包括on和once
#### 手写EventBus自定义事件-on和once分开存储
#### 手写EventBus自定义事件-单元测试
#### 用JS实现一个LRU缓存-分析数据结构特点，使用Map
#### 用JS实现一个LRU缓存-代码演示和单元测试
#### 不用Map实现LRU缓存-分析问题，使用双向链表
#### 不用Map实现LRU缓存-代码演示
#### 手写JS深拷贝-考虑各种数据类型和循环引用
#### 扩展补充：根据一个 DOM 树，写出一个虚拟 DOM 对象
#### 重点及注意事项总结

## 七、分析和解决问题的思路 - 可以独立解决问题

#### [1, 2, 3].map(parseInt)
#### 读代码-函数修改形参，能否影响实参？
#### 把一个数组转换为树
#### 把一个树转换为数组
#### 读代码-构造函数和原型的重名属性
#### 一道让人失眠的promise-then执行顺序问题
#### 读代码-React-setState经典面试题
#### React-setState是微任务还是宏任务
#### 读代码-对象和属性的连续赋值
#### 读代码-对象属性类型的问题
#### 扩展补充：解决问题的常见思路
#### 重点及注意事项总结

## 八、项目设计 - 能否成为项目负责人

#### 扩展：如果你是一个项目的前端技术负责人，你的主要职责是什么？
#### 如何设计一个前端统计SDK-分析功能范围
#### 如何设计一个前端统计SDK-代码结构演示
#### sourcemap有何作用，如何配置
#### SPA和MPA应该如何选择
#### 设计一个H5编辑器的数据模型和核心功能-错误答案展示
#### 扩展知识补充：何时应该使用 SSR ，何时不用？
#### 设计一个H5编辑器的数据模型和核心功能-演示正确答案
#### 设计一个“用户-角色-权限”的模型和功能
#### 简单描述hybrid模板的更新流程
#### 开发一个H5抽奖页，需要后端提供哪些接口
#### 如果你是前端技术负责人，将如何做技术选型
#### 设计实现一个H5图片懒加载SDK
#### 扩展知识补充：我们常说的 B 端和 C 端，有什么区别
## 九、软技能 - 沟通、总结和学习能力

#### 你是否看过“红宝书”-
#### 如何做Code-review，要考虑哪些内容
#### 如何学习一门新语言，需要考虑哪些方面
#### 你觉得自己还有哪些不足之处？