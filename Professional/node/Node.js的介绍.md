# Node.js的介绍

##### 1、Node.js的是建立在Chrome的JavaScript的运行时，可方便地构建快速，可扩展的网络应用程序的平台。Node.js使用事件驱动，非阻塞I/O模型，轻量、高效，可以完美地处理时时数据，运行在不同的设备上。

##### 2、node.js应用的场景

2.1 Web开发：Express + EJS + Mongoose/MySQL

[express](http://expressjs.com/) 是轻量灵活的Nodejs Web应用框架，它可以快速地搭建网站。Express框架建立在Nodejs内置的Http模块上，并对Http模块再包装，从而实际Web请求处理的功能。

[ejs](https://github.com/visionmedia/ejs)是一个嵌入的Javascript模板引擎，通过编译生成HTML的代码。

[mongoose](http://mongoosejs.com/) 是MongoDB的对象模型工具，通过Mongoose框架，可以进行访问MongoDB的操作。

[mysql](https://github.com/felixge/node-mysql) 是连接MySQL数据库的通信API，可以进行访问MySQL的操作。

通常用Nodejs做Web开发，需要3个框架配合使用，就像Java中的SSH。