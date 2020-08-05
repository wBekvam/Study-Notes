# JSP语言基础

## 1、jsp内置对象

### 1、request对象

##### 	   1.1、HTTP请求的所有细节

##### 	   1.2、在请求转发时，需要把一些数据传递到转发后的页面进行处理，这时就需要用request对象的setAttribute()方法将数据保存到request范围变量中。

##### 			用法：request.setAttribute（Strign name, Object obj)

​								name: String, 在转发后的页面取数据时，就是通过这个变量名来获取数据

​								obj: 用于指定需要在request范围内传递的数据

#####                         可以通过getAttribute(String name) 来接收。返回值类型为Object，可以用toString()转为字符						串类型。

##### 		1.3、获取cookie。cookie可以标识用户身份，记录用户名和密码，跟踪重复用户。浏览器将以键值对的形式保存在指定的某个目录下。

​				注意：在保存cookie的信息时，若含有中文，则需要调用java.net.URLEncode类的encode()方法。读取时，需要调用java.net.URLDecoder类的decode()方法

##### 		1.4 获取客户端的内置信息

![1563164556551](C:\Users\HZ\AppData\Roaming\Typora\typora-user-images\1563164556551.png)

##### 	1.5、解决中文乱码问题

​			new String(request.getParameter("name").getBytes("ISO-8859-1"), "utf-8")

##### 	1.6、显示国际化

​				

### 2、response对象：用于响应客户请求，向客户端输出信息

#### 	2.1、重定向网页

​			response.sendRediercet(String path)      path: 可以是相对路径，也可以是不同主机的其他地址

#### 	2.2、处理HTTP文件头

##### 			2.2.1、禁用缓存

​						默认情况下，浏览器会对显示的网页进行缓存。可以通过设置HTTP头的方法禁用缓存

​						<%

​								response.setHeader("Cache-Control", "no-store");

​								response.setDateheader("Expires", 0);

​						%>

##### 			2.2.2、设置页面自动刷新

​					response.setHeader("refresh", 10);            10秒自动刷新一次

##### 			2.2.3、定时跳转网页

​						response.setHeader("refresh", “10; URL=login.jsp");       10秒自动跳转到指定页面

#### 		2.3、设置输出缓冲

​				服务器要输出到客户端的内容不会直接写到客户端，而是先写到一个输出缓冲区，在计算机术语中，缓冲区被定义为暂时放置输入或输出资料的内存

 

### 3、session对象

#### 	3.1、创建及获取客户的会话

​			通过setAttribute()和getAttribute()来创建和获取

#### 	3.2、从会话中移动指定的绑定对象

​			removeAttribute(String name)

#### 	3.3、 销毁session

​			session.invalidate();

#### 	3.4、会话超时管理

​			session.setMaxInactiveInterval()

### 4、application对象

​	用于保存所有的应用程序中的公有数据。它在服务器启动时自动创建，服务器停止时销毁。

​	应用程序初始化参数在web.xml文件中进行配置。

#### 	4.1访问应用程序初始化参数

​		1、application.getInitParameter("url")    返回已命名的参数值

​		2、application.getAttributeNames()        返回已定义的应用程序初始化参数名的枚举

#### 	4.2、管理应用程序环境属性

### 5、out对象：用于在web浏览器输出信息，并且管理应用服务器上的输出缓冲区

#### 	5.1、向客户端输出数据

​			1、print()方法

​			2、println()方法

#### 	5.2、管理响应缓冲区

​			![1563167697207](C:\Users\HZ\AppData\Roaming\Typora\typora-user-images\1563167697207.png)

#### 6、其他内置对象

2、response

3、session

4、application

5、out 

6、其他内置对象：

##### 	（1）获取会话范围的pageContext对象

​			它可以获取JSP页面的其他对象，其创建和初始化都是由容器完成

![1563167905280](C:\Users\HZ\AppData\Roaming\Typora\typora-user-images\1563167905280.png)

##### 	（2）读取web.xml配置信息的config对象

​		主要用于获取服务器的配置信息。

![1563168038633](C:\Users\HZ\AppData\Roaming\Typora\typora-user-images\1563168038633.png)

##### 	（3）page对象

​		类似于 java 的 this

##### 	（4）获取异常信息的exception对象

![1563168102965](C:\Users\HZ\AppData\Roaming\Typora\typora-user-images\1563168102965.png)

