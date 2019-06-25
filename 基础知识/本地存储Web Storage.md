# 本地存储Web Storage

### 好处

减少网络流量：一旦数据保存在本地后，就可以避免再向服务器请求数据，因此减少不必要的数据请求，减少数据在浏览器和服务器间不必要地来回传递。

快速显示数据：性能好，从本地读数据比通过网络从服务器获得数据快得多，本地数据可以即时获得。再加上网页本身也可以有缓存，因此整个页面和数据都在本地的话，可以立即显示。

临时存储：很多时候数据只需要在用户浏览一组页面期间使用，关闭窗口后数据就可以丢弃了，这种情况使用sessionStorage非常方便。 

**localStorage的局限**

1、浏览器的大小不统一，并且在IE8以上的IE版本才支持localStorage这个属性

2、目前所有的浏览器中都会把localStorage的值类型限定为string类型，这个在对我们日常比较常见的JSON对象类型需要一些转换

### sessionStorage、localStorage、cookie的区别

#####  共同点：

都是保存在浏览器端，并且是同源的（URL的协议、端口、主机名是相同的，只要有一个不同就属于不同源）

#####  不同点：

 1、cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递，而sessionStorage和localStorage不会自动把数据发送给服务器，仅在本地保存。cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下。

 2、存储大小限制也不同，cookie数据不能超过4K，同时因为每次http请求都会携带cookie、所以cookie只适合

保存很小的数据，如会话标识。sessionStorage和localStorage虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大。

 3、数据有效期不同，sessionStorage仅仅在当前浏览器窗口关闭之前有效；localStorage始终有效，窗口或者

浏览器关闭之后也一直保存，因此作用持久数据；cookie，只在设置cookie过期时间之前有效，即使窗口关闭或者浏览器关闭。

 4、作用域不同：sessionStorage在不同的浏览器窗口中不共享，即使是同一个页面，localStorage在所有的同源窗口中是共享的，cookie也是在所有同源的窗口中共享的。（只要URL的协议、端口、主机名三者中有一个不同，就属于不同的文档源） 

### 增删改查

##### 写入

```
if(！window.localStorage){
    alert("浏览器不支持localstorage");
    return false;
}else{
    var storage=window.localStorage;
    //写入a字段
    storage["a"]=1;
    //写入b字段
    storage.a=1;
    //写入c字段
    storage.setItem("c",3);
    console.log(typeof storage["a"]);
    console.log(typeof storage["b"]);
    console.log(typeof storage["c"]);
}
```

##### 读取

```
var storage=window.localStorage;
storage["a"]=1;
//第一种方法读取
var a=storage.a;
//第二种方法读取
var b=storage["a"];     
//第三种方法读取
var c=storage.getItem("a");
```

##### 修改（同写入）

##### 删除

1、将localStorage的所有内容清除

```
var storage=window.localStorage;
storage.clear();
```

2、 将localStorage中的某个键值对删除

```
var storage=window.localStorage;
storage.a=1;
storage.removeItem("a");
```

### 获取键

使用key()方法，向其中出入索引即可获取对应的键

```
var storage=window.localStorage;     
for(var i=0;i<storage.length;i++){
    var key=storage.key(i);
    console.log(key);
}
```



 



​    