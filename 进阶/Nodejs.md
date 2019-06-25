# Nodejs

## 了解Nodejs

Nodejs是2009由Ryan Dahl推出的运行在服务端的 JavaScript（类似于java,php,python,.net等服务端语言）,
基于Google的V8引擎，V8引擎执行Javascript的速度非常快，性能非常好。

![](C:\Users\Lemon\Desktop\doc\浏览器与服务器.PNG)

### 版本

> 在命令窗口中输入 `node -v` 可以查看版本

- 0.x 完全不技术 ES6
- 4.x 部分支持 ES6 特性
- 5.x 部分支持ES6特性（比4.x多些），属于过渡产品，现在来说应该没有什么理由去用这个了
- 6.x 支持98%的 ES6 特性
- 8.x 支持 ES6 特性
- 10.x 最新稳定版本

### 安装与配置

1. [下载](https://nodejs.org/en/download/)安装文件
2. 下载完后进行安装，建议安装到默认路径（注意不要有中文路径）
3. 配置环境变量（安装时勾选）
4. 在命令窗口中输入 `node -v`，如果正常显示版本号则表示安装成功

###### 介绍版本及nodejs官网下载（强调环境变量）

## Nodejs的使用

### REPL(Read Eval Print Loop 交互式解释器)

> 类似于在浏览器的开发人员工具的控制台，一般用于在服务端执行一些简单的javascript语句

- 进入REPL：node并回车
- 退出：两次 Ctrl + C

![REPL](C:/Users/Lemon/Documents/WeChat%20Files/yemengzi/FileStorage/File/2019-04/img/repl.png "Optional title")

### 执行js文件代码

REPL 只适用于一些简单的 Javascript 语法，对于稍复杂的程序，可以直接写到 js 文件当中，并执行以下命令

```bash
    node hello.js
```

不同于前端js代码，没有浏览器安全级别的限制，提供很多系统级别的API，如：

- 文件的读写
- 进程的管理
- 网络通信
- ……

### NPM(Node Package Manager包管理)

Node.js 的包管理器 npm，是全球最大的开源库生态系统，随着nodejs一起安装（`npm -v`查看安装版本）

### 模块化开发

为了让Nodejs的文件可以相互调用，Nodejs提供了一个简单的模块系统，一个文件即一个模块，采用同步的commonJS规范

#### 模块分类

> 模块系统是 Nodejs 最基本也是最常用的。一般情况模块可分为四类：

- 原生模块（Nodejs内置模块）
- 文件模块（json文件等）
- 第三方模块
- 自定义模块

## 自定义模块

#### 创建自定义模块

```javascript
//hello.js
function hello(){
    return 'hello laoxie';
}
//对外暴露接口
module.exports = hello;
```

#### 导出模块

> 如果没有这句话，引入模块时 就会得到 空对象

- module.exports

> 对外暴露单个接口，一个模块中只能有一个 module.exports，多个会被覆盖。

- exports

> 在一个模块中暴露多个exports接口

```javascript
    //person.js
    function setName(){
        return 'laoxie'
    }
    function setAge(){
        return 18
    }
    // 对外暴露接口
    exports.setName = setName;
    exports.setAge = setAge;

```

#### 引入模块require()

> 引入模块，使用nodejs内置的require()方法（同步的commonJS规范）

```javascript
    //page.js
    //得到一个对象，包含暴露的setName,setAge方法
    let person = require('./person.js');
    // 既然是得到对象，也可以直接解构
    let {setName,setAge} = require('./person.js');

```

- require 方法中的文件查找策略

![](C:\Users\Lemon\Desktop\doc\模块加载过程.jpg)原生模块

> 原生模块不需要安装，直接引入使用

## http 模块

> 利用nodejs创建http服务器，当每个请求到达服务器时，nodejs会为请求创建一个请求对象（request），request对象包含客户端提交上来的数据。同时也会创建一个响应对象（response），response对象主要负责将服务器的数据响应到客户端

- 常用属性/方法

  - request.url
  - response.writeHead()

  ```
  response.writeHead(200,{'Content-Type':'text/html;charset=utf-8'})
  //扩展：
  text/plain
  text/html
  application/json
  ```

  - response.write()
  - response.end()

```javascript
    //引入原生模块
    var http = require('http');
    http.createServer(function(request, response){
        response.end('服务器启动成功，端口为8080');
    }).listen(8080);
```

###### 扩展：supervisor包

## url 模块

> 向服务器发起请求的 url 地址都是字符串类型，url 所包含的信息也比较多，如：协议、主机名、端口、路径、参数、锚点等，直接操作字符串会相对麻烦，使用Nodejs 的 url 原生模块可轻松解决这一问题

- url常用属性
  - href： 解析前的完整原始 URL，协议名和主机名已转为小写
  - protocol： 请求协议，小写
  - host： url 主机名，包括端口信息，小写
  - hostname: 主机名，小写
  - port: 主机的端口号
  - pathname: URL中路径，下面例子的 /one
  - search: 查询对象，即：queryString，包括之前的问号“?”
  - path: pathname 和 search的合集
  - query: 查询字符串中的参数部分（问号后面部分字符串），或者使用 querystring.parse() 解析后返回的对象
  - hash: 锚点部分（即：“#”及其后的部分）

- url.parse(url, boolean)：把url信息转成对象
  - url：字符串格式的 url
  - boolean：是否把url参数转为对象，
    - false(默认)：参数为字符串
    - true：将参数转转对象

    var url = require('url');
    //第二个参数为 true => {a: 'index', t: 'article', m: 'default'}
    var params = url.parse('http://www.laoxie.com/one?a=index&t=article&m=default#laoxie', true);
    //params.query 为一个对象
    console.log(params.query);
    //第二个参数为 false
    params = url.parse('http://www.laoxie.com/one?a=index&t=article&m=default#laoxie', false);
    //params.query 为一个字符串 => ?a=index&t=article&m=default
    console.log(params.query);
- url.format(params)：对象转字符串，url.parse的反过程
- url.resolve(): 以一种 Web 浏览器解析超链接的方式把一个目标 URL 解析成相对于一个基础 URL

```javascript
    var url = require('url');
    url.resolve('http://laoxie.com/', '/one')// 'http://laoxie.com/one'
```

## fs 模块

> Buffer的理解：一个专门存放二进制数据的缓存区，类似一个数组。
> 在处理像TCP流或文件流时，必须使用到二进制数据，js语言自身没有二进制数据类型，所以Nodejs推出Buffer类型用于解决文件读取与前后端数据传输的问题

- readFile(path,callback): 读取文件内容（异步）

```javascript
    var fs = require('fs');
    fs.readFile('about.txt', function (err, data) {
        // err：无法读取文件时的错误信息，读取成功时为null
        // data: 文件信息（Buffer）
        if (err) {
            return console.error(err);
        }
        console.log("异步读取: " + data.toString());
    });
```

- readFileSync(path) : 读取文件内容（同步）

```javascript
    var fs = require('freadFileSyncs');
    var data = fs.readFileSync('demoFile.txt');
    console.log("同步读取: " + data.toString());
```

- writeFile(): 写入文件内容（覆盖）

```javascript
    var fs = require('fs');
    //每次写入文本都会覆盖之前的文本内容
    fs.writeFile('about.txt', '我们不一样',  function(err) {
        if (err) {
            return console.error(err);
        }
        console.log("数据写入成功！");
    });
```

- appendFile(): 写入文件内容（追加）

```javascript
    var fs = require('fs');
    fs.appendFile('about.txt', '有啥不一样', function (err) {
        if (err) {
            return console.error(err);
        }
        console.log("数据写入成功！");
    });
```

- 图片读取（http服务器）

> 图片读取不同于文本，因为文本读出来可以直接用 console.log() 输出，但图片则需要在浏览器中显示，所以需要先搭建 web 服务，然后以字节方式读取的图片在浏览器中渲染。

```javascript
  var http = require('http');
  var fs = require('fs');
  //图片读取是以字节的方式
  var content =  fs.readFileSync('001.jpg', "binary");
  http.createServer(function(request, response){
    //图片在浏览器的渲染因为没有 img 标签，所以需要设置响应头为 image
    response.writeHead(200, {'Content-Type': 'image/jpeg'});
    response.write(content, "binary");
    response.end();
  }).listen(3000,function(){
    console.log('Server running at http://localhost:3000/');
  });
```



## path模块

> 用于处理文件与目录的路径

- path.parse(path) 返回一个对象，其中对象的属性表示 path 的元素
  - dir 
  - root
  - base
  - name
  - ext
- path.basename(path) 返回 path 的最后一部分（同path.parse()中的base）
- path.dirname(path) 返回 path 的目录名（同path.parse()中的dir）
- path.extname(path) 返回 path 的扩展名，（同path.parse()中的ext）
- path.join([...paths]) 使用平台特定的分隔符把所有 path 片段连接到一起，并规范化生成的路径

###### 案例：图片展示

```
const http = require('http');
const fs = require('fs');
const url = require('url');
const path = require('path');

  //图片读取是以字节的方式
  http.createServer(function(request, response){
    var pathname = url.parse(request.url).pathname;
    var real = path.join(__dirname,pathname);
    console.log(real);

    //图片在浏览器的渲染因为没有 img 标签，所以需要设置响应头为 image
   
    fs.readFile(real, function(err,data){ 
        response.writeHead(200, {'Content-Type': 'image/jpg'});
        response.end(data);
    });

  }).listen(3000,function(){
    console.log('Server running at http://localhost:3000/');
});

```

