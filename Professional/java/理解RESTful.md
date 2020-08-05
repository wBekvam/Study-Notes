# 理解RESTful

## 1. 什么是RESTful

- RESTful架构，就是目前最流行的一种互联网软件架构。它结构清晰、符合标准、易于理解、扩展方便，所以正得到越来越多网站的采用。REST这个词，是Roy Thomas Fielding在他2000年的博士论文中提出的 .

- REST 是Representational State Transfer的缩写，翻译是”表现层状态转化”（表述形式的状态转换）。 可以总结为一句话：REST是所有Web应用都应该遵守的架构设计指导原则。

- 面向资源是REST最明显的特征，对于同一个资源的一组不同的操作。资源是服务器上一个可命名的抽象概念，资源是以名词为核心来组织的，首先关注的是名词。REST要求，必须通过统一的接口来对资源执行各种操作。对于每个资源只能执行一组有限的操作。

  [**如果一个架构符合REST原则，就称它为RESTful架构**](http://www.ruanyifeng.com/blog/2011/09/restful.html)

  > RESTful架构：
  >
  > - 每个URL代表一种资源；
  > - 客户端和服务器之间，传递这种资源的某种表现层；
  > - 客户端通过四个HTTP动词，对服务器端资源进行操作，实现"表现层状态转化"。

**7个HTTP方法：`GET/POST/PUT/DELETE/PATCH/HEAD/OPTIONS`**

## 2. 接口规范

>  我们在项目中用到了GET/POST/PUT/DELETE四种方法

**【说明】**

- `安全`：对服务端的数据不会造成改变。
- `幂等`：多次重复同样操作，对服务端的数据的影响是一样的。
- `不幂等`：多次重复同样操作，对服务端的数据的影响是不一样的，如添加数据，就是不幂等的，因为每次执行，都会生成新的数据；更新数据就是幂等的，因为每次执行，服务器的数据还是那条数据；删除数据是幂等的，因为第一次已经删除了数据，第二次执行不会对服务器再产生影响

### GET

> 安全且幂等

**HTTP响应状态码：**

200（OK） - 表示已在响应中发出
204（无内容） - 资源有空表示
301（Moved Permanently） - 资源的URI已被更新
303（See Other） - 其他（如，负载均衡）
304（not modified）- 资源未更改（缓存）
400 （bad request）- 指代坏请求（如，参数错误）
404 （not found）- 资源不存在
406 （not acceptable）- 服务端不支持所需表示
500 （internal server error）- 通用错误响应
503 （Service Unavailable）- 服务端当前无法处理请求

### POST

> 不安全且不幂等

**HTTP响应状态码：**

200（OK）- 如果现有资源已被更改
201（created）- 如果新资源被创建
202（accepted）- 已接受处理请求但尚未完成（异步处理）
301（Moved Permanently）- 资源的URI被更新
303（See Other）- 其他（如，负载均衡）
400（bad request）- 指代坏请求
404 （not found）- 资源不存在
406 （not acceptable）- 服务端不支持所需表示
409 （conflict）- 通用冲突
412 （Precondition Failed）- 前置条件失败（如执行条件更新时的冲突）
415 （unsupported media type）- 接受到的表示不受支持
500 （internal server error）- 通用错误响应
503 （Service Unavailable）- 服务当前无法处理请求

### PUT

> 不安全但幂等

**HTTP响应状态码：**

200 （OK）- 资源已被删除
301 （Moved Permanently）- 资源的URI已更改
303 （See Other）- 其他，如负载均衡
400 （bad request）- 指代坏请求
404 （not found）- 资源不存在
409 （conflict）- 通用冲突
500 （internal server error）- 通用错误响应
503 （Service Unavailable）- 服务端当前无法处理请求

### DELETE

> 不安全但幂等

**HTTP响应状态码：**

200 （OK）- 资源已被删除
301 （Moved Permanently）- 资源的URI已更改
303 （See Other）- 其他，如负载均衡
400 （bad request）- 指代坏请求
404 （not found）- 资源不存在
409 （conflict）- 通用冲突
500 （internal server error）- 通用错误响应
503 （Service Unavailable）- 服务端当前无法处理请求

**【说明】**

- GET和DELETE，只支持参数方式提交：
  - 查询参数 ?xxx=x&yyy=y
  - 路径参数：标准的REST，/x/y
- POST和PUT，支持Body消息体和参数两种方式
- PUT要带id参数，保证幂等
- URL不要包含动词，动词是4个http的方法

