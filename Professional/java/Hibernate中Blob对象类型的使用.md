# Hibernate中Blob对象类型的使用

> 编辑器 Intellij IDEA 2019 3.3

先创建Hibernate项目，导入相应的安装包

工具包：[下载](https://sourceforge.net/projects/hibernate/
)

## 1. hibernate.cfg.xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">

<hibernate-configuration>
   <session-factory>
      <!-- 1. 数据库链接 -->
      <property name="hibernate.connection.driver_class">org.h2.Driver</property>
      <property name="hibernate.connection.url">jdbc:h2:~/h2/h2demo;AUTO_SERVER=TRUE</property>
      <property name="hibernate.connection.username">sa</property>
      <property name="hibernate.connection.password"></property>
      <property name="hibernate.dialect">org.hibernate.dialect.H2Dialect</property>

      <!-- 2. 其他相关配置 -->
        <!-- 2.1 显示hibernate在运行时候执行的sql语句 -->
        <property name="hibernate.show_sql">false</property>
        <!-- 2.2 格式化sql -->
        <property name="hibernate.format_sql">false</property>
        <!-- 2.3 自动建表  -->
        <!-- <property name="hibernate.hbm2ddl.auto">create</property>  -->

        <!--3. 加载所有映射-->
        <mapping resource="db/hibernate/College.hbm.xml"/>
      <mapping resource="db/hibernate/Student.hbm.xml"/>
   </session-factory>
</hibernate-configuration>
```

### 1.1 数据库 dialect

| **数据库**                 | **方言属性**                               |
| -------------------------- | ------------------------------------------ |
| DB2                        | org.hibernate.dialect.DB2Dialect           |
| HSQLDB                     | org.hibernate.dialect.HSQLDialect          |
| HypersonicSQL              | org.hibernate.dialect.HSQLDialect          |
| Informix                   | org.hibernate.dialect.InformixDialect      |
| Ingres                     | org.hibernate.dialect.IngresDialect        |
| Interbase                  | org.hibernate.dialect.InterbaseDialect     |
| Microsoft  SQL Server 2000 | org.hibernate.dialect.SQLServerDialect     |
| Microsoft  SQL Server 2005 | org.hibernate.dialect.SQLServer2005Dialect |
| Microsoft  SQL Server 2008 | org.hibernate.dialect.SQLServer2008Dialect |
| MySQL                      | org.hibernate.dialect.MySQLDialect         |
| Oracle  (any version)      | org.hibernate.dialect.OracleDialect        |
| Oracle  11g                | org.hibernate.dialect.Oracle10gDialect     |
| Oracle  10g                | org.hibernate.dialect.Oracle10gDialect     |
| Oracle  9i                 | org.hibernate.dialect.Oracle9iDialect      |
| PostgreSQL                 | org.hibernate.dialect.PostgreSQLDialect    |
| Progress                   | org.hibernate.dialect.ProgressDialect      |
| SAP  DB                    | org.hibernate.dialect.SAPDBDialect         |
| Sybase                     | org.hibernate.dialect.SybaseDialect        |
| Sybase  Anywhere           | org.hibernate.dialect.SybaseAnywhereDialec |

### 1.2 参数配置

- #### hibernate.dialect

  - 使 Hibernate 应用为被选择的数据库生成适当的 SQL
  - ◦org.hibernate.dialect.SQLServerDialect

- #### hibernate.connection.driver_class

  - JDBC 驱动程序类

- #### hibernate.connection.url

  - 数据库实例的 JDBC URL

- #### hibernate.connection.username

  - 数据库用户名

- #### hibernate.connection.password

  - 数据库密码

- #### hibernate.connection.pool_size

  - 限制在 Hibernate 应用数据库连接池中连接的数量

- #### hibernate.connection.autocommit

  - 允许在 JDBC 连接中使用自动提交模式

## 2. 实体类 Student.java

> photo 为 Bolo 类型

```java
public class Student {
   private int id;
   private String no;
   private String name;
   private int age;
   private Date birthday;
   private Blob photo;

   public Student() {}

   public Student(int id, String no, String name, int age, Date birthday) {
      this.id = id;
      this.no = no;
      this.name = name;
      this.age = age;
      this.birthday = birthday;
   }
   public int getId() {
      return id;
   }

   public void setId(int id) {
      this.id = id;
   }

   public String getNo() {
      return no;
   }

   public void setNo(String no) {
      this.no = no;
   }

   public String getName() {
      return name;
   }

   public void setName(String name) {
      this.name = name;
   }

   public int getAge() {
      return age;
   }

   public void setAge(int age) {
      this.age = age;
   }

   public Date getBirthday() {
      return birthday;
   }

   public void setBirthday(Date birthday) {
      this.birthday = birthday;
   }

   public Blob getPhoto() {
      return photo;
   }

   public void setPhoto(Blob photo) {
      this.photo = photo;
   }

   @Override
   public String toString() {
      return "Student{" +
            "id=" + id +
            ", no='" + no + '\'' +
            ", name='" + name + '\'' +
            ", age=" + age +
            ", birthday=" + birthday +
            ", photo=" + photo +
            '}';
   }
}
```

## 3. 配置 Student.hbm.xml

> 用Hibernate映射到数据库中所使用的类型

```xml
<?xml version="1.0"?>
<!DOCTYPE hibernate-mapping PUBLIC "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://hibernate.sourceforge.net/hibernate-mapping-3.0.dtd">
<!-- Generated 2019-3-31 23:15:19 by Hibernate Tools 3.5.0.Final -->
<hibernate-mapping>
    <class name="db.jdbc.Student" table="STUDENT">
        <id name="Id" type="java.lang.Integer">
            <column name="ID" />
            <generator class="assigned" />
        </id>
        <!-- 添加属性和表字段的映射 -->
        <property name="no" type="java.lang.String">
            <column name="NO" />
        </property>
        <property name="name" type="java.lang.String">
            <column name="NAME"/>
        </property>
        <property name="age" type="java.lang.Integer">
            <column name="AGE"/>
        </property>
        <!--使用 java.util.Date 类型，在数据库中字段的类型为 datetime（会出现时分秒）-->
        <!--<property name="birthday" type="java.util.Date">-->
        <property name="birthday" type="java.sql.Date">
            <column name="BIRTHDAY"/>
        </property>
        
        <property name="photo" type="java.sql.Blob" column="PHOTO"></property>
    </class>
</hibernate-mapping>
```

## 4. 添加测试类

> 两个测试方法：insertPhoto() 插入一条记录；readPhoto() 读一条记录并保存

```java
/**
 * @author hz
 * @date 2020/4/23
 */

public class TestStudent {
    private SessionFactory factory;
    private Session session;

    @Before
    public void init() {
        //创建服务注册对象
        StandardServiceRegistry standardRegistry = new StandardServiceRegistryBuilder().configure().build();
        Metadata metadata = new MetadataSources(standardRegistry).getMetadataBuilder().build();
        factory = metadata.buildSessionFactory();
        //会话对象
        session = factory.openSession();
        //开启事务
        session.beginTransaction();
    }

    @After
    public void destory() {
        session.getTransaction().commit(); //提交事务
        session.close(); //关闭会话
        factory.close(); //关闭会话工厂
    }

    
    @Test
    public void insertPhoto() throws IOException {
        // 生日
        long b = System.currentTimeMillis();
        Date birthday = new Date(b - b % oneDay);
        // 获取图片
        String imagePath = System.getProperty("user.dir") + "\\t3.jpg";
        File file = new File(imagePath);
        InputStream is = new FileInputStream(file);
        Student student = new Student(888, "20170104", "王五", 20, birthday);
        // 创建一个Blob对象
        Blob image = Hibernate.getLobCreator(session).createBlob(is, is.available());
        // 设置照片属性
        student.setPhoto(image);
        session.save(student);
    }
    
    @Test
    public void readPhoto() throws SQLException, IOException {
        // 主键为 888 的记录
        Student student = session.get(Student.class, 888);
        // 获取blob 对象
        Blob blob = student.getPhoto();
        // 获取照片输入流
        InputStream is = blob.getBinaryStream();
        // 创建输出流
        String imagePath = System.getProperty("user.dir") + "\\t4.jpg";
        File file = new File(imagePath);
        OutputStream os = new FileOutputStream(file);
        // 创建缓冲区
        byte [] buffer = new byte[is.available()];
        is.read(buffer);
        os.write(buffer);
        is.close();
        os.close();
    }

}
```

查看数据，数据插入成功：

![image-20200423134507948](C:\Users\HZ\AppData\Roaming\Typora\typora-user-images\image-20200423134507948.png)

