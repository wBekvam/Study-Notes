# Lombok

### 一、简介

Lombok是一个可以通过简单的注解的形式来帮助我们简化消除一些必须有但显得很臃肿的 Java 代码的工具，简单来说，比如我们新建了一个类，然后在其中写了几个字段，然后通常情况下我们需要手动去建立`getter`和`setter`方法啊，构造函数啊之类的，Lombok的作用就是为了省去我们手动创建这些代码的麻烦，它能够在我们编译源码的时候自动帮我们生成这些方法。Lombok能够达到的效果就是在源码中不需要写一些通用的方法，但是在编译生成的字节码文件中会帮我们生成这些方法。

**传送门**

- [Lombok的官方地址](https://projectlombok.org)

- [Lombok的Github地址](https://github.com/rzwitserloot/lombok)

### 二、Lombok安装

- ##### Maven安装

  ```xml
  <dependencies>
      <dependency>
          <groupId>org.projectlombok</groupId>
          <artifactId>lombok</artifactId>
          <version>1.18.12</version>
          <scope>provided</scope>
      </dependency>
  </dependencies>
  ```

- ##### Gradle安装

  ```java
  dependencies {
      compileOnly 'org.projectlombok:lombok:1.18.12'
      annotationProcessor 'org.projectlombok:lombok:1.18.12'
  
      testCompileOnly 'org.projectlombok:lombok:1.18.12'
      testAnnotationProcessor 'org.projectlombok:lombok:1.18.12'
  }
  ```

- ##### IntelliJ IEDA

  Go to File  ->  Setting  ->  Plugins

  ![image-20200530095814160](C:\Users\HZ\AppData\Roaming\Typora\typora-user-images\image-20200530095814160.png)

### 三、使用

使用：在类或方法上添加需要的注解即可。

#### **Lombok注解**

- val 声明final局部变量
- var 声明局部变量
- @NonNull注解在方法参数上，用于自动生成空值参数检查，自动帮助我们避免空指针
- @Cleanup自动帮我们调用close()方法，作用在局部变量上，在作用域结束时会自动调用close()方法释放资源，可以关闭流
- @Getter注解在属性（类）上，为属性（所有非静态成员变量）提供get()方法
- @Setter注解在属性（类）上，为属性（所有非静态成员变量）提供set()方法
- @ToString 该注解的作用是为类自动生成toString()方法
- @EqualsAndHashCode为对象字段自动生成hashCode()和equals()实现
- @AllArgsConstructor，@RequiredArgsConstructor，@NoArgsConstructor顾名思义，为类自动生成对应参数的构造器
- @Data注解在类上，自动为所有字段添加@ToString，@EqualsAndHashCode，@Getter为非final字段添加@Setter和@RequiredArgsConstructor本质上相当于几个注解的综合效果
- @Value注解和@Data类似，区别在于它会把所有成员变量默认定义为private final修饰，并且不会生成set()方法
- @Builder注解在类上，被注解的类加个构造者模式
- @SneakyThrows等同于try/catch捕获异常
- Synchronized 注解在类上，加个同步锁
- @With Immutable 'setters' - methods that create a clone but with one changed field.
- @Getter(lazy=true) Laziness is a virtue!
- @Log，@Log4j，@Log4j2，@Slf4j，@XSlf4j，@CommonsLog，@JBossLog注解在类上，自动为类添加对应的日志支持

**传送门**

[具体方法]( https://projectlombok.org/features/all)

### 四、Lombok的坏处

**强制队友使用**

因为Lombok的使用要求开发者一定要在IDE中安装对应的插件。 如果未安装插件的话，使用IDE打开一个基于Lombok的项目的话会提示找不到方法等错误。导致项目编译失败。 也就是说，如果项目组中有一个人使用了Lombok，那么其他人就必须也要安装IDE插件。否则就没办法协同开发。

**降低代码可读性，可调试性**

Lombok会帮忙自动生成很多代码, 但是这些代码是要在编译阶段才会生成的，所以在开发的过程中，其实很多代码其实是缺失的。 导致代码的可读性会低很多，而且也会给代码调试带来一定的问题。

**有坑，破坏封装**

​	**不要滥用 @Data**

在使用Lombok过程中，如果对于各种注解的底层原理不理解的话，很容易产生意想不到的结果。

1. 举一个简单的例子，当我们使用@Data定义一个类的时候，会自动帮我们生成equals()方法 。 但是如果只使用了@Data，而不使用@EqualsAndHashCode(callSuper=true)的话，会默认是@EqualsAndHashCode(callSuper=false),这时候生成的equals()方法只会比较子类的属性，不会考虑从父类继承的属性，无论父类属性访问权限是否开放。
2. 某些属性就应该是private，并且不可被客户代码访问的，而 @Data 会生成所有属性的 getter/setter 破坏封装

### 总结

Lombok的优点是使用注解即可帮忙自动生成代码，大大减少了代码量，使代码非常简洁。 但在使用Lombok的过程中，还可能存在对队友不友好、对代码不友好、对调试不友好、对升级不友好等问题。

**所以，**是否使用Lombok应综合考虑。