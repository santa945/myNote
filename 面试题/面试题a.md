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

  #### 4-15-18 Vue组件通讯有几种方式

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

  #### 如何检测JS内存泄漏？

* 使用浏览器中的`Performance`, 勾选`Memory`,按左上角record（黑色圆点），点击stop，查看堆(HEAP)的变化，正常状态是锯齿状(创建后销毁)，一开始高，然后垃圾回收后变低；而内存泄漏会一直变高，堆(HEAP)里越来越多

  #### JS内存泄漏的场景有哪些

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
  #### 遍历一个数组用for和forEach哪个更快
  #### nodejs如何开启多进程，进程如何通讯-进程和线程的区别
  #### nodejs如何开启多进程，进程如何通讯-使用child_process.fork方式
  #### nodejs如何开启多进程，进程如何通讯-使用cluster方式
  #### 请描述js-bridge的实现原理
  #### requestIdleCallback和requestAnimationFrame有什么区别
  #### Vue每个生命周期都做了什么
  #### Vue2和Vue3和React三者的diff 算法有什么区别
  #### Vue-router的MemoryHistory是什么
  #### 重点及注意事项总结

## 四、知识广度 - 从前端到全栈

  #### 移动端H5点击有300ms延迟，该如何解决
  #### 扩展：Retina 屏幕的 1px 像素，如何实现
  #### HTTP请求中token和cookie有什么区别-cookie和session
  #### HTTP请求中token和cookie有什么区别-token和JWT
  #### 【连环问】session和JWT哪个更好
  #### 【连环问】如何实现SSO单点登录
  #### HTTP协议和UDP协议有什么区别
  #### 【连环问】HTTP协议1.0和1.1和2.0有什么区别
  #### 什么是HTTPS中间人攻击，如何预防
  #### script标签的defer和async有什么区别
  #### 【连环问】prefetch和dns-prefetch分别是什么
  #### 前端攻击手段有哪些，该如何预防-part1
  #### 前端攻击手段有哪些，该如何预防-part2
  #### WebSocket和HTTP协议有什么区别
  #### WebSocket和HTTP协议有什么区别-扩展-创建简易聊天室
  #### 【连环问】WebSocket和HTTP长轮询的区别
  #### 从输入URL 到网页显示的完整过程
  #### 【连环问】网页重绘repaint和重排reflow有什么区别
  #### 如何实现网页多标签tab通讯
  #### 【连环问】如何实现网页和iframe之间的通讯
  #### 请描述koa2的洋葱圈模型
  #### 扩展：后端有了 java php python ，为何还需要 nodejs ？
  #### 重点及注意事项总结

## 五、实际工作经验 - 是否做过真实项目

  #### H5页面如何进行首屏优化
  #### 后端一次性返回10w条数据，你该如何渲染
  #### 扩展：文字超出省略
  #### 前端常用的设计模式和使用场景
  #### 【连环问】观察者模式和发布订阅模式的区别
  #### 在实际工作中，你对Vue做过哪些优化
  #### 【连环问】你在使用Vue过程中遇到过哪些坑
  #### 在实际工作中，你对React做过哪些优化-上集
  #### 在实际工作中，你对React做过哪些优化-下集
  #### 【连环问】你在使用React时遇到过哪些坑
  #### 如何统一监听Vue组件报错
  #### 如何统一监听React组件报错
  #### 如果一个H5很慢，如何排查性能问题-通过Chrome Performance分析
  #### 如果一个H5很慢，如何排查性能问题-使用lighthouse分析
  #### 工作中遇到过哪些项目难点，是如何解决的
  #### 扩展：处理沟通冲突
  #### 重点及注意事项总结

## 六、编写高质量代码 - 正确，完整，清晰，鲁棒

  #### 手写一个JS函数，实现数组扁平化Array Flatten
  #### 【连环问】手写一个JS函数，实现数组深度扁平化
  #### 手写一个getType函数，获取详细的数据类型
  #### new一个对象的过程是什么，手写代码表示
  #### 深度优先遍历一个DOM树
  #### 广度优先遍历一个DOM树
  #### 【连环问】深度优先遍历可以不用递归吗
  #### 手写一个LazyMan，实现sleep机制
  #### 手写curry函数，实现函数柯里化
  #### instanceof原理是什么，请写代码表示
  #### 手写函数bind功能
  #### 【连环问】手写函数call和apply功能
  #### 手写EventBus自定义事件-包括on和once
  #### 手写EventBus自定义事件-on和once分开存储
  #### 手写EventBus自定义事件-单元测试
  #### 用JS实现一个LRU缓存-分析数据结构特点，使用Map
  #### 用JS实现一个LRU缓存-代码演示和单元测试
  #### 【连环问】不用Map实现LRU缓存-分析问题，使用双向链表
  #### 【连环问】不用Map实现LRU缓存-代码演示
  #### 手写JS深拷贝-考虑各种数据类型和循环引用
  #### 扩展补充：根据一个 DOM 树，写出一个虚拟 DOM 对象
  #### 重点及注意事项总结

## 七、分析和解决问题的思路 - 可以独立解决问题



  #### [1, 2, 3].map(parseInt)
  #### 读代码-函数修改形参，能否影响实参？
  #### 把一个数组转换为树
  #### 【连环问】把一个树转换为数组
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
  #### 【连环问】sourcemap有何作用，如何配置
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