[TOC]



------

# 第 1 章  spring概述

## 1.1  spring概述

### 1.1.1  spring 是什么

​		Spring 是分层的 Java SE/EE 应用 `full-stack` 轻量级开源框架，以 `IoC（Inverse Of Control： 反转控制）和 AOP（Aspect Oriented Programming：面向切面编程）为内核`，提供了展现层 Spring MVC 和持久层 Spring JDBC 以及业务层事务管理等众多的企业级应用技术，还能整合开源世界众多 著名的第三方框架和类库，逐渐成为使用最多的 Java EE 企业应用开源框架。

### 1.1.2  spring 的优势

**方便解耦，简化开发**

​        通过 Spring 提供的 IoC 容器，可以将对象间的依赖关系交由 Spring 进行控制，避免硬编码所造 成的过度程序耦合。用户也不必再为单例模式类、属性文件解析等这些很底层的需求编写代码，可 以更专注于上层的应用。

**AOP 编程的支持**

​		通过 Spring 的 AOP（Aspect Oriented Programming：面向切面编程） 功能，方便进行面向切面的编程，许多不容易用传统 OOP(Object Oriented Programming：面向对象程序设计) 实现的功能可以通过 AOP 轻松应付。

**声明式事务的支持**

​		可以将我们从单调烦闷的事务管理代码中解脱出来，通过声明式方式灵活的进行事务的管理， 提高开发效率和质量。

**方便程序的测试**

​		可以用非容器依赖的编程方式进行几乎所有的测试工作，测试不再是昂贵的操作，而是随手可 做的事情。

**方便集成各种优秀框架**

​		Spring 可以降低各种框架的使用难度，提供了对各种优秀框架（Struts、Hibernate、Hessian、Quartz 等）的直接支持。

**降低 JavaEE API 的使用难度**

​		Spring 对 JavaEE API（如 JDBC、JavaMail、远程调用等）进行了薄薄的封装层，使这些 API 的 使用难度大为降低。

**Java 源码是经典学习范例**

​		Spring 的源代码设计精妙、结构清晰、匠心独用，处处体现着大师对 Java 设计模式灵活运用以 及对 Java 技术的高深造诣。它的源代码无意是 Java 技术的最佳实践的范例。

### 1.1.3  spring 体系结构

![spring-overview](D:\笔记\插图\spring-overview.png)

# 第 2 章  IOC 的概念和作用

## 2.1  程序的耦合和解耦

### 2.1.1  什么是程序的耦合

​		耦合性(Coupling)，也叫耦合度，是对模块间关联程度的度量。耦合的强弱取决于模块间接口的复杂性、调 用模块的方式以及通过界面传送数据的多少。模块间的耦合度是指模块之间的依赖关系，包括控制关系、调用关 系、数据传递关系。模块间联系越多，其耦合性越强，同时表明其独立性越差( 降低耦合性，可以提高其独立 性)。耦合性存在于各个领域，而非软件设计中独有的，但是我们只讨论软件工程中的耦合。 在软件工程中，耦合指的就是就是对象之间的依赖性。对象之间的耦合越高，维护成本越高。因此对象的设计 应使类和构件之间的耦合最小。软件设计中通常用耦合度和内聚度作为衡量模块独立程度的标准。`划分模块的一个 准则就是高内聚低耦合。` 

​		**它有如下分类：** 

​		（1）内容耦合。当一个模块直接修改或操作另一个模块的数据时，或一个模块不通过正常入口而转入另 一个模块时，这样的耦合被称为内容耦合。内容耦合是最高程度的耦合，应该避免使用之。 

​		（2）公共耦合。两个或两个以上的模块共同引用一个全局数据项，这种耦合被称为公共耦合。在具有大 量公共耦合的结构中，确定究竟是哪个模块给全局变量赋了一个特定的值是十分困难的。 

​		（3） 外部耦合 。一组模块都访问同一全局简单变量而不是同一全局数据结构，而且不是通过参数表传 递该全局变量的信息，则称之为外部耦合。 

​		（4） 控制耦合 。一个模块通过接口向另一个模块传递一个控制信号，接受信号的模块根据信号值而进 行适当的动作，这种耦合被称为控制耦合。 

​		（5）标记耦合 。若一个模块 A 通过接口向两个模块 B 和 C 传递一个公共参数，那么称模块 B 和 C 之间 存在一个标记耦合。 

​		（6） 数据耦合。模块之间通过参数来传递数据，那么被称为数据耦合。数据耦合是最低的一种耦合形 式，系统中一般都存在这种类型的耦合，因为为了完成一些有意义的功能，往往需要将某些模块的输出数据作为另 一些模块的输入数据。

​		  (7） 非直接耦合 。两个模块之间没有直接关系，它们之间的联系完全是通过主模块的控制和调用来实 现的。

​		 **总结：** 

​		耦合是影响软件复杂程度和设计质量的一个重要因素，在设计上我们应采用以下原则：如果模块间必须 存在耦合，就尽量使用数据耦合，少用控制耦合，限制公共耦合的范围，尽量避免使用内容耦合。 内聚与耦合 内聚标志一个模块内各个元素彼此结合的紧密程度，它是信息隐蔽和局部化概念的自然扩展。内聚是从 功能角度来度量模块内的联系，一个好的内聚模块应当恰好做一件事。它描述的是模块内的功能联系。耦合是软件 结构中各模块之间相互连接的一种度量，耦合强弱取决于模块间接口的复杂程度、进入或访问一个模块的点以及通 过接口的数据。 程序讲究的是低耦合，高内聚。就是同一个模块内的各个元素之间要高度紧密，但是各个模块之 间的相互依存度却要不那么紧密。 

​		内聚和耦合是密切相关的，同其他模块存在高耦合的模块意味着低内聚，而高内聚的模块意味着该模块同其他 模块之间是低耦合。

​		在进行软件设计时，应力争做到高内聚，低耦合。

> 我们在开发中，有些依赖关系是必须的，有些依赖关系可以通过优化代码来解除的。

```java
示例代码：
/**
* 账户的业务层实现类
* @author hz
* @blog http://www.huangzun.top
* @date 2020/3/3
*/
public class AccountServiceImpl implements IAccountService {
	private IAccountDao accountDao = new AccountDaoImpl();
}
上面的代码表示：
	业务层调用持久层，并且此时业务层在依赖持久层的接口和实现类。如果此时没有持久层实现类，编译将不能通过。这种编译期依赖关系，应该在我们开发中杜绝。我们需要优化代码解决。

```

```java
再比如：
	早期我们的 JDBC 操作，注册驱动时，我们为什么不使用 DriverManager 的 register 方法，而是采用 Class.forName 的方式？
    /**
    * 通过jdbc的案例演示程序的耦合
    * @author hz
    * @blog http://www.huangzun.top
    * @date 2020/3/3
    */
public class JdbcDemo {
    public static void main(String[] args) throws SQLException {
        // 1、注册驱动
        DriverManager.registerDriver(new com.mysql.jdbc.Driver());
        // 2、获取连接
        Connection conn = DriverManager.getConnection("","","");
        // 3、获取数据库的预处理对象
        String sql = "";
        PreparedStatement pstm = conn.prepareStatement(sql);
        // 4、执行SQL, 得到结果集
        ResultSet rs = pstm.executeQuery();
        // 5、遍历结果集
        // 6、释放资源
        rs.close();
        pstm.close();
        conn.close();
    }
}
原因就是：
	我们的类依赖了数据库的具体驱动类（MySQL），如果这时候更换了数据库品牌（比如 Oracle），需要修改源码来重新数据库驱动。这显然不是我们想要的。

```

### 2.1.2  解决程序耦合的思路

```java
	当是我们学习 jdbc 时，是通过反射来注册驱动的，代码如下：
	Class.forName("com.mysql.jdbc.Driver");	//此处只是一个字符串

	此时的好处是，我们的类中不再依赖具体的驱动类，此时就算删除 mysql 的驱动 jar 包，依然可以编译（运行就不要想了，没有驱动不可能运行成功的）。
    实际开发中：
        编译期不依赖，运行期依赖
	同时，也产生了一个新的问题，mysql 驱动的全限定类名字符串是在 java 类中写死的，一旦要改还是要修改源码。
	解决这个问题也很简单，使用配置文件配置。
        1.使用反射创建对象，避免使用关键字new。
        2.通过读取配置文件来获取要创建的对象全限定类名。
```

### 2.1.3  工厂模式解耦

​		在实际开发中我们可以把三层的对象都使用配置文件配置起来，当启动服务器应用加载的时候，让一个类中的 方法通过读取配置文件，把这些对象创建出来并存起来。在接下来的使用的时候，直接拿过来用就好了。

​		 那么，这个读取配置文件，创建和获取三层对象的类就是工厂。

1. **存哪儿？**

   由于我们是很多对象，肯定要找个集合来存。由于有查找需求，选 Map。

   在应用加载时，创建一个 Map，用于存放三层对象。 我们把这个 map 称之为`容器`。

2. **工厂**

   工厂就是负责给我们从容器中获取指定对象的类。这时候我们获取对象的方式发生了改变。

​		

### 2.1.4  控制反转 ( Inversion Of Control )

**传统方法：**

​	我们在获取对象时，都是采用 new 的方式。**是主动的**

<img src="C:\Users\HZ\AppData\Roaming\Typora\typora-user-images\ioctt.png" alt="image-20200330175105274" style="zoom: 50%;" />

**现在：**

​	我们获取对象时，同时跟工厂要，有工厂为我们查找或者创建对象。**是被动的**

<img src="C:\Users\HZ\AppData\Roaming\Typora\typora-user-images\iocnow.png" alt="image-20200330175248648" style="zoom: 50%;" />

> 这种被动接收的方式获取对象的思想就是控制反转，它是 spring 框架的核心之一。

​		控制反转（Inversion of Control, IoC ）把创建对象的权利交给框架，是框架的重要特征，并非面向对象编程的专用术语。它包括依赖注入（Dependency Injection，DI）和依赖查找（Dependency Lookup）。

**明确 ioc 的作用：**

​		削减计算机程序的耦合(解除我们代码中的依赖关系)

耦合：程序间的依赖关系(类之间的依赖关系，方法之间的依赖关系)

解耦： 降低程序之间的依赖关系

# 第 3 章  使用 spring 的 IOC 解决程序耦合 

## 3.1  案例前期准备

​		我们使用的案例是，账户的业务层和持久层的依赖关系解决。。由于我们是使用 spring 解决依赖关系，并不是真正的要做增删改查操作，所以此时我们没必要写实体 类。

### 3.1.1  spring 的开发包

​	官网：http://spring.io/ 

​	下载地址： http://repo.springsource.org/libs-release-local/org/springframework/spring

**特别说明：** 

​	spring5 版本是用 jdk8 编写的，所以要求我们的 jdk 版本是 8 及以上。 

​	同时 tomcat 的版本要求 8.5 及以上。

### 3.1.2  创建一个Maven工程

​	在 pom.xml 导入spring依赖

```xml
<!--设置打包方式为jar-->
<packaging>jar</packaging>

<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${spring.version}</version>
    </dependency>
</dependencies>
```

### 3.1.3  创建业务层接口和实现类

```java
/**
 * 账户业务层的接口
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/3
 */
public interface IAccountService {

    /**
     * 模拟保存账户
     */
    void saveAccount();
}

/**
 * 账户的业务层实现类
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/3 
 */
public class AccountServiceImpl implements IAccountService {
   // 此处的依赖关系有待解决
   private IAccountDao accountDao = new AccountDaoImpl();

    public void saveAccount() {
        accountDao.saveAccount();
    }
}

```

### 3.1.4  创建持久层接口和实现类

```java
/**
 * 账户的持久层接口
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/3
 */
public interface IAccountDao {
    
    /**
     * 模拟保存账户
     */
    void saveAccount();
}

/**
  * 账户的持久层实现类
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/3
 */
public class AccountDaoImpl implements IAccountDao {

    public void saveAccount(){
        System.out.println("保存账户");
    }
}

```

## 3.2  基于XML的配置

### 3.2.1  创建配置文件

	1. 在src --> main --> resource 下新建bean.xml ( 名称不能为中文名 )

### 3.2.2  给文件导入约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
 	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 	xsi:schemaLocation="http://www.springframework.org/schema/beans
http://www.springframework.org/schema/beans/spring-beans.xsd">
    
</beans>

```

### 3.2.3  让 spring 管理资源，在配置文件中配置 service 和 dao

```xml
 	<!-- bean 标签：用于配置让 spring 创建对象，并且存入 ioc 容器之中
         id 属性：对象的唯一标识。
         class 属性：指定要创建对象的全限定类名
    -->

 	<!--把对象的创建交给spring管理-->
    <bean id="accountService" class="com.hz.service.impl.AccountServiceImpl"></bean>

    <bean id="accountDao" class="com.hz.dao.impl.AccountDaoImpl"></bean>
```

### 3.2.4  测试配置是否成功

```java
 /**
  * 模拟表现层，用于调用业务层
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/3
 */
public class client {
    /**
     * 获取Spring的IOC核心容器, 根据id获取对象
     *    ApplicationContext三个实现类：
     *      ClassPathXmlApplicationContext：可以加载类路径下的配置文件，要求文件必须在类路径下
     *      FileSystemXmlApplicationContext: 可以加载磁盘任意路径下的配置文件（必须有访问权限）
     *      AnnotationConfigApplicationContext: 用于读取注解创建容器
     * @param args
     */
    public static void main(String[] args) {
        // 1、获取核心容器对象
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        // 2、根据id获取Bean对象
        IAccountService service =  (IAccountService)ac.getBean("accountService");
        IAccountDao dao = ac.getBean("accountDao", IAccountDao.class);
        System.out.println(service);
        System.out.println(dao);
    }
}
```

运行结果：

![image-20200330192650248](C:\Users\HZ\AppData\Roaming\Typora\typora-user-images\image-20200330192650248.png)

## 3.3  spring 基于XML 的 IOC 细节

### 3.3.1  spring 中工厂的类结构图

![image-20200330192834747](C:\Users\HZ\AppData\Roaming\Typora\typora-user-images\image-20200330192834747.png)

#### 3.3.1.1  spring 的核心容器

> BeanFactory 才是 Spring 容器中的顶层接口。 ApplicationContext 是它的子接口。

**BeanFactory 和 ApplicationContext 的区别：**

```
ApplicationContext:   单列对象适用
      它在构建核心容器时，创建对象采取的策略是采用立即加载的方式。
      也就是说，一读取完配置文件马上就创建配置文件中配置对象
      
BeanFactory:   多例对象适用
      它在构建核心容器时，创建对象采取的措施是采用延迟加载的方式。
      也就是说，什么时候根据id获取对象了，什么时候才能真正创建对象
      
总结: 创建对象的时间点不一样
```

#### 3.3.1.2  ApplicationContext 接口的实现类

>  获取Spring的IOC核心容器, 根据id获取对象

```
ApplicationContext三个实现类：
  	ClassPathXmlApplicationContext：
        加载类路径下的配置文件，要求文件必须在类路径下 → 推荐使用这种
    FileSystemXmlApplicationContext：
        可以加载磁盘任意路径下的配置文件（必须有访问权限）
    AnnotationConfigApplicationContext:
        当我们使用注解配置容器对象时，需要使用此类来创建 spring 容器。它用来读取注解创建容器。
```

### 3.3.2  IOC 中 bean 标签和管理对象细节

#### 3.3.2.1  bean 标签

```
作用：
	用于配置对象让 spring 来创建的。
	默认情况下它调用的是类中的无参构造函数。如果没有无参构造函数则不能创建成功。
属性：
	id：给对象在容器中提供一个唯一标识。用于获取对象。
	class：指定类的全限定类名。用于反射创建对象。默认情况下调用无参构造函数。
	scope：指定对象的作用范围。
		* singleton :默认值，单例的.
        * prototype :多例的.
        * request :WEB 项目中,Spring 创建一个 Bean 的对象,将对象存入到 request 域中.
        * session :WEB 项目中,Spring 创建一个 Bean 的对象,将对象存入到 session 域中.
        * global session :WEB 项目中,应用在 Portlet 环境.如果没有 Portlet 环境那么globalSession 相当于 session.
    init-method：指定类中的初始化方法名称。
    destroy-method：指定类中销毁方法名称。
```

#### 3.3.2.2  bean 生命周期

```
 bean对象的生命周期
 	单例对象：scope="singleton"
 		一个应用只有一个对象的实例。它的作用范围就是整个引用。
        生命周期：
            出生：当容器创建时出生
            活着：只要容器存在，对象一直活着
            死亡：容器销毁，对象消亡
            总结：单例对象的生命周期和容器相同
	多例对象：scope="prototype"
		每次访问对象时，都会重新创建对象实例。
		生命周期：
            出生：当使用对象时，spring框架为我们创建
            活着：对象只要在使用过程中，就一直活着
            死亡：当对象长时间不用，且没有别的对象引用时，由Java的垃圾回收器回收
```

#### 3.3.2.3  实例化 Bean 的三种方式 

> 第一种方式： 使用默认无参构造函数
>
> ​		在spring的配置文件中使用bean标签，配以id和class之后,且没有其他属性和标签时，采用的就是默认构造函数创建bean对象，此时如果类中没有默认构造函数，则无法创建。
>
> ​		如果 bean 中没有默认无参构造函数，将会创建失败。

```xml
<bean id="accountService" class="com.hz.service.impl.AccountServiceImpl"></bean>
```

> 第二种方式：spring 管理静态工厂-使用静态工厂的方法创建对象
>
> ​	此种方式是：
>
> ​		先把工厂的创建交给 spring 来管理。
>
> ​		在使用工厂的 bean 来调用里面的方法
>
> ​			factory-bean 属性：用于指定实例工厂 bean 的 id。
>
> ​			factory-method 属性：用于指定实例工厂中创建对象的方法。

```java
/**
 * 模拟一个工厂类，创建业务层实现类（该类可能存在于jar包中，我们无法通过修改源码的方式来提供默认构造函数）
 * 此工厂创建对象，必须现有工厂实例对象，再调用方法
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/4
 */
public class InstanceFactory {
    public IAccountService getAccountService(){
        return new AccountServiceImpl();
    }
}
```

```xml
<bean id="instanceFactory" class="com.hz.factory.InstanceFactory"></bean>
<bean id="accountService" factory-bean="instanceFactory" factory-method="getAccountService"></bean>
```

### 3.3.3  spring 的依赖注入

#### 3.3.3.1  依赖注入的概念

```
	依赖注入( Dependency Injection, DI)，在当前类需要用到其他类的对象，有spring为我们提供，我们只需要在配置文件中说明依赖关系的维护。它是 spring 框架核心 ioc 的具体实现。
	我们的程序在编写时，通过控制反转( ioc )把对象的创建交给了 spring，但是代码中不可能出现没有依赖的情况。
	ioc 解耦只是降低他们的依赖关系，但不会消除。例如：我们的业务层仍会调用持久层的方法。
	那这种业务层和持久层的依赖关系，在使用 spring 之后，就让 spring 来维护了。 简单的说，就是坐等框架把持久层对象传入业务层，而不用我们自己去获取。
```

#### 3.3.3.2  构造函数注入

> ​		顾名思义，就是使用类中的构造函数，给成员变量赋值。注意，赋值的操作不是我们自己做的，而是通过配置 的方式，让 spring 框架来为我们注入。

```java
/**
 * 账户的业务层实现类
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/3
 */
public class AccountServiceImpl implements IAccountService {
    private String name;
    private Integer age;
    private Date birthday;

    public AccountServiceImpl(String name, Integer age, Date birthday) {
        this.name = name;
        this.age = age;
        this.birthday = birthday;
    }

    public void saveAccount() {
        System.out.println("service-->saveAccount" + name + ", " + age + ", " + birthday);
    }
}
```

```xml
<!--
    构造函数注入：
        使用的标签：constructor-arg
        标签出现的位置：bean标签的内部
        标签中的属性:
            type: 用于指定要注入的数据的数据类型，该数据类型也是构造函数中某个或某些参数的类型
            index: 用于指定要注入的数据给构造函数中指定索引位置的参数赋值，索引的位置从0开始
            name: 用于指定给构造函数中指定名称的参数赋值（常用）
            ====== 以上三个用于给指定构造函数中哪个参数赋值 =========
            value: 用于提供基本类型和String类型的数据
            ref: 用于指定其他的bean类型数据，他指的是在spring的Ioc核心容器中出现过的bean对象

        优势：
            在获取bean对象时，注入数据是必须的操作，否则对象无法创建成功
        弊端：
            改变了bean对象的实例化方式，是我们在创建对象时，如果不用到这些数据时，也必须提供
-->

 	<bean id="accountService" class="com.hz.service.impl.AccountServiceImpl">
        <constructor-arg name="name" value="张三"></constructor-arg>
        <constructor-arg name="age" value="18"></constructor-arg>
        <constructor-arg name="birthday" ref="now" ></constructor-arg>
    </bean>
 	<!--配置一个日期对象-->
    <bean id="now" class="java.util.Date"></bean>
```

#### 3.3.3.3  set 方法注入

> 顾名思义，就是在类中提供需要注入成员的 set 方法。

```java
/**
 * 账户的业务层实现类
 */
public class AccountServiceImpl implements IAccountService {

    private String name;
    private Integer age;
    private Date birthday;

    public void setName(String name) {
        this.name = name;
    }
    
    public void setAge(Integer age) {
        this.age = age;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }


    public void saveAccount() {
        System.out.println("service-->saveAccount   " + name + ", " + age + ", " + birthday);
    }
}
```

```xml
<!--
 	set方法注入
        涉及内容：property
        出现的位置: bean标签的内部
        标签的属性：
            name: 用于指定注入时所调用的set方法名称
            value: 用于提供基本类型和String类型的数据
            ref: 用于指定其他的bean类型数据，他指的是在spring的Ioc核心容器中出现过的bean对象

        优势：
            创建对象时没有明确的限制，可以直接使用默认构造函数
        弊端：
            如果某个成员必须有值，则获取对象是有可能set方法没有执行
-->

    <bean id="accountService" class="com.hz.service.impl.AccountServiceImpl">
        <property name="name" value="张三"></property>
        <property name="age" value="18"></property>
        <property name="birthday" ref="now"></property>
    </bean>
    <!--配置一个日期对象-->
    <bean id="now" class="java.util.Date"></bean>
```

#### 3.3.3.4  注入集合属性

> 顾名思义，就是给类中的集合成员传值，它用的也是set方法注入的方式，只不过变量的数据类型都是集合。

```java
public class AccountServiceImpl implements IAccountService {

    private  String [] myStrs;
    private List<String> myList;
    private Set<String> mySet;
    private Map<String, String> myMap;
    private Properties myPros;

    public void setMyStrs(String[] myStrs) {
        this.myStrs = myStrs;
    }

    public void setMyList(List<String> myList) {
        this.myList = myList;
    }

    public void setMySet(Set<String> mySet) {
        this.mySet = mySet;
    }

    public void setMyMap(Map<String, String> myMap) {
        this.myMap = myMap;
    }

    public void setMyPros(Properties myPros) {
        this.myPros = myPros;
    }

    public void saveAccount() {
        System.out.println(Arrays.toString(myStrs));
        System.out.println(myList);
        System.out.println(mySet);
        System.out.println(myMap);
        System.out.println(myPros);
    }
}
```

```xml
<!-- 
    注入集合数据
        List 结构的：
        	array,list,set
        Map 结构的
        	map,entry,props,prop
-->
     <bean id="accountService" class="com.hz.service.impl.AccountServiceImpl">
            <property name="myStrs">
                <set>
                    <value>AAA</value>
                    <value>BBB</value>
                </set>
            </property>

            <property name="myList">
                <array>
                    <value>CCC</value>
                    <value>DDD</value>
                </array>
            </property>

            <property name="mySet">
                <array>
                    <value>CCC</value>
                    <value>DDD</value>
                </array>
            </property>

            <property name="myMap">
                <map>
                    <entry key="test1" value="test1"></entry>
                    <entry key="test2">
                        <value>test2</value>
                    </entry>
                </map>
            </property>

            <property name="myPros">
                <props>
                    <prop key="test">pros</prop>
                </props>
            </property>
        </bean>
```

## 3.4  基于注解的 IOC 配置 

###  3.4.1  明确  写在最前

```
	学习基于注解的 IoC 配置，大家脑海里首先得有一个认知，即注解配置和 xml 配置要实现的功能都是一样 的，都是要降低程序间的耦合。只是配置的形式不一样。 
	关于实际的开发中到底使用xml还是注解，每家公司有着不同的使用习惯。所以这两种配置方式我们都需要掌握。
```

### 3.4.2  案例 前期准备

> 使用 spring 的 IoC 的实现账户的 CRUD

#### 3.4.2.1 新建Maven工程  添加依赖

> 版本自行选择

```xml
<!-- 打包方式 -->
<packaging>jar</packaging>
<!-- 依赖 -->
<dependencies>
    <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
       <version>${spring.version}</version>
    </dependency>

    <dependency>
        <groupId>commons-dbutils</groupId>
        <artifactId>commons-dbutils</artifactId>
        <version>1.4</version>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.44</version>
    </dependency>

    <dependency>
        <groupId>c3p0</groupId>
        <artifactId>c3p0</artifactId>
        <version>0.9.1.2</version>
    </dependency>

    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.10</version>
    </dependency>
</dependencies>
```

#### 3.4.2.2  创建数据库和编写实体类

新建数据库:

```sql
create table account(
    id int primary key auto_increment,
    name varchar(40),
    money float
)character set utf8 collate utf8_general_ci;

insert into account(name,money) values('aaa',1000);
insert into account(name,money) values('bbb',1000);
insert into account(name,money) values('ccc',1000);
```

编写实体类：

```java
/**
 * 账户的实体类
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/7 
 */
public class Account implements Serializable {

    private Integer id;
    private String name;
    private Float money;

    @Override
    public String toString() {
        return "Account{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", money=" + money +
                '}';
    }
    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public Float getMoney() {
        return money;
    }

    public void setMoney(Float money) {
        this.money = money;
    }
}
```

#### 3.4.2.3  编写持久层代码

```java
/**
 *  账户的持久层接口
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/7 
 */
public interface IAccountDao {
    /**
     * 查询所有
     * @return
     */
    List<Account> findAllAccount();

    /**
     * 查询一个
     * @return
     */
    Account findAccountById(Integer accountId);

    /**
     * 保存账户
     * @param account
     */
    void saveAccount(Account account);

    /**
     * 更新账户
     * @param account
     */
    void updateAccount(Account account);

    /**
     * 删除账户
     * @param accountId
     */
    void deleteAccount(Integer accountId);
}
```

持久层实现类

```java
/**
  * 账户的持久层实现类
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/7 
 */
public class AccountDaoImpl implements IAccountDao {
    private QueryRunner runner;

    public void setRunner(QueryRunner runner) {
        this.runner = runner;
    }

    @Override
    public List<Account> findAllAccount() {
        try{
            return runner.query("select * from account", new BeanListHandler<>(Account.class));
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }

    @Override
    public Account findAccountById(Integer accountId) {
        try {
            return runner.query("select * from account where id = ?", new BeanHandler<>(Account.class), accountId);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    @Override
    public void saveAccount(Account account) {
        try{
            runner.update("insert into account(name,money)values(?,?)", account.getName(),account.getMoney());
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }

    @Override
    public void updateAccount(Account account) {
        try{
            runner.update("update account set name=?,money=? where id=?", account.getName(),account.getMoney());
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }

    @Override
    public void deleteAccount(Integer accountId) {
        try{
            runner.update("delete from account where id=?", accountId);
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }
}
```

#### 3.4.2.4  编写 编写业务层代码

```java
/**
  *  账户的业务层接口
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/7 
 */
public interface IAccountService {

    /**
     * 查询所有
     * @return
     */
    List<Account> findAllAccount();

    /**
     * 查询一个
     * @return
     */
    Account findAccountById(Integer accountId);

    /**
     * 保存账户
     * @param account
     */
    void saveAccount(Account account);

    /**
     * 更新账户
     * @param account
     */
    void updateAccount(Account account);

    /**
     * 删除账户
     * @param accountId
     */
    void deleteAccount(Integer accountId);

}
```

业务层实现类

```java
/**
  *   账户的业务层实现类
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/7 
 */
public class AccountServiceImpl implements IAccountService {

    private IAccountDao accountDao;

    public void setAccountDao(IAccountDao accountDao) {
        this.accountDao = accountDao;
    }

    @Override
    public List<Account> findAllAccount() {
        return accountDao.findAllAccount();
    }

    @Override
    public Account findAccountById(Integer accountId) {
        return accountDao.findAccountById(accountId);
    }


    @Override
    public void saveAccount(Account account) {
        accountDao.saveAccount(account);
    }

    @Override
    public void updateAccount(Account account) {
        accountDao.updateAccount(account);
    }

    @Override
    public void deleteAccount(Integer accountId) {
        accountDao.deleteAccount(accountId);
    }
}
```

#### 3.4.2.5  创建并编写配置文件

​	在 src --> main --> resources 添加配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	   xsi:schemaLocation="http://www.springframework.org/schema/beans 
       http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--配置Service-->
    <bean id="accountService" class="com.hz.service.impl.AccountServiceImpl">
        <!--注入dao-->
        <property name="accountDao" ref="accountDao"></property>
    </bean>

    <!--配置Dao对象-->
    <bean id="accountDao" class="com.hz.dao.impl.AccountDaoImpl">
        <property name="runner" ref="runner"></property>
    </bean>

    <!--配置QueryRunner-->
    <bean id="runner" class="org.apache.commons.dbutils.QueryRunner" scope="prototype">
        <!--注入数据源-->
        <constructor-arg name="ds" ref="dateSource"></constructor-arg>
    </bean>

    <!--配置数据源-->
    <bean id="dateSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <!--连接数据库必备信息-->
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/eesy?useUnicode=true&amp;characterEncoding=utf8"></property>
        <property name="user" value="root"></property>
        <property name="password" value="root"></property>
    </bean>
</beans>
```

#### 3.4.2.6  测试案例

添加测试代码

```java
/**
 * 测试代码
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/7 
 */
public class AccountServiceTest {
  
    @Test
    public void TestFindAll(){
        // 1. 获取容器
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        // 2. 得到业务对象
        IAccountService as = ac.getBean("accountService", IAccountService.class);
        List<Account> accounts = as.findAllAccount();
        for (Account account : accounts){
            System.out.println(account);
        }
    }

    @Test
    public void TestFindOne(){
        // 1. 获取容器
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        // 2. 得到业务对象
        IAccountService as = ac.getBean("accountService", IAccountService.class);
        Account account = as.findAccountById(1);
        System.out.println(account);

    }
    @Test
    public void TestSave(){
        // 1. 获取容器
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        // 2. 得到业务对象
        IAccountService as = ac.getBean("accountService", IAccountService.class);
        Account account = new Account();
        account.setName("save");
        account.setMoney(3000.0f);
        as.saveAccount(account);

    }
    @Test
    public void TestUpdate(){
        // 1. 获取容器
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        // 2. 得到业务对象
        IAccountService as = ac.getBean("accountService", IAccountService.class);
        Account account = as.findAccountById(4);
        account.setName("update");
        as.updateAccount(account);
    }
    
    @Test
    public void TestDelete(){
        // 1. 获取容器
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        // 2. 得到业务对象
        IAccountService as = ac.getBean("accountService", IAccountService.class);
        as.deleteAccount(4);

    }

}
```

#### 3.4.2.7  分析测试了中的问题

> ​		通过上面的测试类，我们可以看出，每个测试方法都重新获取了一次 spring 的核心容器，造成了不必要的重复代码，增加了我们开发的工作量。这种情况，在开发中应该避免发生。
>
> ​		可能有人想到，我们把容器的获取定义到类中去。例如：>

```java
/**
 * 方法一
 * 测试代码
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/7 
 */
public class AccountServiceTest {
    private ApplicationContext ac;
    private IAccountService as;

    @Before
    public void init(){
        // 1. 获取容器
        ac = new ClassPathXmlApplicationContext("bean.xml");
        // 2. 得到业务对象
        as = ac.getBean("accountService", IAccountService.class);
    }
    @Test
    public void TestFindAll(){
        // 1. 获取容器
        ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
        // 2. 得到业务对象
        IAccountService as = ac.getBean("accountService", IAccountService.class);
        // 3. 执行方法
        List<Account> accounts = as.findAllAccount();
        for (Account account : accounts){
            System.out.println(account);
        }

    }
}

/**
 * 方法二
 * 测试代码
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/7 
 */
 public class AccountServiceTest {
    private ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
    private IAccountService as ac.getBean("accountService", IAccountService.class);
}
```

这种方式虽然能解决问题，但是扔需要我们自己写代码来获取容器。 

能不能测试时直接就编写测试方法，而不需要手动编码来获取容器呢？

### 3.4.3   基于注解的 IOC 配置 

> 我们在讲解注解配置时，采用上一章节的案例，把 spring 的 xml 配置内容改为使用注解逐步实现。

**注意：**

​	在基于注解的配置中，需要 aop 的 jar 包。

<img src="C:\Users\HZ\AppData\Roaming\Typora\typora-user-images\image-20200330215207773.png" alt="image-20200330215207773" style="zoom: 50%;" />

#### 3.4.3.1  使用注解配置管理的资源

```java
/**
 * 账户的业务层实现类
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/7 
 */
@Service("accountService")
public class AccountServiceImpl implements IAccountService {

    @Autowired
    private IAccountDao accountDao;

    @Override
    public List<Account> findAllAccount() {
        return accountDao.findAllAccount();
    }

    @Override
    public Account findAccountById(Integer accountId) {
        return accountDao.findAccountById(accountId);
    }

    @Override
    public void saveAccount(Account account) {
        accountDao.saveAccount(account);
    }

    @Override
    public void updateAccount(Account account) {
        accountDao.updateAccount(account);
    }

    @Override
    public void deleteAccount(Integer accountId) {
        accountDao.deleteAccount(accountId);
    }
}

/**
 * 账户的持久层实现类
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/7 
 */
@Repository("accountDao")
public class AccountDaoImpl implements IAccountDao {

    @Autowired
    private QueryRunner runner;

    @Override
    public List<Account> findAllAccount() {
        try{
            return runner.query("select * from account", new BeanListHandler<>(Account.class));
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }

    @Override
    public Account findAccountById(Integer accountId) {
        try {
            return runner.query("select * from account where id = ?", new BeanHandler<>(Account.class), accountId);
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }

    @Override
    public void saveAccount(Account account) {
        try{
            runner.update("insert into account(name,money)values(?,?)", account.getName(),account.getMoney());
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }

    @Override
    public void updateAccount(Account account) {
        try{
            runner.update("update account set name=?,money=? where id=?", account.getName(),account.getMoney());
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }

    @Override
    public void deleteAccount(Integer accountId) {
        try{
            runner.update("delete from account where id=?", accountId);
        }catch (Exception e){
            throw new RuntimeException(e);
        }
    }
}
```

#### 3.4.3.2  创建 spring 的 xml 配置文件并开启对注解的支持

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context.xsd">

    <!--告知spring在创建容器时要扫描的包-->
    <context:component-scan base-package="com.hz"></context:component-scan>

    <!--配置QueryRunner-->
    <bean id="runner" class="org.apache.commons.dbutils.QueryRunner" scope="prototype">
        <!--注入数据源-->
        <constructor-arg name="ds" ref="dateSource"></constructor-arg>
    </bean>

    <!--配置数据源-->
    <bean id="dateSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <!--连接数据库必备信息-->
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/eesy?useUnicode=true&amp;characterEncoding=utf8"></property>
        <property name="user" value="root"></property>
        <property name="password" value="root"></property>
    </bean>
</beans>
```

### 3.4.4  常用注解

#### 2.3.4.1  用于创建对象的

​	相当于：```<bean id="" class="">```

##### 2.3.4.1.1  @Component

```
作用：
	把资源让 spring 来管理。相当于在 xml 中配置一个 bean。
属性：
	value：指定 bean 的 id。如果不指定 value 属性，默认 bean 的 id 是当前类的类名。首字母小写。
```

##### 2.3.4.1.2  @Controller @Service @Repository

```
他们三个注解都是针对一个的衍生注解，他们的作用及属性都是一模一样的。
他们只不过是提供了更加明确的语义化。
	@Controller：一般用于表现层的注解。
	@Service：一般用于业务层的注解。
	@Repository：一般用于持久层的注解。
细节：如果注解中有且只有一个属性要赋值时，且名称是 value，value 在赋值是可以不写。

```

#### 2.3.4.2   用于注入数据的

​	相当于：  `<property name="" value=""></property>`

​					  `<property name="" ref=""></property>`

##### 2.3.4.2.1  @Autowired

```
作用：
	自动按照类型注入。当使用注解注入属性时，set 方法可以省略。它只能注入其他 bean 类型。当有多个
类型匹配时，使用要注入的对象变量名称作为 bean 的 id，在 spring 容器查找，找到了也可以注入成功。找不到
就报错。
```

##### 2.3.4.2.2  @Qualifier

```
作用：
	在自动按照类型注入的基础之上，再按照 Bean 的 id 注入。它在给字段注入时不能独立使用，必须和
@Autowire 一起使用；但是给方法参数注入时，可以独立使用。

属性：
	value：指定 bean 的 id。

```

##### 2.3.4.2.3  @Resource

```
作用：
	直接按照 Bean 的 id 注入。它也只能注入其他 bean 类型。
	
属性：
	name：指定 bean 的 id。
```

##### 2.3.4.2.4  @Value

```
作用：
	注入基本数据类型和 String 类型数据的
	
属性：
	value：用于指定值
```

#### 2.3.4.3  用于改变作用范围的

​	相当于：```<bean id="" class="" scope="">```

##### 2.3.4.3.1  @scope

```
作用：
	指定 bean 的作用范围。
属性：
	value：指定范围的值。
	取值：
 		singleton:单例的（默认值）
		prototype:多例的
		request：作用于web应用的请求范围
		session:作用于web应用的会话范围
		global-session:作用于集群环境的会话范围（全局会话范围）,当不是集群环境时，他就是session

```

#### 2.3.4.4  和生命周期相关的

​	相当于：```<bean id="" factory-bean="" factory-method=""></bean>```

##### 2.3.4.4.1  @PostConstruct

​	用于指定初始化方法。

##### 2.3.4.4.1  @PreDestroy

​	用于指定销毁方法。

### 3.4.5  关于 Spring 注解和 XML 的选择问题

**注解的优势：**

 	配置简单，维护方便（我们找到类，就相当于找到了对应的配置）。 

**XML 的优势：** 

​	修改时，不用改源码。不涉及重新编译和部署。 Spring 管理 Bean 方式的比较：

<img src="C:\Users\HZ\AppData\Roaming\Typora\typora-user-images\image-20200331001000192.png" alt="image-20200331001000192" style="zoom: 50%;" />

## 3.5  spring 的纯注解配置 

​	写到此处，基于注解的 IoC 配置已经完成，但是大家都发现了一个问题：我们依然离不开 spring 的 xml 配 置文件，那么能不能不写这个 bean.xml，所有配置都用注解来实现呢？

#### 3.5.1  待改造的问题

> 我们发现，之所以我们现在离不开 xml 配置文件，是因为我们有一句很关键的配置

```xml
    <!-- 告知spring框架在，读取配置文件，创建容器时，扫描注解，依据注解创建对象，并存入容器中 -->
    <context:component-scan base-package="com.hz"></context:component-scan>
	<!--
    如果他要也能用注解配置，那么我们就离脱离 xml 文件又进了一步。
    另外，数据源和 JdbcTemplate 的配置也需要靠注解来实现。
	-->
	<!--配置QueryRunner-->
    <bean id="runner" class="org.apache.commons.dbutils.QueryRunner" scope="prototype">
        <!--注入数据源-->
        <constructor-arg name="ds" ref="dateSource"></constructor-arg>
    </bean>

    <!--配置数据源-->
    <bean id="dateSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <!--连接数据库必备信息-->
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/eesy?useUnicode=true&amp;characterEncoding=utf8"></property>
        <property name="user" value="root"></property>
        <property name="password" value="root"></property>
    </bean>
```

#### 3.5.2  与配置有关的注解

##### 3.5.2.1  @Configuration

```
作用：
	用于指定当前类是一个 spring 配置类，当创建容器时会从该类上加载注解。获取容器时需要使用
AnnotationApplicationContext(有@Configuration 注解的类.class)。

属性：
	value:用于指定配置类的字节码
```

##### 3.5.2.2  @ComponentScan

```
作用：
	用于指定 spring 在初始化容器时要扫描的包。作用和在 spring 的 xml 配置文件中的：
<context:component-scan base-package="com.hz"/>是一样的。

属性：
	basePackages：用于指定要扫描的包。和该注解中的 value 属性作用一样。

```

##### 3.5.2.3  @Bean

```
作用：
	该注解只能写在方法上，表明使用此方法创建一个对象，并且放入 spring 容器。
	
属性：
	name：给当前@Bean 注解方法创建的对象指定一个名称(即 bean 的 id）。
```

##### 3.5.2.4  @Import

```
作用：
	用于导入其他的配置类 
	
属性：
	value: 用于指定其他配置类的字节码
	当我们使用Import的注解之后，有Import注解的类就是父配置类，导入的就是子配置类
```

##### 3.5.2.5  @PropertySource

```
作用：
	用于加载.properties 文件中的配置。例如我们配置数据源时，可以把连接数据库的信息写到properties 配置文件中，就可以使用此注解指定 properties 配置文件的位置。

属性：
	value：用于指定 properties 文件位置。如果是在类路径下，需要写上 classpath:
    关键词：classpath, 表示类路径下
```

**示例代码**

```java
/**
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/7
 */
@Configuration
@ComponentScan(basePackages = "com.hz")
@Import(JdbcConfig.class)
@PropertySource("classpath: jdbcConfig.properties")
public class SpringConfiguration {

    /**
     * 用于创建一个QueryRunner对象
     * @param dataSource
     * @return
     */
    @Bean(name = "runner")
    public QueryRunner createQueryRunner(DataSource dataSource){
        return new QueryRunner(dataSource);
    }
}
```

```java
/**
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/7
 */
public class JdbcConfig {

    @Value("${jdbc.driver}")
    private String driver;

    @Value("${jdbc.url}")
    private String url;

    @Value("${jdbc.username}")
    private String username;

    @Value("jdbc.password")
    private String password;

    /**
     * 创建数据源对象
     * @return
     */
    @Bean(name = "dataSource")
    public DataSource createDataSource(){
        try{
            ComboPooledDataSource ds = new ComboPooledDataSource();
            ds.setDriverClass(driver);
            ds.setJdbcUrl(url);
            ds.setUser(username);
            ds.setPassword(password);
            return ds;
        } catch (Exception e) {
            throw new RuntimeException(e);
        }
    }
}
```

在  resources 添加文件 JdbcConfig.properties

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=dbc:mysql://localhost:3306/eesy
jdbc.username=root
jdbc.password=root
```

#### 3.5.3  通过注解获取容器

```java
ApplicationContext ac =
new AnnotationConfigApplicationContext(SpringConfiguration.class);
```

# 第 4 章  spring 整合 Junit

> 当使用spring 5.x版本时，要求Junit版本4.12 以上

## 4.1  测试类中的问题和解决思路

### 4.1.1  问题

```java
// 在测试类中，每个测试方法都有以下两行代码：
ApplicationContext ac = new ClassPathXmlApplicationContext("bean.xml");
IAccountService as = ac.getBean("accountService", IAccountService.class);
// 这两行代码的作用是获取容器，如果不写的话，直接会提示空指针异常。所以又不能轻易删掉。
```

### 4.1.2  解决思路分析


		针对上述问题，我们需要的是程序能自动帮我们创建容器。一旦程序能自动为我们创建 spring 容器，我们就无须手动创建了，问题也就解决了。
		我们都知道，junit 单元测试的原理（在 web 阶段课程中讲过），但显然，junit 是无法实现的，因为它自己都无法知晓我们是否使用了 spring 框架，更不用说帮我们创建 spring 容器了。不过好在，junit 给我们暴露了一个注解，可以让我们替换掉它的运行器。
		这时，我们需要依靠 spring 框架，因为它提供了一个运行器，可以读取配置文件（或注解）来创建容器。我们只需要告诉它配置文件在哪就行了。


## 4.2  配置步骤

1  添加依赖

```xml
    <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>4.10</version>
    </dependency>
```

2   使用@RunWith 注解替换原有运行器

```java
@RunWith(SpringJUnit4ClassRunner.class)
public class AccountServiceTest {
	
}
```

3   使用@ContextConfiguration 指定 spring 配置文件的位置

> @ContextConfiguration 注解： 
>
> ​	locations 属性：用于指定配置文件的位置。如果是类路径下，需要用 classpath:表明 	classes 属性：用于指定注解的类。当不使用 xml 配置时，需要用此属性指定注解类的位置。

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfiguration.class)
public class AccountServiceTest {

}
```

4  使用@Autowired 给测试类中的变量注入数据

```JAVA
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfiguration.class)
public class AccountServiceTest {
	@Autowired
	private IAccountService as ;
}
```

## 4.3  为什么不把测试类配到 xml 中

```
在解释这个问题之前，先解除大家的疑虑，配到 XML 中能不能用呢？
	答案是肯定的，没问题，可以使用。
那么为什么不采用配置到 xml 中的方式呢？
	这个原因是这样的：
		第一：当我们在 xml 中配置了一个 bean，spring 加载配置文件创建容器时，就会创建对象。
		第二：测试类只是我们在测试功能时使用，而在项目中它并不参与程序逻辑，也不会解决需求上的问题，所以创建完了，并没有使用。那么存在容器中就会造成资源的浪费。
	所以，基于以上两点，我们不应该把测试配置到 xml 文件中。
```

