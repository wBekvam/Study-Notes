### Python 基础

### 一、字符串

##### 1、字符隐式连接

​	s = "hello" "word"

​	print(s)    #helloword

##### 2、转义字符

 \t  水平制表符

 \v  垂直制表符

\r   将光标跳至首行

##### 3、回文

通过切片获取一个字符的反转值--->判断回文数

s = "abd"

a = s[::-1]    #dba

##### 4、常用字符串的方法(s 为字符串)

1、s.isdigit()                          #判断字符串是否全为数字

2、s.isalpha()                        #判断字符串的字符是否全为英文字母

3、s.islower()                        #判断是否全为小写字母

4、s.isupper()                       #判断是否全为大写字母



5、s.center(width[,fill])        #将字符居中，左右默认用空白字符填充

​                                                        width 居中的长度

​														fill       空白填充默认空白字符

6、s.count(sub[,start[,end]])  #获取一个字符串字串的个数

7、s.strip()                                  #返回一个去掉左右空白字符的字符串

8、s.lstrip()                                  #去掉左边的空白字符

9、s.rstrip()                                 #去掉右边的空白字符

##### 5、常用的类型码

%s          字符串

%d          十进制整数

%o          八进制整数

%x          十六进制整数

%f           十进制浮点数

%%          相当于一个%

###### 占位符和类型码之间语法格式：

%[ - + 0 宽度 精度 ]类型码

- -左对齐（默认右对齐）
- +右对齐显示+号
- 0左侧补0
- 宽度 整个字符输出的宽度
- 精度 保留小数点多少位 默认6

### 二、循环

### 三、列表

##### 1、可通过函数生成列表：

##### L = list()

​	list(str)     str:必须是可迭代对象

##### 2、列表常用的方法

L.index(v[begin,[,end]])                           #返回v元素对应的下标

L.insert(index, obj)                                   # 将某个元素插放到列表的指定位置

L.count(x)                                                  #返回列表中某个元素的个数

L.remove(x)                                              #删除第一次出现在列表中的值

L.copy()                                                     #浅拷贝

L.append(str)                                           #向列表中追加另外一个元素

L.append(list)                                           #向列表中追加一个列表

L.clear()                                                     #清空列表

L.sort(reverse=False)                              # 将列表进行排序

L.reverse()                                                #列表的反转

L.pop(index)                                            #删除index对应的元素 

​																		删除后，会返回删除的值

​																		没有参数，默认删除最后一个元素

##### 3、文本解析的方法

split()     join()                                   #split 分割

1、split()                                           #默认分隔符为空格

​	s = "Hello word"

​	L = s.split(" ")                                    #L = ["Hello", "word"]            

2、join() 

​	s = "-".join(L)                                    #s--> "Hello-word"

##### 4、列表的推到式

###### 1--9之间所奇数的平方

L = [i**2 for i in range(1, 10)  if i % 2 != 0]

<=>

L = []

for i in range(1, 10):

​	if i % 2 !=0:

​		n = i**2

​		L.append(n)



### 三、函数

#### 1、参数的传递

##### 1、四种实参方式

###### 1、位置传参

def myfun(name, age, address):

​    pass

###### 2、序列传参

L = ("hah","hehe")

def myfun(*L):

​	pass

###### 3、关键字传参

myfn(name="zs", age="18", address="北京")

###### 4、字典关键字传参

d = {"name":"zs", "age": "18", "address":"北京"}

myfun(**d)

#### 2、四种形参方式

###### 1、位置形参

###### 2、星号元组形参

def myfun(*args):

​	pass

myfun(1, 2)

myfun(1)

###### 3、命名关键字形参

def myfun(*args, c, d):                             # * 前面是位置传参； * 后面是关键字传参

​	pass

###### 4、双星号字典命名关键字形参

def myfun(**kwargs):

​	pass



#### 注：命名关键字形参：                  个数确定，名称不可变

#### 	   双星号字典命名关键字形参     个数不确定 名称不固定

​         def fun(*args, **kwargs)            

### 四、函数

#### 1、函数的装饰器

##### 1、闭包

###### 1、闭包三要素：

​				1、在函数的内部在定义一个函数

​				2、内嵌函数要用到外部函数的变量

​				3、外部函数的返回值是内部函数的函数名	

eg:

def test(number):

​	def test_in(number_in):

​		print("number_in:",%number_in)

​	return test_in

fun = test(2)

fun(10)            #12

