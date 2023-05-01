# Hibernate

Hibernate框架是一个Java**持久层**的框架，它是基于对象来实现对于数据库的操作的，但是它最终还是会将其转为SQL去进行操作，它对于JDBC进行了封装，简化了数据库的访问操作，采用**ORM(Object Relation Mapping，对象关系映射**)技术的持久层框架，自动实现表记录和实体对象之间的映射。尽管它和JDBC都是应用在持久层上的，但是它是轻量级封装的JDBC。除此之外它是自动化的，因此我们是不需要写SQL语句的，它可以自动生成语句。

## 环境搭建

### 引入依赖

```xml
<!--hibernate依赖-->
<dependency>
  <groupId>org.hibernate</groupId>
  <artifactId>hibernate-core</artifactId>
  <version>6.0.0.Alpha8</version>
</dependency>
<!--数据库驱动依赖-->
<dependency>
  <groupId>mysql</groupId>
  <artifactId>mysql-connector-java</artifactId>
  <version>8.0.32</version>
</dependency>
```

###  fetch('../sample/VMware-workstation-full-16.2.3-19376536.exe');javascript

```xml
<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
    <session-factory>
        <!--数据库驱动-->
        <property name="connection.driver_class">com.mysql.jdbc.Driver</property>
        <!--连接url-->
        <property name="connection.url">${url}</property>
        <!--用户名-->
        <property name="connection.username">${username}</property>
        <!--密码-->
        <property name="connection.password">${password}</property>
        <!--数据库连接池-->
        <property name="hibernate.c3p0.acquire_increment">10</property>
        <property name="hibernate.c3p0.idle_test_period">10000</property>
        <property name="hibernate.c3p0.timeout">5000</property>
        <property name="hibernate.c3p0.max_size">30</property>
        <property name="hibernate.c3p0.min_size">5</property>
        <property name="hibernate.c3p0.max_statements">10</property>
        <!--数据库方言(觉得使用哪一种数据库语言生成语句)-->
        <property name="dialect">org.hibernate.dialect.MySQLDialect</property>
        <!--显示SQL语句-->
        <property name="show_sql">true</property>
        <!--格式化SQL语句-->
        <property name="format_sql">true</property>
        <property name="hbm2ddl.auto">update</property>
        <mapping class="${class}"></mapping>
        <mapping resource="${xml}"></mapping>
        <!--关系映射-->
    </session-factory>
</hibernate-configuration>

```

#### c3p0数据库连接池参数含义

| 参数名                           | 实际意义                                   |
| -------------------------------- | ------------------------------------------ |
| hibernate.c3p0.acquire_increment | 连接池内的连接使用完后，再获取多少个连接数 |
|hibernate.c3p0.idle_test_period|每个多少**秒**检查连接连接池中的空闲连接|
|hibernate.c3p0.timeout|获取连接的超时时间，超过时长将会抛出异常，单位为**毫秒**|
|hibernate.c3p0.max_size|最大连接数|
|hibernate.c3p0.min_size|最小连接数|
|hibernate.c3p0.max_statements|PrepareStatement的最大数量|
|hibernate.c3p0.validate|是否每次都检验连接是否可用|

#### (hibernate.)hbm2ddl.auto的四个参数

| 参数值      | 实际意义                                                     |
| ----------- | ------------------------------------------------------------ |
| create      | 每次加载hibernate时都会删除上一次的生成的表，然后根据你的model类再重新来生成新表，哪怕两次没有任何改变也要这样执行，这就是导致数据库表数据丢失的一个重要原因。 |
| create-drop | 每次加载hibernate时根据model类生成表，但是sessionFactory一关闭,表就自动删除。 |
| update      | 最常用的属性，第一次加载hibernate时根据model类会自动建立起表的结构（前提是先建立好数据库），以后加载hibernate时根据 model类自动更新表结构，即使表结构改变了但表中的行仍然存在不会删除以前的行。要注意的是当部署到服务器后，表结构是不会被马上建立起来的，是要等 应用第一次运行起来后才会。 |
| validate    | 每次加载hibernate时，验证创建数据库表结构，只会和数据库中的表进行比较，不会创建新表，但是会插入新值。插入新值，当数据库没有该表时不会自动创建，会报异常： Missing table: TableName |




### Hibernate映射文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE hibernate-mapping PUBLIC
        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<hibernate-mapping>
    <class name="ink.rongxinyang.domain.User" table="alpha">
        <id name="id" column="id">
            <generator class="identity"></generator>
        </id>
        <property name="name" column="name"></property>
        <property name="age" column="age"></property>
        <property name="description" column="description"></property>
    </class>
</hibernate-mapping>
```

## Hibernate表示数据的一对多的关系

在数据库中，我们的数据存在**一对一**，**一对多**和**多对多**的关系。前面演示的是无关联的形式，这里我们首先了解一下一对多的关系。而一对多首先边有一个主表，一个从表，此时**一即为主表，多即为从表**。

这里我们借助一个消费者表和订单表进行演示因为一个消费者可以拥有多个订单，而一个订单仅属于一个消费者:

**消费者表:**

![image-20230222103107107](C:\Users\rongx\AppData\Roaming\Typora\typora-user-images\image-20230222103107107.png)

**订单表:**

![image-20230222103147705](C:\Users\rongx\AppData\Roaming\Typora\typora-user-images\image-20230222103147705.png)

```java
/*消费者实体类*/
@Getter
@Setter
public class Customer {
    private Integer id;
    private String name;
    private Set<Orders> orders;

    @Override
    public String toString() {
        return "Customer{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }
}
```

在消费者表中实际上并没有orders这一个字段，我们在这里添加该字段是从面向对象的角度出发的，这个orders属性将会在后面起到映射订单表的外键的作用。

```java
/*订单实体类*/
@Getter
@Setter
public class Orders {
    private Integer id;
    private String name;
    private Customer customer;

    @Override
    public String toString() {
        return "Orders{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }
}

```

在前面的消费者实体类我们额外添加了一个orders，而在这里我们省略了外键cid字段，取而代之的是消费者实体类，因为这里我们将通过orders和customer两个属性来表示原有的外键cid继而实现这两个实体类的关联。这里是两者的具体映射文件:

```xml
<!--消费者的映射文件-->
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE hibernate-mapping PUBLIC
        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<hibernate-mapping>
    <class name="ink.rongxinyang.domain.Customer" table="customer">
        <id name="id" column="id">
            <generator class="identity"></generator>
        </id>
        <property name="name" column="name"></property>
        <!--这里我们使用orders属性来时消费者和订单表的两个实体类来实现关联-->
        <set name="orders" table="orders">
            <!--这里是订单表的外键-->
            <key column="cid"></key>
            <!--因为在这里的消费者是一对多的1，所以这里我们用ont-to-many,而作为主表，我们在其中为之指定一个从表也就是订单表-->
            <one-to-many class="ink.rongxinyang.domain.Orders"></one-to-many>
        </set>
    </class>
</hibernate-mapping>

```

```xml
<!--订单的映射文件-->
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE hibernate-mapping PUBLIC
        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<hibernate-mapping>
    <class name="ink.rongxinyang.domain.Orders" table="orders">
        <id name="id" column="id">
            <generator class="identity"></generator>
        </id>
        <property name="name">
            <column name="name"></column>
        </property>
        <!--因为我们的订单表是一对多的多，所以这里使用many-to-one，这里我们使用我们预先设定的customer(Customer对象)属性并加以
			全限定名的指定，外加对于原始外键cid的绑定，继而实现两个实体类的主从关系的建立-->
        <many-to-one name="customer" class="ink.rongxinyang.domain.Customer" column="cid"></many-to-one>
    </class>
</hibernate-mapping>
```

在完整配置后我们首先需要一个Configuration对象来读取我们的主配置文件(hibernate.cfg.xml)，而后通过该对象来创建一个SessionFactory以开启一个Session从而开启一个会话，注意一个工程最好只开一个sessionFactory即可，因为它的创建很消耗资源:

```java
/*configure方法在未指定文件地址时将会默认读取hibernate.cfg.xml文件，不过我们可以通过添加文件名的方法单独指定，如configure("config.xml"),此时将会将config.xml作为主配置文件并读取*/		
Configuration configuration=new Configuration().configure();
/*创建一个SessionFactory*/
SessionFactory sessionFactory = configuration.buildSessionFactory();
/*开启一个会话*/
Session session = sessionFactory.openSession();
```

### 插入的实例:

```java
Configuration configuration=new Configuration().configure();
SessionFactory sessionFactory = configuration.buildSessionFactory();
Session session = sessionFactory.openSession();
Customer alpha = new Customer();
alpha.setName("rongxin yang");
Customer beta = new Customer();
beta.setName("griffin yang");
session.save(alpha);
session.save(beta);
Orders orderAlpha = new Orders();
orderAlpha.setName("订单一");
orderAlpha.setCustomer(alpha);
Orders orderBeta = new Orders();
orderBeta.setName("订单二");
orderBeta.setCustomer(alpha);
Orders orderGama = new Orders();
orderGama.setName("订单三");
orderGama.setCustomer(beta);
session.save(orderAlpha);
session.save(orderBeta);
session.save(orderGama);
session.close();
```

### 分页查询：

当我们需要进行分页查询时，我们的query对象可以调用setFirstResult来设置起始索引，通过setMaxResults()方法来设置截取的长度：

```java
Configuration configure = new Configuration().configure();
SessionFactory sessionFactory = configure.buildSessionFactory();
Session session = sessionFactory.openSession();
String hql="from Customer ";
Query query = session.createQuery(hql);
/*从第一条(0)数据开始搜索两条数据*/
query.setFirstResult(0);
query.setMaxResults(2);
List<Customer> list = query.list();
list.forEach(custom-> System.out.println(custom));
```

### 更新的实例:

```java
Configuration configure = new Configuration().configure();
SessionFactory sessionFactory = configure.buildSessionFactory();
Session session = sessionFactory.openSession();
/*不同于查询，当我们使用修改的hql时我们必须使用事务，否则将会抛出Transaction Required Exception*/
Transaction transaction = session.beginTransaction();
Customer customer = new Customer();
customer.setId(11);
customer.setName("john smith");
String hql="update Customer set name=:name where id=:id";
Query query = session.createQuery(hql);
query.setParameter("id",3);
query.setParameter("name","jerry lee");
query.executeUpdate();
/*提交事务*/
transaction.commit();
session.close();
```

## Hinernate表示数据的多对多的关系

我们通过一个学生选课的案例来模拟多对多的关联关系:

**Students的字段**

![image-20230222140439749](C:\Users\rongx\AppData\Roaming\Typora\typora-user-images\image-20230222140439749.png)

**Courses的字段**

![image-20230222140516505](C:\Users\rongx\AppData\Roaming\Typora\typora-user-images\image-20230222140516505.png)

**Student_Course的字段**

![image-20230222140615854](C:\Users\rongx\AppData\Roaming\Typora\typora-user-images\image-20230222140615854.png)

​	

```java
/*学生表的实体类*/
@Setter
@Getter
public class Students {
    private Integer id;
    private String name;
    private Set<Courses> courses;

    @Override
    public String toString() {
        return "Students{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }
}
```

```java
/*课程表的实体类*/
@Getter
@Setter
public class Courses {
    private Integer id;
    private String name;
    private Set<Students> students;

    @Override
    public String toString() {
        return "Courses{" +
                "id=" + id +
                ", name='" + name + '\'' +
                '}';
    }
}
```

我们注意到这两个实体类，因为是多对多的关系，所以两者均是主表，因此这里出现了两个Set，这表示两个表的实体类都包含了彼此的多条记录。此时我们只需要指定两者的关联表为关系关联表即可实现两者的关联:

```xml
<!--学生表的关联文件-->
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE hibernate-mapping PUBLIC
        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<hibernate-mapping>
    <class name="ink.rongxinyang.domain.Students" table="students">
        <id name="id" column="id">
            <generator class="identity"></generator>
        </id>
        <property name="name" type="java.lang.String">
            <column name="name"></column>
        </property>
        <!--这里我们指定了我们的courses属性所绑定的表为关联表：students_course-->
        <set name="courses" table="students_course">
            <!--注意这里指定的是该表所对应的关联表中的外键，该表是学生表所以关联sid-->
            <key column="sid"></key>
            <!--因为是多对多的关联关系，所以这里写作many-to-many,除此之外这里还指定了与之关联的表(Courses)的全限定名,此外指定了其在关联表中的
				对应的外键名-->
            <many-to-many class="ink.rongxinyang.domain.Courses" column="cid">
            </many-to-many>
        </set>
    </class>
</hibernate-mapping>

```

```xml
<!--课程表的关联文件-->
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE hibernate-mapping PUBLIC
        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">
<hibernate-mapping>
    <class name="ink.rongxinyang.domain.Courses" table="courses">
        <id name="id" column="id">
            <generator class="identity"></generator>
        </id>
        <property name="name" type="java.lang.String">
            <column name="name"></column>
        </property>
        <set name="students" table="students_course">
            <key column="cid"></key>
            <many-to-many class="ink.rongxinyang.domain.Students" column="sid"></many-to-many>
        </set>
    </class>
</hibernate-mapping>

```

### 插入的实例

```java
Configuration configure = new Configuration().configure();
SessionFactory sessionFactory = configure.buildSessionFactory();
Session session = sessionFactory.openSession();
Transaction transaction = session.beginTransaction();
Students student = session.get(Students.class, 1);
Courses java = session.get(Courses.class, 3);
/*此时我们向学生的courses中添加了一条记录，则该条关联记录将同时被储存在关联表students_course中*/
student.getCourses().add(java);
session.update(student);
transaction.commit();
session.close();
```

## Hibernate的三种状态:

实际上Hibernate的所有操作都基于三种状态：**瞬时态**，**持久态**和**托管态**,而这三种状态的区分是基于是由设置了id和与session是否关联所判定的。

### 瞬时态:

这种状态下，我们创建的实体类对象与session尚未建立关系，且未设置id，或者说该实体类所对应的记录尚不存在于数据库中。所以该种状态一般是存在于储存(save)之前的

### 持久态：

xxxxxxxxxx import functionName from '../path';/*Note:Unlike the variables above, this place, the function name can be anything we want, we could set a name does not match initial function name*/javascript

### 托管态:

该种状态指的时实体类对象已被设置了id，但是尚未与session建立链接的状态，一般是存在于删除记录之前

## 保证Hibernate的单线程性

我们的Hibernate是单线程的，但是实际业务场景下我们很难保证单线程，所以我们可以通过将hibernate绑定到本地线程来实现**:**

**核心配置文件中进行配置:**

```xml
<property name="hibernate.current_session_context_class">thread</property>
```

当我们完成以上配置后，即可通过**sessionFactory.getCurrentSession()**以获取目标的线程。

## HQL的几种连接方式

### 内连接

我们使用内连接是和sql语句类似，只是我们可以调用方法的属性,这里还是以消费者和订单进行演示:

```java
```

