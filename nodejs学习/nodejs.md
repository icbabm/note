# 大纲-day01
## 初识Nodejs
- JavaScript是什么？ 
- JavaScript可以运行在哪里？ 

![](./_media/1.png)

浏览器 | 内核    
-------|---------
IE     | Trident 
FireFox| Gecko   
Chrome | WebKit  
Safari | WebKit  
Opera  | Presto  
Edge   | Chakra  

## Node.js的诞生
![image](./_media/2.png)

- 作者Ryan Dahl 瑞恩·达尔
    + 2004 纽约 读数学博士 
    + 2006 退学到智利 转向开发 
    + 2009.5对外宣布node项目，年底js大会发表演讲 
    + 2010 加入Joyent云计算公司 
    + 2012 退居幕后

> Node.js 是一种建立在Google Chrome’s v8 engine上的 non-blocking (非阻塞）, event-driven （基于事件的） I/O平台. 
Node.js平台使用的开发语言是JavaScript，平台提供了操作系统低层的API，方便做服务器端编程，具体包括文件操作、进程操作、通信操作等系统模块

## Node.js可以用来做什么？

- 具有复杂逻辑的动态网站 
- WebSocket服务器 
- 命令行工具 
- 带有图形界面的本地应用程序 
- ......

## node.js开发环境准备

1. 普通安装方式[官方网站](https://nodejs.org/zh-cn/)
2. LTS(长期支持版)稳定版，Current体验版
3. 打开命令行 cmd，输入`node --version` 或者`node -v` 查看版本
4. 多版本安装方式
    - 卸载已有的Node.js
    - 下载[nvm](https://github.com/coreybutler/nvm-windows)
    - 在C盘创建目录dev
    - 在dev目中中创建两个子目录nvm和nodejs
    - 并且把nvm包解压进去nvm目录中
    - 在install.cmd文件上面右键选择【以管理员身份运行】
    - 打开的cmd窗口直接回车会生成一个settings.txt文件，修改文件中配置信息
    - 配置nvm和Node.js环境变量
        + NVM_HOME:C:\dev\nvm
        + NVM_SYMLINK:C:\dev\nodejs
    - 把配置好的两个环境变量加到Path中
## nvm常用的命令
- nvm list 查看当前安装的Node.js所有版本
- nvm install 版本号 安装指定版本的Node.js
- nvm uninstall 版本号 卸载指定版本的Node.js
- nvm use 版本号 选择指定版本的Node.js

## Node.js之HelloWorld
- 解析执行javascript

  - 创建javaScript脚本文件
  - 打开终端，定位到脚本文件所属目录
  - 输入`node 文件名`执行对应的文件
  - 注意：文件名不要使用`node.js`来命名。

- 文件读写

  ```
  // 浏览器中的 JavaScript 是没有文件操作的能力的
  // 但是 Node 中的 JavaScript 具有文件操作的能力
  
  // fs 是 file-system 的简写，就是文件系统的意思
  // 在 Node 中如果想要进行文件操作，就必须引入 fs 这个核心模块
  // 在 fs 这个核心模块中，就提供了所有的文件操作相关的 API
  // 例如：fs.readFile 就是用来读取文件的
  
  // 1. 使用 require 方法加载 fs 核心模块
  var fs = require('fs')
  
  // 2. 读取文件
  //    第一个参数就是要读取的文件路径
  //    第二个参数是一个回调函数
  //          
  //        成功
  //          data 数据
  //          error null
  //        失败
  //          data undefined没有数据
  //          error 错误对象
  fs.readFile('./data/a.txt', function (error, data) {
    // <Buffer 68 65 6c 6c 6f 20 6e 6f 64 65 6a 73 0d 0a>
    // 文件中存储的其实都是二进制数据 0 1
    // 这里为什么看到的不是 0 和 1 呢？原因是二进制转为 16 进制了
    // 但是无论是二进制01还是16进制，人类都不认识
    // 所以我们可以通过 toString 方法把其转为我们能认识的字符
    // console.log(data)
  
    // console.log(error)
    // console.log(data)
  
    // 在这里就可以通过判断 error 来确认是否有错误发生
    if (error) {
      console.log('读取文件失败了')
    } else {
      console.log(data.toString())
    }
    
     // 3. 写文件
      // 第一个参数：文件路径
      // 第二个参数：文件内容
      // 第三个参数：回调函数
      //    error
      //    
      //    成功：
      //      文件写入成功
      //      error 是 null
      //    失败：
      //      文件写入失败
      //      error 就是错误对象
    fs.writeFile('./data/你好.md', '大家好，给大家介绍一下，我是Node.js', function (error) {
    // console.log('文件写入成功')
    // console.log(error)
    if (error) {
      console.log('写入失败')
    } else {
      console.log('写入成功了')
    }
  })
  ```

- http

  ```
  var http = require('http')
  
  // 1. 创建 Server
  var server = http.createServer()
  
  // 2. 监听 request 请求事件，设置请求处理函数
  server.on('request', function (req, res) {
    console.log('收到请求了，请求路径是：' + req.url)
    console.log('请求我的客户端的地址是：', req.socket.remoteAddress, req.socket.remotePort)
  
    // res.write('hello')
    // res.write(' world')
    // res.end()
  
    // 上面的方式比较麻烦，推荐使用更简单的方式，直接 end 的同时发送响应数据
    // res.end('hello nodejs')
  
    // 根据不同的请求路径发送不同的响应结果
    // 1. 获取请求路径
    //    req.url 获取到的是端口号之后的那一部分路径
    //    也就是说所有的 url 都是以 / 开头的
    // 2. 判断路径处理响应
  
    var url = req.url
  
    if (url === '/') {
      res.end('index page')
    } else if (url === '/login') {
      res.end('login page')
    } else if (url === '/products') {
      var products = [{
          name: '苹果 X',
          price: 8888
        },
        {
          name: '菠萝 X',
          price: 5000
        },
        {
          name: '小辣椒 X',
          price: 1999
        }
      ]
  
      // 响应内容只能是二进制数据或者字符串
      //  数字
      //  对象
      //  数组
      //  布尔值
      res.end(JSON.stringify(products))
    } else {
      res.end('404 Not Found.')
    }
  })
  
  // 3. 绑定端口号，启动服务
  server.listen(3000, function () {
    console.log('服务器启动成功，可以访问了。。。')
  })
  
  ```

- 其他

  ```
  // 用来获取机器信息的
  var os = require('os')
  
  // 用来操作路径的
  var path = require('path')
  
  // 获取当前机器的 CPU 信息
  console.log(os.cpus())
  
  // memory 内存
  console.log(os.totalmem())
  
  // 获取一个路径中的扩展名部分
  // extname extension name
  console.log(path.extname('c:/a/b/c/d/hello.txt'))
  ```

  

- http







## 服务器端模块化
- 服务器端模块化规范CommonJS与实现Node.js
- 模块导出与引入
- 模块导出机制分析
- 模块加载规则
    + 模块查找 不加扩展名的时候会按照如下后缀顺序进行查找 .js .json .node
- 模块分类
    + 自定义模块
    + 系统核心模块
        * fs 文件操作
        * http 网络操作
        * path 路径操作
        * querystring 查询参数解析
        * url url解析
        * ......

## ES6常用语法
- 变量声明let与const
- 变量的解构赋值
    + 数组解构赋值
    + 对象解构赋值
    + 字符串解构赋值
- 字符串扩展
    + includes()
    + startsWith()
    + endsWith()
    + 模板字符串
- 函数扩展
    + 参数默认值
    + 参数结构赋值
    + rest参数
    + 扩展运算符
    + 箭头函数
- 类与继承