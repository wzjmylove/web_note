# node

简介：就是一个运行环境，该引擎就是V8引擎，但不同于浏览器，其是JS的后端运行环境

内置API：fs、path、http、quertstring（没有DOM、BOM、AJAX）

## 终端

定义：专为开发人员设计的，用于实现人机交互的一种方式

常用指令：

> cd + 路径 		进入当前磁盘内的文件夹
>
> D:					进入D盘
>
> tab				   自动补全文件名
>
> ↑					  执行上次一操作
>
> esc				  清空当前操作的指令
>
> cls/clear		   清空页面
>
> CTRL + c		 停止服务器运行

## fs文件系统模块

fs模块是node的内置模块，用于操作文件

fs引入方式：`const fs = require('fs')`
只要安装了node，就会有fs模块，引入时，require会自动取查找fs模块

### fs.readFile()		

> 作用：用于读取指定文件中的内容
>
> 语法：`fs.readFile(path , [option] , callback)`
>
> 参数：
>
> > path：字符串，表示文件路径
> >
> > option：字符串，以什么编码格式来读取文件，无默认值		可选
> >
> > callback：文件读取成功后，通过回调函数拿到读取的结果
> >
> > > callback函数里面有两个参数	`(err,result) => {}`	
> > >
> > > err：读取失败时的结果，读取成功时默认未null
> > >
> > > result：读取成功时，文件内的内容，读取失败是未undefined
> > >
> > > 即：读取失败时，err为错误对象，result为undefined；读取成功时，err为null，result为文件内容

例：

```js
const fs = require('fs');
fs.readFile('./code.txt', 'utf8', (err, result) => {
    console.log('err:', err);
    console.log('result', result);
})
```

#### 判断文件是否读取成功

通过判断err的值是否为null即可

例：

```js
const fs = require('fs');
fs.readFile('./code.txt', 'utf8', (err, result) => {
    if (err) {
        return console.log('文件读取失败\n', err.message);
    }
    console.log('文件读取成功，内容是\n', result);
})
```

### fs.writeFile()		

> 作用：用于向指定的文件写入内容
>
> 语法：`fs.writeFile(file , data , [options] , callback)`
>
> 参数：
>
> > path：字符串，表示文件路径	（如果找不到该path下的文件，则新建一个）
> >
> > data：表示要写入的内容
> >
> > options：字符串，以什么编码格式来读取文件，默认为utf8		可选
> >
> > callback：文件读取成功后，通过回调函数拿到读取的结果
> >
> > > callback函数里面有两个参数	`(err,result) => {}`	
> > >
> > > err：读取失败时的结果，读取成功时默认未null
>
> 注：一个文件重复调用writeFile方法，会使新内容全部覆盖旧内容

例：

```js
const fs = require('fs');

fs.writeFile('./file/test2.txt', 'Hello WZ!', 'utf8', (err) => {
    if (err) {
        return console.log('文件写入失败\n', err.message);
    }
    console.log('文件写入成功');
})
```

### 文件路径解决办法

文件路径的问题：

正常的 ./ 和 ../ 是相对路径，在给path赋值相对路径，很容易发生路径动态拼接的问题

路径动态拼接问题：就是在A文件夹里面调用另一个B文件夹下的代码，该代码内的相对路线会取拼接A文件夹的路径，但是该代码的相对								路径是相对于B文件夹来说的，因此会报错，该错误就被称为路径动态拼接问题。

#### 绝对路径

给path一个绝对路径

绝对路径中，路径之间用两个斜线分割（因为一个斜线在JS中代表转义的意思）

如：`F:\\学习\\前端培训\\课程练习\\node\\fs\\test.js`

缺点：移植性差，不利于维护

#### __dirname

解决了绝对路径的移植性差问题

node中有个标识符：__dirname

表示当前文件所处的目录(当前JS所处目录)

语法：`__dirname + './xxx/xx.js'`

如：

```js
const fs = require('fs');
fs.readFile( __dirname + '/code.txt', 'utf8', (err, result) => {})
```

## path路径模块

path 模块是 Node.js 官方提供的、用来处理路径的模块

path引入方式：`const fs = require('path')`
只要安装了node，就会有path模块，引入时，require会自动取查找path模块

### path.join()

> 作用：用来将多个路径片段拼接成一个完整的路径字符串
>
> 语法：`path.join([...paths])`
>
> 参数：paths：字符串，路径，可以是多个		可选
>
> 返回：一个拼接之后的字符串
>
> 注：如果出现 ../ ，那么会抵消一次路径

如：

```js
const path = require('path')

const mypath = path.join(`${__dirname}/file/code.js`)
console.log(mypath);
//结果：'f:\学习\前端培训\课程练习\node\path\file\code.js'

const a = path.join('/a', '/b/c', '../', './d', 'e');
console.log(a);
//结果：'\a\b\d\e'
```

与传统 + 拼接字符串的方式表示路径的不同：

path.join()方法可以将 ./ 中的 . 给忽略掉，但如果是传统方法，那么将会报错（因为找不到`a/b./c`路径下的文件）

### path.basename()

> 作用：用来从路径字符串中，将文件名解析出来
>
> 语法：`path.basename(path,[ext])`
>
> 参数：
>
> > path：字符串，表示文件路径
> >
> > ext：文件拓展名（如txt、js），一般用来切片删除扩展名	可选参数
>
> 返回：字符串，路径中的最后一部分

如：

```js
const path = require('path');

let fpath = path.join('a', './b', 'c/index.html');
console.log(fpath);
//结果：'a\b\c\index.html'

let fullName = path.basename(fpath);
console.log(fullName);
//结果：'index.html'

let nameWithoutExt = path.basename(fpath, '.html');
console.log(nameWithoutExt);
//结果：'index'
```

### path.extname() 

> 作用：可以获取路径中的扩展名部分
>
> 语法：`path.extname(path,[ext])`
>
> 参数：path：字符串，表示文件路径
>
> 返回：字符串，文件的拓展名（带 **.** ）

如：

```js
const path = require('path');

let fpath = path.join('a', './b', 'c/index.html');
console.log(fpath);
//结果：'a\b\c\index.html'

let fext = path.extname(fpath);
console.log(fext);
//结果：'.html'
```

## HTTP模块

> 客户端：在网络节点中，负责消费资源的电脑，叫做客户端
>
> 服务器：负责对外提供网络资源的电脑，叫做服务器
>
> 服务器和客户端的区别：服务器上安装了 web 服务器软件，例如：IIS、Apache 等。同理通过安装这些服务器软件，就能把一台普通的电脑变成一台 web 服务器。

http 模块是 Node.js 官方提供的、用来创建 web 服务器的模块。通过 http 模块提供的 http.createServer() 方法，就能方便的把一台普通的电脑，变成一台 Web 服务器，从而对外提供 Web 资源服务。

在 Node.js 中，我们不需要使用 IIS、Apache 等这些第三方 web 服务器软件。因为我们可以基于 Node.js 提供的 http 模块，**通过几行简单的代码，就能轻松的手写一个服务器软件**，从而对外提供 web 服务。

http引入方式：`const http = require('http')`
只要安装了node，就会有path模块，引入时，require会自动取查找http模块

### 创建基本Web服务器

步骤

> 1、导入 http 模块
>
> 2、创建 web 服务器实例：调用 http.createServer() 方法，即可快速创建一个 web 服务器实例
>
> 3、为服务器实例绑定 request 事件（客户端向服务器发送访问请求，就会调用request）
>
> 4、启动服务器：调用服务器实例的 .listen() 方法，即可启动当前的 web 服务器实例

代码：

```js
// 1.导入http模块
const http = require('http');
//2.创建Web服务器实例
const server = http.createServer();
//3.为服务器绑定request事件，监听客户端发过来的请求
server.on('request', (req, res) => {
    console.log('欢迎访问我的服务器');
});
//4.启动服务器
server.listen(8080, () => {
    console.log('服务器运行在这个网址:http://127.0.0.1:8080');
})
```

#### http.createServer()

> 作用：创建 web 服务器实例
>
> 语法：`const server = http.createServer()`

#### server.on()

> 作用：客户端向服务器发送访问请求request，就会调用on方法里面的回调函数
>
> 语法：`server.on('request', function(req,res) {} )`
>
> 参数：
>
> > request：客户端向服务器发送的请求，且请求为reqest；或者是respond
> >
> > req
> >
> > res

##### req

> 定义：req是与客户端相关的数据或属性
>
> 常见属性：
>
> > req.url：客户端访问的地址，非服务器地址，而是ip后面的 / 、/index.html 等
> >
> > req.method：客户端向服务器发起的method请求类型（如get、post）

##### res

> 作用：向客户端响应一些内容
>
> 常见方法：
>
> > res.end(xxx)：向客户端响应xxx内容
> >
> > res.setHeader('Content-Type', 'text/plain;charset=utf-8')：防止中文乱码

#### server.listen()

> 作用：启动服务器
>
> 语法：`server.listen(duankou, function() {})`
>
> 参数：duankou：服务器的访问端口（默认是80）

## 模块化

定义：模块化是指解决一个复杂问题时，自顶向下逐层把系统划分成若干模块的过程。对于整个系统来说，模块是可组合、分解和更换的单元。

编程中的模块化：**遵守固定的规则**，把一个大文件拆成独立并互相依赖的多个小模块

优点：

> 1、提高了代码的复用性
>
> 2、提高了代码的可维护性
>
> 3、可以实现按需加载

Node.js 中根据模块来源的不同，将模块分为了 3 大类，分别是：

> 1、内置模块（内置模块是由 Node.js 官方提供的，例如 fs、path、http 等）
>
> 2、自定义模块（用户创建的每个 .js 文件，都是自定义模块）
>
> 3、第三方模块（由第三方开发出来的模块，并非官方提供的内置模块，也不是用户创建的自定义模块，使用前需要先下载）

### 加载模块

require() 方法，可以加载需要的内置模块、用户自定义模块、第三方模块进行使用

注意：

> 1、加载内置模块和第三方模块时，只需写模块名字；但是加载自定义模块，需要写路径（如：`require('./xxx.js')`）
>
> 2、加载自定义模块时，可以不写后缀名 如 .js .html 
>
> 3、使用 require() 方法加载其它模块时，会执行被加载模块中的代码
> 即：当模块内有 打印 等代码时，在require模块时就会执行 打印 等操作

### 模块作用域

