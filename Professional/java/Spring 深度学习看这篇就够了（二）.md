# 第 5 章   AOP 的相关概念

## 5.1  AOP 概述

### 5.1.1  什么是 AOP

![image-20200331093815232](C:\Users\HZ\AppData\Roaming\Typora\typora-user-images\image-20200331093815232.png)

​		简单的说它就是把我们程序重复的代码抽取出来，在需要执行的时候，使用动态代理的技术，在不修改源码的 基础上，对我们的已有方法进行增强。

### 5.1.2  AOP 的作用及优势

作用：

​		在程序运行期间，不修改源码对已有方法进行增强。 

优势： 

​		减少重复代码 

​		提高开发效率 

​		维护方便

### 5.1.3  AOP  的实现方式

​		使用动态代理技术

## 5.2  AOP 的具体应用

### 5.2.1  案例中的问题

```java
/**
 * 账户的业务层实现类
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/5 
 */
public class AccountServiceImpl implements IAccountService {
   // 此处的依赖关系有待解决
   private IAccountDao accountDao = new AccountDaoImpl();

    public void saveAccount() {
        accountDao.saveAccount();
    }
}
```

**存在问题：**

​		事务被自动控制了。换言之，我们使用了 connection 对象的 setAutoCommit(true)

​		此方式控制事务，如果我们每次都执行一条 sql 语句，没有问题，但是如果业务方法一次要执行多条 sql 语句，这种方式就无法实现功能了。

```java
请看下面的示例：
	我们在业务层中多加入一个方法
	
业务层接口
/**
 * 转账
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/5 
 */
void transfer(String sourceName,String targetName,Float money);

业务层实现类
@Override
public void transfer(String sourceName, String targetName, Float money) {
    // 根据名称查询两个账户信息
    Account source = accountDao.findByName(sourceName);
    Account target = accountDao.findByName(targetName);
    // 转出账户减钱，转入账户加钱
    source.setMoney(source.getMoney()-money);
    target.setMoney(target.getMoney()+money);
    // 更新两个账户
    accountDao.update(source);
    int i=1/0;  //模拟转账异常
    accountDao.update(target);
}

	当我们执行时，由于执行有异常，转账失败。但是因为我们是每次执行持久层方法都是独立事务，导致无法实
现事务控制（不符合事务的一致性）

```

### 5.2.2  问题解决

> 解决办法：让业务层来控制事务的提交和回滚。

```java
改造后的业务层实现类：
	注：此处没有使用 spring 的 IoC

/**
 * 转账
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/5 
 */
public class AccountServiceImpl implements IAccountService {
    private IAccountDao accountDao = new AccountDaoImpl();
    
    @Override
    public void saveAccount(Account account) {
        try {
            TransactionManager.beginTransaction();
            accountDao.save(account);
        	TransactionManager.commit();
        } catch (Exception e) {
            TransactionManager.rollback();
            e.printStackTrace();
        }finally {
        	TransactionManager.release();
        }
    }
}

@Override
public void transfer(String sourceName, String targetName, Float money) {
    try {
        TransactionManager.beginTransaction();
        Account source = accountDao.findByName(sourceName);
        Account target = accountDao.findByName(targetName);
        source.setMoney(source.getMoney()-money);
        target.setMoney(target.getMoney()+money);
        accountDao.update(source);
        int i=1/0;
        accountDao.update(target);
        TransactionManager.commit();
    } catch (Exception e) {
        TransactionManager.rollback();
        e.printStackTrace();
    }finally {
        TransactionManager.release();
    }
}
```

TransactionManager 类的代码

```java
/**
 * 转账
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/5 
 */
public class TransactionManager {
    //定义一个 DBAssit
    private static DBAssit dbAssit = new DBAssit(C3P0Utils.getDataSource(),true);
    
    //开启事务
    public static void beginTransaction() {
        try {
        	dbAssit.getCurrentConnection().setAutoCommit(false);
        } catch (SQLException e) {
        	e.printStackTrace();
        }
	}
	
    //提交事务
    public static void commit() {
        try {
        	dbAssit.getCurrentConnection().commit();
        } catch (SQLException e) {
        	e.printStackTrace();
        }
    }
    
    //回滚事务
    public static void rollback() {
        try {
        	dbAssit.getCurrentConnection().rollback();
        } catch (SQLException e) {
        	e.printStackTrace();
        }
    }
    
    //释放资源
    public static void release() {
        try {
        	dbAssit.releaseConnection();
        } catch (Exception e) {
        	e.printStackTrace();
        }
    }
}
```

### 5.2.3  新的问题

通过对业务层改造，已经可以实现事务控制了，但是由于我们添加了事务控制，也产生了一 个新的问题：

​		业务层方法变得臃肿了，里面充斥着很多重复代码。并且业务层方法和事务控制方法耦合了。 

​		试想一下，如果我们此时提交，回滚，释放资源中任何一个方法名变更，都需要修改业务层的代码，况且这还 只是一个业务层实现类，而实际的项目中这种业务层实现类可能有十几个甚至几十个。

​	如何解决？

### 5.2.4  动态代理回顾

#### 5.2.4.1  动态代理的特点

​	字节码随用随创建，随用随加载。 

​	它与静态代理的区别也在于此。因为静态代理是字节码一上来就创建好，并完成加载。 

​	装饰者模式就是静态代理的一种体现。

#### 5.2.4.2  动态代理常用的有两种方式

1. 基于接口的动态代理

   ​	提供者：JDK 官方的 Proxy 类。 

   ​	要求：被代理类最少实现一个接口。

2. 基于子类的动态代理

   ​	提供者：第三方的 CGLib，如果报 asmxxxx 异常，需要导入 asm.jar。 

   ​	要求：被代理类不能用 final 修饰的类（最终类）。

# 第 6 章 spring 中的 AOP

## 6.1  spring 中的 AOP 的细节

> 通过配置的方式实现上一张功能

### 6.1.1  AOP 的相关术语

1. **Joinpoint(连接点)：**

   所谓连接点是指那些被拦截到的点。在 spring 中,这些点指的是方法,因为 spring 只支持方法类型的 连接点。

2. **Pointcut(切入点)：**

   所谓切入点是指我们要对哪些 Joinpoint 进行拦截的定义

3. **Advice(通知/增强):**

   所谓通知是指拦截到 Joinpoint 之后所要做的事情就是通知。 

   通知的类型：前置通知, 后置通知, 异常通知, 最终通知, 环绕通知。

4. **Introduction(引介):**

   引介是一种特殊的通知在不修改类代码的前提下, Introduction 可以在运行期为类动态地添加一些方 法或 Field。

5. **Target(目标对象):**

   代理的目标对象。

6. **Weaving(织入):**

   是指把增强应用到目标对象来创建新的代理对象的过程。 

   spring 采用动态代理织入，而 AspectJ 采用编译期织入和类装载期织入。

7. **Proxy（代理）:**

   一个类被 AOP 织入增强后，就产生一个结果代理类。

8. **Aspect(切面):**

   是切入点和通知（引介）的结合。

   

### 6.1.2  学习 spring 中的 AOP 先知

**开发阶段**（我们做的）

​	编写核心业务代码（开发主线）：大部分程序员来做，要求熟悉业务需求。 

​	把公用代码抽取出来，制作成通知。（开发阶段最后再做）：AOP 编程人员来做。 

​	在配置文件中，声明切入点与通知间的关系，即切面。：AOP 编程人员来做。

**运行阶段**（Spring 框架完成的）

​	Spring 框架监控切入点方法的执行。一旦监控到切入点方法被运行，使用代理机制，动态创建目标对 象的代理对象，根据通知类别，在代理对象的对应位置，将通知对应的功能织入，完成完整的代码逻辑运行。

### 6.1.3  关于代理的选择

> 在 spring 中，框架会根据目标类是否实现了接口来决定采用哪种动态代理的方式。

## 6.2  基于 XML 的 AOP 配置

> 我们在学习 spring 的 aop 时，采用账户转账作为示例。 并且把 spring 的 ioc 也一起应用进来。

### 6.2.1  环境搭建

#### 6.2.1.1 导入依赖

```xml
<!-- 解析切入面表达式-->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.8.7</version>
</dependency>
```

#### 6.2.1.2  创建 spring 的配置文件并导入约束

> 此处要导入 aop 的约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xmlns:aop="http://www.springframework.org/schema/aop"
     xsi:schemaLocation="http://www.springframework.org/schema/beans
     http://www.springframework.org/schema/beans/spring-beans.xsd
     http://www.springframework.org/schema/aop
     http://www.springframework.org/schema/aop/spring-aop.xsd">
    
</beans>

```

#### 6.2.1.3  配置 spring 的 ioc

```xml
<!--配置spring的ioc, 把service对象配置进来-->
<bean id="accountService" class="com.hz.service.impl.AccountServiceImpl"></bean>

<!-- 配置 dao -->
<bean id="accountDao" class="com.hz.dao.impl.AccountDaoImpl">
    <property name="dbAssit" ref="dbAssit"></property>
</bean>

<!-- 配置数据库操作对象 -->
<bean id="dbAssit" class="com.hz.dbassit.DBAssit">
	<property name="dataSource" ref="dataSource"></property>
	<!-- 指定 connection 和线程绑定 -->
	<property name="useCurrentConnection" value="true"></property>
</bean>

<!-- 配置数据源 -->
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
    <property name="jdbcUrl"  value="jdbc:mysql://localhost:3306/eesy?useUnicode=true&amp;characterEncoding=utf8"></property>
    <property name="user" value="root"></property>
    <property name="password" value="root"></property>
</bean>
```

#### 6.2.1.4  抽取公共代码制作成通知

```java
/**
 * 事务控制类
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/5 
 */
public class TransactionManager {
    //定义一个 DBAssit
    private static DBAssit dbAssit = new DBAssit(C3P0Utils.getDataSource(),true);
    
    public void setDbAssit(DBAssit dbAssit) {
        this.dbAssit = dbAssit;
    }

    //开启事务
    public static void beginTransaction() {
        try {
        	dbAssit.getCurrentConnection().setAutoCommit(false);
        } catch (SQLException e) {
        	e.printStackTrace();
        }
	}
	
    //提交事务
    public static void commit() {
        try {
        	dbAssit.getCurrentConnection().commit();
        } catch (SQLException e) {
        	e.printStackTrace();
        }
    }
    
    //回滚事务
    public static void rollback() {
        try {
        	dbAssit.getCurrentConnection().rollback();
        } catch (SQLException e) {
        	e.printStackTrace();
        }
    }
    
    //释放资源
    public static void release() {
        try {
        	dbAssit.releaseConnection();
        } catch (Exception e) {
        	e.printStackTrace();
        }
    }
}
```

### 6.2.2  配置步骤

#### 6.2.2.1  把通知Bean也教给spring来管理

```xml
<!-- 配置通知 -->
<bean id="txManager" class="com.hz.utils.TransactionManager">
	<property name="dbAssit" ref="dbAssit"></property>
</bean> 
```

#### 6.2.2.2  使用aop:config标签表明开始AOP配置

```xml
aop:config:
	作用：用于声明开始 aop 的配置
<aop:config>	
	<!-- 配置的代码都写在此处 -->
</aop:config>
```

#### 6.2.2.3  使用aop:aspect标签标明配置切面

```xml
aop:aspect:
	作用：
		用于配置切面。
	属性：
		id：给切面提供一个唯一标识。
		ref：引用配置好的通知类 bean 的 id。
		
<aop:aspect id="txAdvice" ref="txManager">
	<!--配置通知的类型要写在此处-->
</aop:aspect>
```

#### 6.2.2.4  使用 aop:pointcut 配置切入点表达式

> 切入点表达式：见 6.2.3

```xml
aop:pointcut：
	作用：
		用于配置切入点表达式。就是指定对哪些类的哪些方法进行增强。
	属性：
		expression：用于定义切入点表达式。
		id：用于给切入点表达式提供一个唯一标识

<aop:pointcut  id="pt1"  expression="execution(*com.hz.service.impl.*.*(..))"/>
```

#### 6.2.2.5  在aop:aspect标签内部使用对应的标签类配置通知类型

> 通知的类型：前置通知, 后置通知, 异常通知, 最终通知, 环绕通知

```xml
前置通知 aop:before
	作用：
		用于配置前置通知。指定增强的方法在切入点方法之前执行
	属性：
		method:用于指定通知类中的增强方法名称
		ponitcut-ref：用于指定切入点的表达式的引用
		poinitcut：用于指定切入点表达式
	执行时间点：
		切入点方法执行之前执行

后置通知 aop:after-returning
	作用：
		用于配置后置通知
	属性：
		method：指定通知中方法的名称。
		pointct：定义切入点表达式
		pointcut-ref：指定切入点表达式的引用
	执行时间点：
		切入点方法正常执行之后。它和异常通知只能有一个执行

异常通知 aop:after-throwing
	作用：
		用于配置异常通知
	属性：
		method：指定通知中方法的名称。
		pointct：定义切入点表达式
		pointcut-ref：指定切入点表达式的引用
	执行时间点：
		切入点方法执行产生异常后执行。它和后置通知只能执行一个

最终通知 aop:after
	作用：
		用于配置最终通知
	属性：
		method：指定通知中方法的名称。
		pointct：定义切入点表达式
		pointcut-ref：指定切入点表达式的引用
	执行时间点：
		无论切入点方法执行时是否有异常，它都会在其后面执行。

<!--配置切面-->
<aop:aspect  id="txAdvice"  ref="txManager">
    <!--前置通知:在切入点方法执行之前-->
    <aop:before  method="beginTransaction"  pointcut-ref="pt1"></aop:before>

    <!--后置通知:在切入点方法正常执行之后-->
    <aop:after-returning  method="commit"  pointcut-ref="pt1"></aop:after-returning>

    <!--异常通知:在切入点方法执行产生异常之后-->
    <aop:after-throwing  method="rollback"  pointcut-ref="pt1"></aop:after-throwing>

    <!--最终通知:无论切入点方法是否正常执行，都会在其后执行-->
    <aop:after  method="release"  pointcut-ref="pt1"></aop:after>
</aop:aspect>
```

### 6.2.3  切入点表达式说明

**execution**:匹配方法的执行(常用)

关键字： execution(表达式)

**表达式语法：**

​		execution( 访问修饰符  返回值类型 包名.类名.方法名(参数))

**标准表达式：**
  		public void com.hz.service.impl.AccountServiceImpl.saveAccount()

**返回值可以使用通配符，表示任意通配符**

​	*****  void com.hz.service.impl.AccountServiceImpl.saveAccount()

**包名可以使用通配符，表示任意包。但是有几级包，就需要写几个 `*.` **

*****  void  *****.*****.*****.*****.  AccountServiceImpl.saveAccount()

**包名可以使用 `..` 表示当前包及其子包**

*****  void ***.. ** AccountServiceImpl.saveAccount()

**类名和方法名可以使用`*`来统配**

`* *..*.*()`

**参数列表:**

​	可以直接写数据类型:
​    		基本类型直接写名称： int
​    		引用类型： 包名.类名   java.lang.String
​	可以使用通配符表示任意类型，但必须有参数
​	可以使用..表示有无参数均可，有参数是任意类型

**实际开发中切入点表达式的通常写法：**
    	切到业务层实现类下的所有方法

`* com.hz.service.impl.*.*(..)`

**全通配符:**

`* *..*.*(..)`

### 6.2.4  环绕通知

```xml
aop:around：
	作用：
		用于配置环绕通知
	属性：
		method：指定通知中方法的名称。
		pointct：定义切入点表达式
		pointcut-ref：指定切入点表达式的引用
	说明：
	它是 spring 框架为我们提供的一种可以在代码中手动控制增强代码什么时候执行的方式。
	注意：
		通常情况下，环绕通知都是独立使用的
<aop:config>
    <aop:pointcut expression="execution(* com.hz.service.impl.*.*(..))" id="pt1"/>
    <aop:aspect id="txAdvice" ref="txManager">
    <!-- 配置环绕通知 -->
    <aop:around method="transactionAround" pointcut-ref="pt1"/>
    </aop:aspect>
</aop:config>
```

>  spring 框架为我们提供了一个接口：ProceedingJoinPoint，它可以作为环绕通知的方法参数。
>
> 在环绕通知执行时，spring 框架会为我们提供该接口的实现类对象，我们直接使用就行。

```java
/**
* 环绕通知
* @param pjp
* @return
*/
public Object transactionAround(ProceedingJoinPoint pjp) {
    //定义返回值
    Object rtValue = null;
    try {
        //获取方法执行所需的参数
        Object[] args = pjp.getArgs();
        //前置通知：开启事务
        beginTransaction();
        //执行方法
        rtValue = pjp.proceed(args);
        //后置通知：提交事务
        commit();
    }catch(Throwable e) {
        //异常通知：回滚事务
        rollback();
        e.printStackTrace();
        }finally {
        //最终通知：释放资源
        release();
    }
    return rtValue;
}
```

## 6.3  基于注解的 AOP 配置

### 6.3.1  环境准备

#### 6.3.1.1  导入依赖

```xml
 <dependency>
     <groupId>org.springframework</groupId>
     <artifactId>spring-context</artifactId>
     <version>5.0.2.RELEASE</version>
</dependency>
<!-- 解析切入面表达式-->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.8.7</version>
</dependency>
```

#### 6.3.1.2  添加配置文件

> 导入 context 的名称空间

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:aop="http://www.springframework.org/schema/aop"
    xmlns:context="http://www.springframework.org/schema/context"
     xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="http://www.springframework.org/schema/beans
     http://www.springframework.org/schema/beans/spring-beans.xsd
     http://www.springframework.org/schema/aop
     http://www.springframework.org/schema/aop/spring-aop.xsd
     http://www.springframework.org/schema/context
     http://www.springframework.org/schema/context/spring-context.xsd">
    
    
</beans>
```

#### 6.3.1.3  把资源使用注解配置

```java
/**
 * 账户业务层实现类
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/5 
 */
@Service("accountService")
public class AccountServiceImpl implements IAccountService {
    @Override
    public void saveAccount() {
        System.out.println("save");
    }

    @Override
    public void updateAccount(int i) {
        System.out.println("update --> " + i);
    }
    
    @Override
    public int deleteAccount() {
        System.out.println("delete");
        return 0;
    }
}

/**
 * 账户业务层
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/5 
 */
public interface IAccountService {

    /**
     * 模拟保存账户
     */
    void saveAccount();

    void updateAccount(int i);

    int deleteAccount();
}
```

#### 6.3.1.4  在配置文件中指定 spring 要扫描的包

```xml
<!--配置spring创建容器时要扫描的包-->
<context:component-scan base-package="com.hz"></context:component-scan>
```

#### 6.3.1.5  新建一个类Logger， 模拟通知

```java
/**
 * 日志记录，提供公共代码
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/5 
 */
public class Logger {

    /**
     * 前置通知
     */
    public void beforePrintLog() {
        System.out.println("前置通知：Logger开始beforePrintLog记录日志");
    }

    /**
     * 后置通知
     */
    public void afterReturningPrintLog() {
        System.out.println("后置通知：Logger开始afterReturningPrintLog记录日志");
    }

    /**
     * 异常通知
     */
    public void afterThrowingPrintLog() {
        System.out.println("异常通知：Logger开始beforeThrowingPrintLog记录日志");
    }

    /**
     * 最终通知
     */
    public void afterPrintLog() {
        System.out.println("最终通知：Logger开始beforePrintLog记录日志");
    }
}
```



#### 6.3.2  配置步骤

1. ##### 把通知类也使用注解配置

> @Component

2. ##### 在通知类上使用@Aspect 注解声明为切面

> @Aspect

```java
/**
 * 日志记录，提供公共代码
 * @author hz
 * @blog http://www.huangzun.top
 * @date 2020/3/5 
 */
@Component("logger")
@Aspect  //表示当前类是一个切面类
public class Logger {
	@Pointcut("execution(* com.hz.service.impl.*.*(..))")
    private void pt1() {
    }
}
```

​	3. **在增强的方法上使用注解配置通知**

```java
前置通知 @Before
	作用：
		把当前方法看成是前置通知。
	属性：
		value：用于指定切入点表达式，还可以指定切入点表达式的引用。
	/**
     * 前置通知
     */
    @Before("pt1()")
    public void beforePrintLog() {
        System.out.println("前置通知：Logger开始beforePrintLog记录日志");
    }

后置通知 @AfterReturning
	作用：
		把当前方法看成是后置通知。
	属性：
		value：用于指定切入点表达式，还可以指定切入点表达式的引用
    /**
     * 后置通知
     */
    @AfterReturning("pt1()")
    public void afterReturningPrintLog() {
        System.out.println("后置通知：Logger开始afterReturningPrintLog记录日志");
    }

异常通知 @AfterThrowing
    作用：
		把当前方法看成是异常通知。
	属性：
		value：用于指定切入点表达式，还可以指定切入点表达式的引用
    /**
     * 异常通知
     */
    @AfterThrowing("pt1()")
    public void afterThrowingPrintLog() {
        System.out.println("异常通知：Logger开始beforeThrowingPrintLog记录日志");
    }

最终通知 @After
	作用：
		把当前方法看成是最终通知。
	属性：
		value：用于指定切入点表达式，还可以指定切入点表达式的引用
    /**
     * 最终通知
     */
    @After("pt1()")
    public void afterPrintLog() {
        System.out.println("最终通知：Logger开始beforePrintLog记录日志");
    }
```

4. ##### 在 spring 配置文件中开启 spring 对注解 AOP 的支持

```java
<!--配置spring开启注解AOP的支持-->
<aop:aspectj-autoproxy></aop:aspectj-autoproxy>
```

#### 6.3.3  环绕通知注解配置

```java
环绕通知 @Around
	作用：
		把当前方法看成是环绕通知。
	属性：
		value：用于指定切入点表达式，还可以指定切入点表达式的引用。
    
问题：
	当我们配置了环绕通知，切入点方法没有执行，而通知方法执行了
分析：
    动态代理的环绕通知中有明确的切入点方法调用
解决：
    Spring框架提供了一个接口: ProceedingJoinPoint. 该接口有一个方法: proceed(). 此方法就相当于明确调用切入点方法
    该接口可以作为环绕通知的方法参数，在程序执行时，spring框架会为我们提供该接口的实现类供我们使用
    
	@Around("pt1()")  // 括号不能省略
    public Object aroundPrintLog(ProceedingJoinPoint proceedingJoinPoint) {
        Object result = null;
        try {
            Object[] args = proceedingJoinPoint.getArgs();  // 得到方法执行所需参数
            System.out.println("环绕通知： 开始前置通知");
            result = proceedingJoinPoint.proceed(args);  // 明确调用业务层方法(切入点方法)
            System.out.println("环绕通知： 开始后置通知");
            return result;
        } catch (Throwable throwable) {
            System.out.println("环绕通知： 开始异常通知");
            throwable.printStackTrace();
        } finally {
            System.out.println("环绕通知： 开始最终通知");
        }
        return result;
    }
```

#### 6.3.4  切入点表达式注解配置

```java
@Pointcut
	作用：
		指定切入点表达式
	属性：
		value：指定表达式的内容
		
 @Pointcut("execution(* com.hz.service.impl.*.*(..))")
 private void pt1() {
 
 }
```

#### 6.3.5  不使用 XML 的配置方式

```java
@Configuration
@ComponentScan(basePackages="com.hz")
@EnableAspectJAutoProxy
public class SpringConfiguration {
    
}
```

