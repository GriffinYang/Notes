# Java框架整合

## Spring和Spring MVC的区别与功能

在传统的Java代码中我们如果需要创建一个对象，那么我们需要手动的来创建对象，也就是使用**new**关键字来实现，这也就意味着我们的工程量会很大，而且生成和销毁对象的管理工作将使其变得更加庞杂，为此我们引入了Spring框架，实际上Spring是一个容器，这个容器是一个对象的容器，它将会根据我们在配置文件中提供的信息来使用Java的反射机制来实现对于对象的动态创建和销毁以实现对于对象的管理，这也就意味着，使用Spring容器将会大大降低我们的代码复杂度，以简化开发。

而SpringMVC全称为（**Spring Model View Controller**），其含义即为其内部的三层结构。首先我们希望实现的功能从客户端向服务端发送请求，而后后端根据请求来返回响应。那么这一步就需要由Controller来实现，而Controller实际上就是一个控制器。而Model和View则时一起的，我们向前端返回的页面即为View，而返回的View则是根据我们的Model(**计算后的数据**)来生成的。

## SSM(Spring SpringMVC Mybatis)框架的整合

SSM框架使用Spring作为容器，SpringMVC作为前端调度器，Mybatis作为数据库操作的中介以实现开发。其中Mybatis将数据库操作单独提取出来以降低了耦合度，以简化开发，除此之外，我们可以借助MybatisPlus来实现进一步的简化，因为其将几乎单表操作全部进行了封装，我们只需调用方法即可。这里我们着重说明SSM框架的搭建思路和搭建的过程:

### 搭建思路

* 创建一个webapp项目

* 引入spring,springmvc,数据库，mybatis(mybatisplus),json依赖

* 创建mybatis配置文件

* 创建spring容器(配置文件)

  * 启用注解

  * 规定servlet的处理对象

  * 设置url配置(url处理策略，前端控制器，视图前缀后缀配置)

  * 指定扫描包名

  * 配置数据源

  * 配置事务

    * 开启事务注解

    * 定义事务管理器

    * 为事务注解设置事务管理器

  * 配置mybatis

    * 配置mapper的目录
    * 定义sessionFactory

* 将spring配置文件在web.xml中声明

### 基本的目录结构

!["目录结构"](.\目录结构.png)

### 配置文件

#### 依赖文件

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>ink.rongxinyang</groupId>
  <artifactId>SSMProject</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>SSMProject Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
      <!--springmvc依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>5.3.3</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.3.3</version>
    </dependency>
    <!--数据库依赖-->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.32</version>
    </dependency>
      <!--spring的依赖-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-orm</artifactId>
      <version>5.3.3</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aop</artifactId>
      <version>5.3.3</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>5.3.3</version>
    </dependency>
     <!--数据库连接池-->
    <dependency>
      <groupId>com.mchange</groupId>
      <artifactId>c3p0</artifactId>
      <version>0.9.5.5</version>
    </dependency>
      <!--lombok依赖-->
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <version>1.18.26</version>
    </dependency>
    <!--使用mybatis使用的依赖-->
    <!--注意mybatisplus依赖一个mybatis不可同时存在，否则以产生冲突-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>2.0.6</version>
    </dependency>
    <dependency>
      <groupId>com.baomidou</groupId>
      <artifactId>mybatis-plus-extension</artifactId>
      <version>3.5.3.1</version>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>3.5.13</version>
    </dependency>
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>3.0.1</version>
    </dependency>
    <!--使用mybatisplus使用的依赖-->
    <!--注意mybatisplus依赖一个mybatis不可同时存在，否则以产生冲突-->
    <dependency>
      <groupId>com.github.pagehelper</groupId>
      <artifactId>pagehelper</artifactId>
      <version>5.2.1</version>
    </dependency>
    <dependency>
      <groupId>com.baomidou</groupId>
      <artifactId>mybatis-plus</artifactId>
      <version>3.5.2</version>
    </dependency>
     <!--配置json支持-->
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-core</artifactId>
      <version>2.13.3</version>
    </dependency>
    <dependency>
      <groupId>com.fasterxml.jackson.core</groupId>
      <artifactId>jackson-databind</artifactId>
      <version>2.14.2</version>
    </dependency>
      <!--log4j的日志依赖-->
    <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>1.2.17</version>
    </dependency>
  </dependencies>
  <build>
    <finalName>SSMProject</finalName>
    <plugins>
        <!--maven的编译插件-->
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
        </configuration>
      </plugin>
    </plugins>

  </build>
</project>

```



#### mybatis配置文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- mybatis运行时设置 -->
    <settings>
        <!-- 启用log4j日志 -->
        <setting name="logImpl" value="LOG4J"></setting>
        <!--开启驼峰命名-->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>

    <typeAliases>
         <!--为实体类包下的实体类添加别名-->
        <package name="ink.rongxinyang.ssm.domain" />
    </typeAliases>

    <!-- mybatis分页插件 -->
    <plugins>
        <plugin interceptor="com.github.pagehelper.PageInterceptor">
            <!-- 配置mysql方言 -->
            <property name="helperDialect" value="mysql" />
            <!-- 设置为true时，如果pageSize=0就会查询出全部的结果 -->
            <property name="pageSizeZero" value="true" />
            <!-- 3.3.0版本可用，分页参数合理化，默认false禁用 -->
            <!-- 启用合理化时，如果pageNum<1会查询第一页，如果pageNum>pages会查询最后一页 -->
            <!-- 禁用合理化时，如果pageNum<1或pageNum>pages会返回空数据 -->
            <property name="reasonable" value="true" />
        </plugin>
    </plugins>
</configuration>
```

#### spring配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx" xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/tx
	http://www.springframework.org/schema/tx/spring-tx.xsd
	http://www.springframework.org/schema/mvc
	https://www.springframework.org/schema/mvc/spring-mvc.xsd">
    <!--开启注解-->
    <mvc:annotation-driven></mvc:annotation-driven>
    <!--设置servlet处理类为默认值-->
    <mvc:default-servlet-handler></mvc:default-servlet-handler>
    <!--使用BeanNameUrlHandlerMapping作为请求映射的处理类-->
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
    <!--配置调度器-->
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
    <!--配置视图解析器-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/"/>
        <property name="suffix" value=".html"/>
    </bean>
    <!--扫描项目的根目录(包)-->
    <context:component-scan base-package="ink.rongxinyang.ssm"></context:component-scan>
    <!--开启事务注解-->
    <tx:annotation-driven></tx:annotation-driven>
    <!--引入数据库链接的配置文件-->
    <context:property-placeholder location="classpath:mysql.properties">		     </context:property-placeholder>
    <!--配置数据源-->
    <bean id="dataSource" class="com.mchange.v2.c3p0.DriverManagerDataSource">
        <property name="driverClass">
            <value>${jdbc.driverName}</value>
        </property>
        <property name="jdbcUrl">
            <value>${jdbc.url}</value>
        </property>
        <property name="user">
            <value>${jdbc.username}</value>
        </property>
        <property name="password">
            <value>${jdbc.password}</value>
        </property>
    </bean>
    <!--为事务注解添加事务管理器-->
    <tx:annotation-driven transaction-manager="transactionManager"></tx:annotation-driven>
    <!--声明事务管理器-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"></property>
    </bean>

    <bean id="mapperScanner" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <property name="basePackage" value="ink.rongxinyang.ssm.mapper"></property>
    </bean>
    <!--mybatis的sqlSessionFactory-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"></property>
        <property name="mapperLocations" value="classpath*:mapper/*.xml"></property>
       <property name="configLocation" value="classpath:mybatis-config.xml"></property>
    </bean>
    <!--mybatisplus的sqlSessionFactory-->
    <bean name="sessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"></property>
        <property name="mapperLocations" value="classpath*:mapper/*.xml"></property>
        <property name="configLocation" value="classpath:mybatis-config.xml"></property>
    </bean>
</beans>

```

#### web.xml配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee
         http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
  <display-name>SSM Work</display-name>
  <servlet>
    <!--这里配置的servlet-name为spring,则我们的spring配置文件需要被命名为spring-servlet，且应位于WEB-INF之下-->
    <servlet-name>spring</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    <load-on-startup>1</load-on-startup>
  </servlet>
    
  <servlet-mapping>
    <servlet-name>spring</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>
  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
  </welcome-file-list>
</web-app>

```

#### log4j配置文件

```properties
log4j.appender.Console=org.apache.log4j.ConsoleAppender
log4j.appender.console.Target=System.out
log4j.appender.Console.layout=org.apache.log4j.PatternLayout
log4j.appender.Console.layout.ConversionPattern=%d [%t] %-5p [%c] - %m%n

log4j.logger.org.apache=ERROR
log4j.logger.org.mybatis=ERROR
log4j.logger.org.springframework=ERROR


log4j.logger.log4jdbc.debug=ERROR
log4j.logger.com.gk.mapper=ERROR
log4j.logger.jdbc.audit=ERROR
log4j.logger.jdbc.resultset=ERROR

log4j.logger.jdbc.sqlonly=DEBUG
log4j.logger.jdbc.sqltiming=ERROR
log4j.logger.jdbc.connection=FATAL
```



## SSH (Spring SpringMVC Hibernate)框架的整合

其整合思路与前者是类似的，只是依赖和配置文件有所区别:

#### 依赖(pom.xml)

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>ink.rongxinyang</groupId>
  <artifactId>beta</artifactId>
  <packaging>war</packaging>
  <version>1.0</version>
  <name>beta Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <properties>
    <spring-version>5.3.3</spring-version>
    <hibernate-version>5.4.2.Final</hibernate-version>
  </properties>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
    <!--spring依赖--->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>${spring-version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-orm</artifactId>
      <version>${spring-version}</version>
    </dependency>
    <!--springmvc依赖--->  
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>${spring-version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>${spring-version}</version>
    </dependency>
    <!--数据库连接依赖--->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>8.0.32</version>
    </dependency>
    <!--lombok依赖--->
    <dependency>
      <groupId>org.projectlombok</groupId>
      <artifactId>lombok</artifactId>
      <version>1.18.26</version>
    </dependency>
    <!--json依赖--->
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.8.5</version>
    </dependency>
    <!--hibernate依赖-->
    <dependency>
      <groupId>org.hibernate.javax.persistence</groupId>
      <artifactId>hibernate-jpa-2.1-api</artifactId>
      <version>1.0.2.Final</version>
    </dependency>
    <dependency>
      <groupId>org.hibernate</groupId>
      <artifactId>hibernate-core</artifactId>
      <version>${hibernate-version}</version>
    </dependency>
  </dependencies>
  <build>
    <finalName>beta</finalName>
      <plugins>
          <plugin>
              <groupId>org.apache.maven.plugins</groupId>
              <artifactId>maven-compiler-plugin</artifactId>
              <configuration>
                  <source>8</source>
                  <target>8</target>
              </configuration>
          </plugin>
      </plugins>
  </build>
</project>

```

#### spring配置文件

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"     xmlns:context="http://www.springframework.org/schema/context"
       xmlns:mvc="http://www.springframework.org/schema/mvc"     xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/mvc http://www.springframework.org/schema/mvc/spring-mvc-3.2.xsd
http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-3.2.xsd
http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.2.xsd">
    <!--包扫描-->
    <context:component-scan base-package="ink.rongxinyang.hibernate"></context:component-scan>
    <!--启用事务注解-->
    <tx:annotation-driven/>
    <!--启用mvc注解-->
    <mvc:annotation-driven></mvc:annotation-driven>
    <!--设置默认的servlet-handler-->
    <mvc:default-servlet-handler></mvc:default-servlet-handler>
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <property name="prefix" value="/"/>
        <property name="suffix" value=".html"/>
    </bean>
    <bean class="org.springframework.jdbc.datasource.DriverManagerDataSource" name="dataSource">
        <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"></property>
        <property name="url" value="jdbc:mysql://localhost:3306/hibernate"></property>
        <property name="username" value="root"></property>
        <property name="password" value="root"></property>
    </bean>
    <!--创建hibernate的sessionFactory-->
    <bean class="org.springframework.orm.hibernate5.LocalSessionFactoryBean" name="factory">
        <property name="dataSource" ref="dataSource"></property>
        <property name="hibernateProperties">
            <props>
                <!--规定数据库方言-->
                <prop key="hibernate.dialect">org.hibernate.dialect.MySQLDialect</prop>
                <!--是否显示sql语句-->
                <prop key="hibernate.show_sql">true</prop>
                <!--设置数据库操作的模式-->
                <prop key="hibernate.hbm2ddl.auto">update</prop>
                <!--格式化sql语句-->
                <prop key="hibernate.format_sql">true</prop>
            </props>
        </property>
        <!--指定实体类-->
        <property name="annotatedClasses">
            <list>
                <value>ink.rongxinyang.hibernate.entity.MainTable</value>
                <value>ink.rongxinyang.hibernate.entity.StaffInfo</value>
            </list>
        </property>
    </bean>
    <!--根据sessionFactory来创建Hibernate模板-->
    <bean class="org.springframework.orm.hibernate5.HibernateTemplate" name="hibernateTemplate">
        <property name="sessionFactory" ref="factory"></property>
    </bean>
    <!--定义hibernate的事务管理器-->
    <bean
            class="org.springframework.orm.hibernate5.HibernateTransactionManager"
            name="hibernateTransactionManager"
    >
        <property name="sessionFactory" ref="factory"></property>
    </bean>
</beans>
```

大体上SSM和SSH的整合是类似的，只是在整合hibernate时我们需要进行一点额外的配置:

* 指定实体类(仅仅依赖hibernate的实体类注解是不可靠的)
* 定义hibernateTemplate

## SpringBoot框架

SpringBoot并非是一个新的框架，它只是将传统的SSM框架进行了简化，其理念是默认配置代替手动配置，因为实际开发中，我们搭建的环境很多地方都是重复的，那么SpringBoot便将这些重复的地方设置为默认值，这样我们便无需对其进行额外的配置了。因此我们的SpringBoot也就只存在一个核心配置文件即application.properties(.yml)。我们可以直接通过官网进行下载[SpringBoot](https://start.spring.io/)。

### 目录结构:

![img](.\springboot结构.png)

### 依赖(pom.xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.0.5</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
    <groupId>ink.rongxinyang.springboot</groupId>
    <artifactId>Spring</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <name>Spring</name>
    <description>Spring</description>
    <properties>
        <java.version>19</java.version>
    </properties>
    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.5.3.1</version>
        </dependency>
        <!--mybatis-plus时-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter-test</artifactId>
            <version>3.5.3.1</version>
        </dependency>
        <!--mybatis时-->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
           <version>3.0.1</version>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.32</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.17</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>

</project>

```

### application.yml 配置文件

```yaml
spring:
  datasource:
    username: root
    password: root
    url: jdbc:mysql:///hibernate
    driver-class-name: com.mysql.cj.jdbc.Driver
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  mapper-locations: classpath*:mapper/*xml
```

### Application.java

```java
package ink.rongxinyang.springboot.spring;

import org.mybatis.spring.annotation.MapperScan;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
/*表示该类为SpringBoot的启动类*/
@SpringBootApplication
/*扫描mapper包(mybatis(-plus))*/
@MapperScan("ink.rongxinyang.springboot.spring.mapper")
public class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}
```

## SpringCloud框架

在传统的单体项目中，我们的一个程序包含了各种不同的模块功能，这样首先会导致代码繁杂，维护成本高，且耦合度很高。为了解决这个问题，我们可以借助微服务的解决方案，而这个微服务的核心思想即为服务的拆分:当我们希望设计一个程序时，我们可以将不同的功能模块分离成一个单独的项目，这要一个项目只做一个功能，我们首先可以实现开发的分离，也可实现部署的分离，配置的分离和数据的分离，因为它们都是一个单独的项目。而且在这个基础上，我们还可以为每一个功能构建一个集群以降低服务器压力与保证其高可用性。

**当然在使用微服务架构时，我们需要也需要面临许多，问题:**

* 服务间的相互调用以及调用关系的维护，因为我们在微服务架构下，每一个功能都是一个单独的项目，那么如果服务间的需要相互调用，在传统的单体应用内时可以直接进行的，而在微服务架构中则需要依赖http协议(feign)进行接口的调用，我们也可以使用**Spring中的RestTemplate**来发送请求。此时每个服务的接口地址就需要被维护，此时我们便可以使用注册中心来实现这一点。一个注册中心将会保存所有的服务的接口信息，当一个服务需要调用另一个服务时，则可以通过注册中心获取目标服务的接口地址以进行调用而获得结果

* 我们提到，当我们使用微服务时可以实现配置的分离，与此同时我们也需要将一些共有配置进行管理以避免<u>手动管理</u>(**不具有现实意义**)，此时我们便可以使用配置中心来实现配置的管理

* 服务器的高可用性，注册中心的心跳机制可以保证获取的服务器都可高可用的

  ....

### SpringCloud的依赖

```xml
 <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-dependencies</artifactId>
            <version>${spring.cloud-version}</version>
            <type>pom</type>
            <scope>import</scope>
</dependency>
```



### 注册中心

注册中心也就是我们实现微服务间沟通的媒介，其会保存各个服务的接口，当一个服务需要调用一个指定的服务时，注册中心将可以根据需求来提供响应的接口地址，以实现服务间的通信，除此之外，其心跳机制亦保证了每一个服务的高可用性。而搭建一个注册中心分为了三步:

### eureka注册中心

* 引依赖：

  * ```xml
      <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
                <version>${eureka-version}</version>
      </dependency>
    ```

* 配置文件

  * ```yaml
    spring:
      application:
        name: eurekaserver #定义注册中心的名称
    ```

  * ```yaml
    eureka:
      client:
        service-url:        #定义注册中心的地址，如果注册中心存在集群，则以逗号分隔。因为一个注册中心需要向所有注册中心提供自身的信息(包括自己)
          defaultZone: http://localhost:9413/eureka
        fetch-registry: false #不从eureka中获取注册信息，因为自身就是注册zhong'x
    ```

* 启动类上添加注解(**@EnableEurekaServer**):

  * ```java
    @SpringBootApplication
    @EnableEurekaServer
    public class EurekaApplication {
    
        public static void main(String[] args) {
            SpringApplication.run(EurekaApplication.class, args);
        }
    
    }
    ```

当我们配置了注册中心后，我们便需要创建服务端了，我们的每一个服务端既是一个消费者亦为一个提供者。所以我们需要将自身的信息全部注册到注册中心中，其配置与前者类似:

* 引依赖:

  * ```xml
     <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
                <version>${eureka-version}</version>
     </dependency>
    ```

* 配置文件:

  * ```yaml
    spring
       application:
        name: orderservice #必须为每一个服务设置一个服务名，当设置一个服务名后，当该服务设置集群后，我们的注册中心可以根据该服务名来判断该集群下的						   服务器是属于某一个服务的，而后，我们请求时，直接请求该服务名即可，ribbon组件会根据从注册中心中拉取的信息将服务名转换						为可用的服务接口地址
    eureka:
      client:
        service-url:	   #指定该服务所使用的注册中心地址，同样的这里也可以存在多个注册中心地址
          defaultZone: http://localhost:9413/eureka
    ```

* (**启动类添加EnableDiscoveryClient注解**)

  * ```java
    @SpringBootApplication
    @EnableDiscoveryClient
    public class UserServiceApplication {
    	public static void main(String[] args) {
    		SpringApplication.run(UserServiceApplication.class, args);
    	}
    
    }
    ```

### 负载均衡

当我们的微服务都搭建了集群，那么基于保证服务器的最佳执行效率以提高响应速度，我们便需要实现负载均衡，而负载均衡的实现则需要依赖于**Ribbon组件**。如果我们需要手动的实现负载均均衡，则我们可以在声明RestTemplate时为之添加负载均衡的注解(**@LoadBalanced**)：

```java
  @Bean
  @LoadBalanced
  public RestTemplate restTemplate(){
        return new RestTemplate();
  }
```

#### Ribbon的执行流程

![""](.\ribbon.png)

#### 负载几种均衡的策略

![""](.\负载均衡策略.png)

##### 定义IRule的对象配置策略

如果说我们需要修改默认的负载均衡策略，我们可以声明一个IRole对象，而后为之创建一个指定的实现策略对象:

```java
@Bean
public IRule randomRule(){
    return new RandomRule();
}
```

##### 配置文件指定策略

**该种配置是全局配置，项目中所有的微服务都将遵循该种策略(注意，在调用者中配置被调用者，如这里的配置就应当配置在orderservice中，因为，orderservice调用了userservice)**

```yaml
userservice:
	ribbon:
		NFLoadBalancerRuleClassName: com.netflix.loadbalancer.RandomRule #定义策略
```

#### 饥饿加载

Ribbon在默认情况下是在第一次被访问时才会创建LoadBalanceLoader对象的，那么这也就意味着第一次加载会耗时较长。为了使服务器在启动时就直接创建该对象，我们可以为其配置为饥饿加载模式:

```yaml
robbon:
	eager-load:
		enabled: true #开启饥饿加载
		clients: userservice #指定开启饥饿加载的服务的名称
```

### nacos注册中心

前面我们提到了eureka的注册中心，接下来我们来研究一下阿里的nacos注册中心。nacos注册中心相较于前面的eureka注册中心来说功能更加丰富，而使用它也是很方便的。当我们使用eureka时，我们需要首先创建一个注册中心的工程，首先启动这个注册中心的工程，然后才能使用。在使用nacos时，我们可以选择直接进行安装，而后进行命令行启动，这个内置的nacos项目就可以直接被启动了。

因此，我们对于使用nacos注册中心的服务端，直接声明为服务端即可,其声明步骤为:

* **引入依赖**:

  * ```xml
    	    <dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-dependencies</artifactId>
                <version>2.2.0.RELEASE</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
          		<dependency>
                <groupId>com.alibaba.nacos</groupId>
                <artifactId>nacos-client</artifactId>
                <version>2.1.1</version>
            </dependency>
          		<dependency>
                <groupId>com.alibaba.cloud</groupId>
                <artifactId>spring-cloud-alibaba-nacos-discovery</artifactId>
                <version>2.0.0.RELEASE</version>
            </dependency>
    ```

* **配置注册中心地址:**

  * ```yaml
      spring:
      	application:
        	name: userservice
      	cloud: #nacos配置
        	nacos:
          	discovery:
            	server-addr: localhost:8848
    ```

* (**添加@EnableClientDiscovery注解**):

```java
@SpringBootApplication
@EnableDiscoveryClient
@MapperScan("ink.rongxinyang.userservice.mapper")
public class UserServiceApplication {
	public static void main(String[] args) {
		SpringApplication.run(UserServiceApplication.class, args);
	}
}
```

#### nacos的分级存储模型

默认情况下，我们的nacos会将同一个机房内的所有的同一服务的实例视为一个默认集群的实例，而如果说几个不同机房甚至地区内的实例，我们时可以为之添加不同的集群名称的，也就是说我们为服务的实例划分了：**服务**-->**集群**-->**实例**三级，我们两个服务间调用优先调用同一个集群内的服务的实例**(Nacos默认使用的也是轮询策略，如果希望实现同一集群内优先调用，则需要进行额外的配置)**，而如果我们希望为某一个服务添加集群名称，我们可以修改配置文件:

```yaml
spring：
  cloud: #nacos配置
    nacos:
      discovery:
        server-addr: localhost:8848
        cluster-name: Houston  #规定当前所属集群的名称
```

对于**IUser**的配置(实现同一集群优先原则):

```yaml
userservice:
  ribbon:
    NFLoadBalancerRuleClassName: com.alibaba.cloud.nacos.ribbon.NacosRule
```

#### nacos服务实例的权重(0~1之间)设置

当我们同一个服务具有多个实例时，这里每个实例所使用的服务器其性能是并不相同的，那么我们需要适当的增加性能较好的服务器的请求次数而减少性能较差的服务器的访问次数以合理分配资源。除此之外，我们亦可以通过将权重设置为0来避免请求的接入,此时如不系统的更新升级就可以悄无声息(分批升级)的实现了。

##### 修改实例的权重:

**进入实例列表，点击修改**

![""](.\权重修改.png)

**修改权重**

![""](.\权重修改2.png)

#### 环境隔离(namespace)

我们在nacos中可以创建namespace,每一个namespace都是一个单独的环境，**每两个命名空间之间的实例将不可以相互访问**(<u>**默认为指定命名空间是将会将实例统一置于public命名空间之下**</u>)。

##### 创建命名空间:

**进入命名空间中，点击创建命名空间:**

![""](.\命名空间创建01.png)

![""](.\命名空间02.png)

当我们生成了命名空间后我们可以通过将id设置于服务中以为之指定命名空间

##### 配置文件指定命名空间

```yaml
spring：
  cloud: #nacos配置
    nacos:
      discovery:
        server-addr: localhost:8848
        cluster-name: Houston  #规定当前所属集群的名称
		namespace: bcc571cd-3987-4198-8c84-e77b0195116a
```

### nacos的配置中心

我们的nacos既可以作为注册中心，亦可作为配置中心，而配置中心则要求我们在读取application.yml之前便首先读取配置中心中配置文件的内容。此时我们可以添加一个bootstrap.yml文件，这个文件将会在application.yml之前被读取，因此我们可以将配置中心中的配置文件的信息先告诉它.

#### nacos配置中心的设置：

##### 创建配置中心的文件

![""](.\配置中心.png)

##### 引入依赖

```xml
		<dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-nacos-config</artifactId>
            <version>2.0.0.RELEASE</version>
        </dependency>
```

##### 创建bootstrap.yml

````yaml
spring:
  application:
    name: orderservice 
  profiles:
    active: dev
  cloud:
    nacos:
      config:
        file-extension: yaml #配置文件后缀名
        server-addr: localhost:8848 #指定注册中心
      discovery:
        server-addr: localhost:8848 #指定配置中心
````

#### 实现配置文件的热更新

##### 添加@RefreshScope注解

如果说我们希望使用**@Value**注解来为变量赋值，则我们需要在类上添加**@RefreshScope**注解:

```java
@RestController
...
@RefreshScope
public class Main{
    @Value("${....prop}")
    private String prop;
    ...
}
```

##### 使用配置文件配置类实现

**配置类**

```java
@Component
@Data
@ConfigurationProperties(prefix="....")
public class Props{
    private String prop;
}
```

**直接调用**

```java
@RestController
...
public class Main{
    @AutoWire
    private Props props;
    ...
        ...=props.getProp();
    ...
}
```

**<a style="color:rgba(255,0,0,0.65);font-size:1.25rem">由此我们注意到，我们通过添加<u>@RefreshScope</u>的方式和直接使用配置类的方法都可以实现相同的效果</a>**

#### nacos配置管理

SpringCloud其启动后或依次启动三种文件:

```txt
servicename-profile.yml --> servicename.yml  --> application.yml
```

前两者均为服务器在线配置，优先级均高于本地配置的application，而在服务器配置中，服务名-环境名(dev或者test等).yml的属于当前环境配置，其优先级高于服务名.yml。

### Nacos和Eureka的区别

* 我们的eureka对于实例采用的是心跳检测机制，也就是当一个实例没有心跳了，eureka就会将其判别为不健康状态。而nacos则首先将所有实例分为临时实例和非临时实例两种，其中默认是临时实例，而临时实例的的健康判断于eureka是一致的，只是频率会更高，而且一旦一个实例被判断为不健康就会被直接剔除。而非临时实例则采用是主动问询机制，也就是nacos将会不断地向服务端闻讯其是否健康，不健康则设置为不健康状态，注意与前者不同的是，它并会不会从列表中将实例剔除。
* 消费者每个三十秒会向eureka拉取可用的服务列表，这样就存在某一个时间点内可用列表不够准确。为此，nacos采用了拉取和推送两者方式，这样，在nacos检测到某一个服务不可用后会立刻将消息推送给其他服务，以保证其他服务的可用列表是高可用的。

#### 配置非临时实例

```yaml
spring：
  cloud: #nacos配置
    nacos:
      discovery:
      	ephemeral: false
```

### nacos集群搭建

#### 创建数据库

```sql
CREATE TABLE `config_info` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `data_id` varchar(255) NOT NULL COMMENT 'data_id',
  `group_id` varchar(255) DEFAULT NULL,
  `content` longtext NOT NULL COMMENT 'content',
  `md5` varchar(32) DEFAULT NULL COMMENT 'md5',
  `gmt_create` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `gmt_modified` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '修改时间',
  `src_user` text COMMENT 'source user',
  `src_ip` varchar(50) DEFAULT NULL COMMENT 'source ip',
  `app_name` varchar(128) DEFAULT NULL,
  `tenant_id` varchar(128) DEFAULT '' COMMENT '租户字段',
  `c_desc` varchar(256) DEFAULT NULL,
  `c_use` varchar(64) DEFAULT NULL,
  `effect` varchar(64) DEFAULT NULL,
  `type` varchar(64) DEFAULT NULL,
  `c_schema` text,
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_configinfo_datagrouptenant` (`data_id`,`group_id`,`tenant_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='config_info';

/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = config_info_aggr   */
/******************************************/
CREATE TABLE `config_info_aggr` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `data_id` varchar(255) NOT NULL COMMENT 'data_id',
  `group_id` varchar(255) NOT NULL COMMENT 'group_id',
  `datum_id` varchar(255) NOT NULL COMMENT 'datum_id',
  `content` longtext NOT NULL COMMENT '内容',
  `gmt_modified` datetime NOT NULL COMMENT '修改时间',
  `app_name` varchar(128) DEFAULT NULL,
  `tenant_id` varchar(128) DEFAULT '' COMMENT '租户字段',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_configinfoaggr_datagrouptenantdatum` (`data_id`,`group_id`,`tenant_id`,`datum_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='增加租户字段';


/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = config_info_beta   */
/******************************************/
CREATE TABLE `config_info_beta` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `data_id` varchar(255) NOT NULL COMMENT 'data_id',
  `group_id` varchar(128) NOT NULL COMMENT 'group_id',
  `app_name` varchar(128) DEFAULT NULL COMMENT 'app_name',
  `content` longtext NOT NULL COMMENT 'content',
  `beta_ips` varchar(1024) DEFAULT NULL COMMENT 'betaIps',
  `md5` varchar(32) DEFAULT NULL COMMENT 'md5',
  `gmt_create` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `gmt_modified` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '修改时间',
  `src_user` text COMMENT 'source user',
  `src_ip` varchar(50) DEFAULT NULL COMMENT 'source ip',
  `tenant_id` varchar(128) DEFAULT '' COMMENT '租户字段',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_configinfobeta_datagrouptenant` (`data_id`,`group_id`,`tenant_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='config_info_beta';

/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = config_info_tag   */
/******************************************/
CREATE TABLE `config_info_tag` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `data_id` varchar(255) NOT NULL COMMENT 'data_id',
  `group_id` varchar(128) NOT NULL COMMENT 'group_id',
  `tenant_id` varchar(128) DEFAULT '' COMMENT 'tenant_id',
  `tag_id` varchar(128) NOT NULL COMMENT 'tag_id',
  `app_name` varchar(128) DEFAULT NULL COMMENT 'app_name',
  `content` longtext NOT NULL COMMENT 'content',
  `md5` varchar(32) DEFAULT NULL COMMENT 'md5',
  `gmt_create` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `gmt_modified` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '修改时间',
  `src_user` text COMMENT 'source user',
  `src_ip` varchar(50) DEFAULT NULL COMMENT 'source ip',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_configinfotag_datagrouptenanttag` (`data_id`,`group_id`,`tenant_id`,`tag_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='config_info_tag';

/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = config_tags_relation   */
/******************************************/
CREATE TABLE `config_tags_relation` (
  `id` bigint(20) NOT NULL COMMENT 'id',
  `tag_name` varchar(128) NOT NULL COMMENT 'tag_name',
  `tag_type` varchar(64) DEFAULT NULL COMMENT 'tag_type',
  `data_id` varchar(255) NOT NULL COMMENT 'data_id',
  `group_id` varchar(128) NOT NULL COMMENT 'group_id',
  `tenant_id` varchar(128) DEFAULT '' COMMENT 'tenant_id',
  `nid` bigint(20) NOT NULL AUTO_INCREMENT,
  PRIMARY KEY (`nid`),
  UNIQUE KEY `uk_configtagrelation_configidtag` (`id`,`tag_name`,`tag_type`),
  KEY `idx_tenant_id` (`tenant_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='config_tag_relation';

/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = group_capacity   */
/******************************************/
CREATE TABLE `group_capacity` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `group_id` varchar(128) NOT NULL DEFAULT '' COMMENT 'Group ID，空字符表示整个集群',
  `quota` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '配额，0表示使用默认值',
  `usage` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '使用量',
  `max_size` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '单个配置大小上限，单位为字节，0表示使用默认值',
  `max_aggr_count` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '聚合子配置最大个数，，0表示使用默认值',
  `max_aggr_size` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '单个聚合数据的子配置大小上限，单位为字节，0表示使用默认值',
  `max_history_count` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '最大变更历史数量',
  `gmt_create` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `gmt_modified` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '修改时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_group_id` (`group_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='集群、各Group容量信息表';

/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = his_config_info   */
/******************************************/
CREATE TABLE `his_config_info` (
  `id` bigint(64) unsigned NOT NULL,
  `nid` bigint(20) unsigned NOT NULL AUTO_INCREMENT,
  `data_id` varchar(255) NOT NULL,
  `group_id` varchar(128) NOT NULL,
  `app_name` varchar(128) DEFAULT NULL COMMENT 'app_name',
  `content` longtext NOT NULL,
  `md5` varchar(32) DEFAULT NULL,
  `gmt_create` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `gmt_modified` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP,
  `src_user` text,
  `src_ip` varchar(50) DEFAULT NULL,
  `op_type` char(10) DEFAULT NULL,
  `tenant_id` varchar(128) DEFAULT '' COMMENT '租户字段',
  PRIMARY KEY (`nid`),
  KEY `idx_gmt_create` (`gmt_create`),
  KEY `idx_gmt_modified` (`gmt_modified`),
  KEY `idx_did` (`data_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='多租户改造';


/******************************************/
/*   数据库全名 = nacos_config   */
/*   表名称 = tenant_capacity   */
/******************************************/
CREATE TABLE `tenant_capacity` (
  `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '主键ID',
  `tenant_id` varchar(128) NOT NULL DEFAULT '' COMMENT 'Tenant ID',
  `quota` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '配额，0表示使用默认值',
  `usage` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '使用量',
  `max_size` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '单个配置大小上限，单位为字节，0表示使用默认值',
  `max_aggr_count` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '聚合子配置最大个数',
  `max_aggr_size` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '单个聚合数据的子配置大小上限，单位为字节，0表示使用默认值',
  `max_history_count` int(10) unsigned NOT NULL DEFAULT '0' COMMENT '最大变更历史数量',
  `gmt_create` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  `gmt_modified` datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '修改时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_tenant_id` (`tenant_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='租户容量信息表';


CREATE TABLE `tenant_info` (
  `id` bigint(20) NOT NULL AUTO_INCREMENT COMMENT 'id',
  `kp` varchar(128) NOT NULL COMMENT 'kp',
  `tenant_id` varchar(128) default '' COMMENT 'tenant_id',
  `tenant_name` varchar(128) default '' COMMENT 'tenant_name',
  `tenant_desc` varchar(256) DEFAULT NULL COMMENT 'tenant_desc',
  `create_source` varchar(32) DEFAULT NULL COMMENT 'create_source',
  `gmt_create` bigint(20) NOT NULL COMMENT '创建时间',
  `gmt_modified` bigint(20) NOT NULL COMMENT '修改时间',
  PRIMARY KEY (`id`),
  UNIQUE KEY `uk_tenant_info_kptenantid` (`kp`,`tenant_id`),
  KEY `idx_tenant_id` (`tenant_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='tenant_info';

CREATE TABLE `users` (
	`username` varchar(50) NOT NULL PRIMARY KEY,
	`password` varchar(500) NOT NULL,
	`enabled` boolean NOT NULL
);

CREATE TABLE `roles` (
	`username` varchar(50) NOT NULL,
	`role` varchar(50) NOT NULL,
	UNIQUE INDEX `idx_user_role` (`username` ASC, `role` ASC) USING BTREE
);

CREATE TABLE `permissions` (
    `role` varchar(50) NOT NULL,
    `resource` varchar(255) NOT NULL,
    `action` varchar(8) NOT NULL,
    UNIQUE INDEX `uk_role_permission` (`role`,`resource`,`action`) USING BTREE
);

INSERT INTO users (username, password, enabled) VALUES ('nacos', '$2a$10$EuWPZHzz32dJN7jexM34MOeYirDdFAZm2kuWj7VEOJhhZkDrxfvUu', TRUE);

INSERT INTO roles (username, role) VALUES ('nacos', 'ROLE_ADMIN');
```

#### Nginx的反向代理(nacos集群的负载均衡)

**以下配置只需在http对象内即可：**

```nginx
upstream nacos-cluster {
    #nacos集群的ip列表
	server 127.0.0.1:8846; 
	server 127.0.0.1:8848;
}

server {
    listen       80;
    server_name  localhost;

    location /nacos {
        proxy_pass http://nacos-cluster;
    }
}
```

### Feign的使用

当我们使用Restemplate进行远程调用时，我们注意到，我们首先的控制url，然后使用url进行操作，这样维护起来将会更加繁琐，为此我们可以使用OpenFiegn进行操作，这是一个声明式的接口调用方法，我们仅需声明一个接口，并为其定义方法，而后我们便可以直接通过该接口调用方法的方式来调用接口了.

#### 引入依赖

```xml
 		<dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
```

#### 添加注解

```java
@SpringBootApplication
@MapperScan("ink.rongxinyang.orderservice.mapper")
@EnableDiscoveryClient
/*开启OpenFeign功能*/
@EnableFeignClients
public class OrderServiceApplication {

    public static void main(String[] args) {
        SpringApplication.run(OrderServiceApplication.class, args);
    }
    @Bean
    @LoadBalanced
    public RestTemplate restTemplate(){
        return new RestTemplate();
    }
}
```

#### 声明接口

```java
@FeignClient("userservice")
public interface UserClient {
    @RequestMapping("/user/{id}")
    User findById(@PathVariable("id")Integer id);
}
```

此时我们调用findById方法即可发送uri/user/id的请求了

#### feign的日志等级的设置

##### 日志等级

在默认情况下，我们的feign时没有日志的，因为默认日志等级为NULL。而实际上Feign拥有四个日志等级:

* **NULL**				没有任何日志
* **BASIC**               仅输出基本日志(发送和响应的时间，时长等)
* **HEADERS**         输出头信息
* **FULL**                 输出所有日志

#####  日志配置

**配置文件方式(全局模式)**:

```yaml
feign:
	client:
		config:
			default:
				loggerLevell: FULL
```

**配置文件方式(仅对userservice生效):**

```yaml
feign:
	client:
		config:
			userservice:
				loggerLevell: FULL
```

**Java配置类的方式(全局模式)**

```java
public class FeignCLientConfiguration{
    @Bean
    public Logger.Level feignLogLevel(){
        return Logger.Level.FULL;
    }
}
```

```java
@EnableFeignClients(defaultConfiguration = FeignClientConfiguration.class);
```

**Java配置类的方式(指定模式)**

```java
@FeignClient(value="userservice",configuration=FeignCLientConfiguration.class)
```

#### Feign的性能优化

我们的Feign在底层时使用了Java自带的BaseURLConnection进行的请求的创建等处理操作，其是不支持连接池的操作的，因此它的性能并不是最佳的。为了解决这一问题，我们可以改变Feign的底层实现:

##### 引入依赖

```xml
		 <dependency>
            <groupId>io.github.openfeign</groupId>
            <artifactId>feign-httpclient</artifactId>
        </dependency>
```

##### 配置文件

```yaml
feign:
	httpclient:
		enabled: true			#开启feign对于httpClient的支持
		max-connection: 200		#最大的连接数
		max-connection-per-route: 50	#每个路径的最大连接数
```

#### feign的优化实践

在上面的例子中，我们是通过手动创建feignClient的手段来实现的feign的使用的。而在这种方式的情况下意味着我们需要在每一个需要使用该接口的服务中都需要手动创建。那么这就出现了重复开发的问题，为了解决这个问题，我们可以将这些通用的功能都单独提取出来，再单独写作一个项目。而后其他需要使用该功能的服务，我们仅需引入依赖即可。

##### 遇到的问题和解决方案

当我们通过引入依赖的方式引入了feign的功能时，我们时无法实现Feign接口的AutoWire的依赖注入的，因为我们启动时，包扫描将不会包含我们所引入的以来的包路径的(常规情况下)，为了解决这个问题，我们便需要为@EnableFeignClients的注解添加一些额外参数即可:

```java
@EnableFeignClients(basePackages = "...") 		/*指定包名*/
@EnableFeignClients(clients={UserClient.class}) /*指定所需要的客户端的类名*/
```

### 网关的使用

我们在访问我们的服务的时候，需要设置网关以实现服务请求的负载均衡和请求的分发，流量控制等操作。而我们在Spring中支持目前最常用的时gateway网关。除此之外，我们需要了解的是，我们的网关，实际上也是一个服务，因此它也需要被单独创建为一个项目，而且其应当是需要实现服务的发现的，因此其本身应当是可以被注册中心发现的，因此我们所需要引入的依赖如下：

#### 依赖引入

```xml
		<dependency>
            <groupId>com.alibaba.cloud</groupId>
            <artifactId>spring-cloud-alibaba-nacos-discovery</artifactId>
            <version>2.0.0.RELEASE</version>
        </dependency>
		<dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-gateway</artifactId>
        </dependency>
```

#### 配置文件

```yaml
server:
  port: 22400
spring:
	application:
		name: gateway
	cloud:
	    nacos:
      		discovery:
        	server-addr: localhost:8848
    	gateway:
      		routes:   #网关的配置
        		- id: user-service  #路由id，需保证唯一
          		uri: lb://userservice #lb表示负载均衡，这里将会访问 http://127.0.0.1:9412  (当然实际按照注册中心中的地址仅需负载均衡后调用)
          		predicates:   #添加断言，即判断请求是否符合路由要求
           			 - Path=/fetch/user/** #添加路由的断言，如果匹配的地址为/user/开头即匹配成功，则选取该路由
        		- id: order-service
         		uri: lb://orderservice
          		predicates:
            		- Path=/fetch/order/**
```

#### 访问网关

此时我们便可以通过网关来实现服务的调用了:

```sh
http://localhost:22400/fetch/user/1 #此时将会访问http://userservice/fetch/user/1
http://localhost:22400/fetch/order/1001 #此时将会访问http://userservice/fetch/order/1001
```

**此时，网关还会在分发请求时进行负载均衡**

#### 路由的断言工厂

前面我们使用了PATH断言工厂，实际上这里存在了11个常用的断言工厂:

| Predicate Factory                  | Description                                                  |
| :--------------------------------- | :----------------------------------------------------------- |
| After Route Predicate Factory      | Matches requests that happen after the current datetime.     |
| Before Route Predicate Factory     | Matches requests that happen before the current datetime.    |
| Between Route Predicate Factory    | Matches requests that happen after datetime1 and before datetime2. |
| Cookie Route Predicate Factory     | Matches cookies that have the given name and the value matches the regular expression. |
| Header Route Predicate Factory     | Matches with a header that has the given name and the value matches the regular expression. |
| Host Route Predicate Factory       | Takes one parameter: a list of host name patterns.           |
| Method Route Predicate Factory     | Matches if the request’s HTTP method is the same as the configured method. |
| Path Route Predicate Factory       | Matches if the request’s path matches the given pattern.     |
| Query Route Predicate Factory      | Matches if the request’s query parameter matches the given name and regular expression. |
| RemoteAddr Route Predicate Factory | Takes a list of CIDR-notation strings and matches if the request’s remote address matches. |
| Weight Route Predicate Factory     | Uses a weighted random selection to route requests to different URIs. |

#### 路由的过滤器工厂

我们的请求和响应在通过网关进行转发时，其首先需要经过路由，除此之外，它们还需经过一层一层的过滤器，而这些过滤器便可以对于请求和响应进行一些操作，如添加一些头文件等, SpringCloud中总共提供了31种不同的过滤器工厂，这里我们列举几个常用几个过滤器工厂，详细的请访问SpringCloud官网:

| GatewayFilter Factory                      | Description                                                  |
| :----------------------------------------- | :----------------------------------------------------------- |
| AddRequestHeader GatewayFilter Factory     | Adds a header to the downstream request’s headers for all matching requests. |
| AddRequestParameter GatewayFilter Factory  | Adds a parameter to the downstream request’s query string for all matching requests. |
| AddResponseHeader GatewayFilter Factory    | Adds a header to the downstream response’s headers for all matching requests. |
| DedupeResponseHeader GatewayFilter Factory | Removes duplicate values of specified response headers.      |
| PrefixPath GatewayFilter Factory           | Prefixes the request path with a specified value.            |
| PreserveHostHeader GatewayFilter Factory   | Preserves the original host header when proxying requests.   |
| RedirectTo GatewayFilter Factory           | Redirects requests to a specified URI.                       |
| RemoveRequestHeader GatewayFilter Factory  | Removes a header from the downstream request’s headers for all matching requests. |
| RemoveResponseHeader GatewayFilter Factory | Removes a header from the downstream response’s headers for all matching requests. |
| RewritePath GatewayFilter Factory          | Rewrites the request path using a regular expression and replacement pattern. |
| SetPath GatewayFilter Factory              | Sets the request path using an expression.                   |

#### 添加过滤器工厂

##### 为指定的路由添加过滤器工厂

```yaml
...:
	gateway:
      		routes:   #网关的配置
        		- id: user-service  #路由id，需保证唯一
          		uri: lb://userservice #lb表示负载均衡，这里将会访问 http://127.0.0.1:9412  (当然实际按照注册中心中的地址仅需负载均衡后调用)
          		predicates:   #添加断言，即判断请求是否符合路由要求
           			 - Path=/fetch/user/** #添加路由的断言，如果匹配的地址为/user/开头即匹配成功，则选取该路由
           			 filters:
           			 - AddRequestHeader=Message,Hello World! #此时user-service的请求路由为/fetch/user/时将会被添加一个请求头Message,且内容为Hello World!；注意此时的,等价于=
```

##### 添加通用的过滤器工厂

```yaml
...:
	gateway:
      		routes:   #网关的配置
        		- id: user-service  #路由id，需保证唯一
          		uri: lb://userservice #lb表示负载均衡，这里将会访问 http://127.0.0.1:9412  (当然实际按照注册中心中的地址仅需负载均衡后调用)
          		predicates:   #添加断言，即判断请求是否符合路由要求
           			 - Path=/fetch/user/** #添加路由的断言，如果匹配的地址为/user/开头即匹配成功，则选取该路由
           		default-filters:
           			 - AddRequestHeader=Message,Hello World! #此时user-service的所有路由请求都将会被添加一个请求头Message,且内容为Hello World!；注意此时的,等价于=
```

#### 全局过滤器

虽然我们可以为请求和响应添加内置的过滤器工厂，但是涉及到到复杂的过滤操作，我们还是需要使用Java来手动配置，而此时因为gateway时基于flux开发的，因此我们需要可以通过一个GlobalFilter接口来将一个类声明为一个全局过滤器类:

```java
@Component /*声明为组件*/
@Order(-1) /*声明过滤器的执行顺序，值越小则最先执行*/
public class UserAuthFilter implements GlobalFilter {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        ServerHttpRequest request = exchange.getRequest(); /*获取一个请求对象*/
        MultiValueMap<String, String> auth = request.getQueryParams(); /*获取一个请求参数*/
        String authStr = auth.getFirst("auth");
        if(authStr.equals("admin"))
           return chain.filter(exchange); /*放行请求，交由下一个过滤器进行处理，该方法返回一个Mono<void>对象*/
        else {
            ServerHttpResponse response = exchange.getResponse();  /*获取一个响应对象*/
            response.setStatusCode(HttpStatus.UNAUTHORIZED); /*设置状态码*/
            return response.setComplete(); /*结束请求，并返回响应，该方法返回一个Mono<void>对象*/
        }
    }
}
```

我们注意到这里的过滤器的使用和servlet中的过滤器是类似的。

#### 过滤器的执行顺序

* 每一种过滤器(default，局部，全局)都是存在顺序的，其中default和局部过滤器的顺序是按照声明的顺序进行的，它们没添加一个过滤器便会将order加一，且它们两者是分开计数的，而全局过滤器的order有我们手动定义
* 当三种过滤器order重复时，其遵循**default>局部>全局**

#### 网关跨域解决方案

解决跨域的问题，我们的的核心是为响应头添加允许跨域的信息，在网关中亦是如此。只是在网关中我们可以通过配置的方法解决:

```yaml
spring:
	cloud:
		gateway：
			globalcors:		#全局跨域处理
				add-simple-url-handler-mapping: true	#解决options被拦截问题
				corsConfigurations:
					'[/**]':
						allowedOrigins:		#允许哪些网站的跨域请求
							- "http://localhost:8090"
							- "http://www.leyou.com"
						allowedMethods:		#允许跨域的ajax的请求方式
							- "GET"
							- "POST"
							- "DELETE"
							- "PUT"
							- "OPTIONS"
						allowedHeaders: "*"		#允许的请求中携带的头信息
						allowcredentials: true	#是否允许携带cookie
						maxAge: 360000			#设置跨域的时间，每申请一次跨域，该段时间内将直接放行(单位为秒)
```

## Docker

在传统的部署中，我们面对多种不同的环境进行部署时，我们需要考虑不同的允许环境的因素，如我们在ubuntu上进行打包，而后在centos上面进行部署，则我们部署的应用将会失败，因为，环境的不同，导致我们的应用无法调用某些需要的功能。而且这些问题不仅仅只会出现在跨系统时，某些情况下，系统的不同版本也会导致这样的问题。除此之外，当我们同时在一个系统上部署一个项目，我们还需要启动某些其他的服务，如数据库等，此时项目和这些服务之间的环境和依赖是相互影响的。

为了解决这个问题，我们便可以使用docker来实现，因为docker将会将每一个服务和功能进行打包，使其成为一个独立的镜像，这个镜像是完全隔离的，这首先**保证了环境的隔离性**；而且，我们的**docker将会将每一个应用所需要的，系统函数库，依赖和环境都打包在这个镜像中**，因此这些镜像被置于一个容器后便可以<u>**直接被运行**</u>。<a style="background:orange;color:white">这镜像的原理是基于linux系统都是采用了同样的内核来实现的，因为linux都是采用了同一个内核，真正不同的是系统软件而已，我们的docker只需要直接调用内核便可以直接实现所需的功能。</a>

而docker应用便是可以进行打包和部署的媒介，其是一个CS架构，我们通过想docker服务端<u>**发送指令(本地)**</u>或者<u>**RestAPI的请求(远程)**</u>。

### Docker命令:

#### 镜像的命名规范:

```sh
#reporitory:version
mysql:8.3 #表示MySQL8.3版本，如果没有指定版本则默认版本为latest
```

#### docker本地命令

|     命令      |          意义          |
| :-----------: | :--------------------: |
| docker build  |        构建建镜        |
| docker images |        查看镜像        |
|  dockers rmi  |        删除镜像        |
|  docker pull  |        拉取镜像        |
|  docker push  |     推送镜像至服务     |
|  docker save  | 将镜像保存为一个压缩包 |
|  docker load  |    将压缩的镜像加载    |

#### docker容器命令

|      命令      |                             意义                             |
| :------------: | :----------------------------------------------------------: |
|   docker run   |                           启动镜像                           |
|  docker pause  |                  暂停镜像运行((进程被挂起)                   |
| docker unpause |                         恢复镜像运行                         |
|  docker stop   |        停止镜像运行(进程被杀死，内存清空，仅保留文件)        |
|  docker start  |                  开始镜像运行(开启新的进程)                  |
|  docker exec   |                       进入容器执行命令                       |
|  docker logs   |                      查看容器运行的日志                      |
|   docker ps    |    查看所有**运行的**容器及其状态，查看所有容器添加参数-a    |
|   docker rm    | 删除容器(杀死进程，内存清空，删除文件)，无法删除正在运行的容器，强制删除添加参数-f |

#### 修改容器内的文件

我们可以使用docker exec来实现对于容器内部文件的修改:

```bash
docker exec -it name bash # -it表示创建标准输入输出流 name表示所需要修改的容器的名称 bash表示使用linux命令，如果是使用redis-cli的命令，则写redis-cli即可直接进入
```

### 数据卷

我们的docker容器可以看作是一个微型的操作系统，默认情况下，我们的文件储存目录都是储存在该操作系统中的，如果我们希望对其进行配置或者数据的存储，都是在这个操作系统中进行的，因此其存在很多的弊端:

* 操作很麻烦，因为镜像仅打包所需的依赖，因此诸如vim这样的不相关工具并未安装，操作文件会很麻烦
* 镜像和数据的耦合度过高，因为数据是储存在容器内的，因此，当我们希望对于容器进行更新时，其数据是被隔离的，如果我们删除了原容器，则数据也不复存在了
* 配置的不确定性，维护成本高。当我们拥有多个容器时，我们如果希望获得两个具有相同配置的容器，因为其配置都是在容器内进行的，而配置也是不存在日志的，因此，我们很难在一段时间后创建一个u上一个容器一模一样的容器

为了解决上述问题，我们可以使用数据卷来解决。所谓的数据卷也就是一个虚拟的路径，这是一个桥梁，连接了宿主机和容器，其将宿主机的某个具体路径与容器进行了映射，因此当容器需要使用某个路径的文件时可以通过挂载的数据卷来映射到宿主机的地址上，因此，我们可以实现配置和数据在宿主机上的持久化，这也就意味着我们可以随时创建两个不同版本的，拥有同样数据和配置的容器，仅需保证它们挂载了同一个数据卷即可

#### 操作数据卷常用命令

数据卷的指令是一个二级指令，其指令格式为:

```bash
docker volume [command]
```

**command的值**

|  命令   |               意义               |
| :-----: | :------------------------------: |
| create  |          创建一个volume          |
| inspect | 显示一个或者多个volume的详细信息 |
|   ls    |         列出所有的volume         |
|  prune  |        删除未使用的volume        |
|   rm    |   删除一个或者多个指定的volume   |

#### 挂载数据卷

```sh
docker run --name redis-server -d -v volumeName:/etc/... redis redis-server
#此时-v表示指定挂载的数据卷， volumeName表示的是数据卷的名称:etc/..表示所需要映射的容器中的路径
```

#### 关联目录

我们可以通过数据卷来管理数据和配置，此时，我们将控制权完全交由了docker，此时，我们要通过docker才能知道，数据的存储位置。

我们亦可以选择直接手动关联目录(宿主机目录:容器目录)。此时，我们可以自由决定文件目录的位置

### 创建镜像

Dockerfile是一个文本文件，其包含了一个个的指令，这些指令是用以说明使用什么操作来构建镜像，而每一个指令都会创建一层(layer)。因此每一个镜像都应当是一层一层的结果，这样也方便我们对于镜像进行更新

**Dockerfile的常用指令**

| Instruction | Description                                                  |
| :---------- | :----------------------------------------------------------- |
| FROM        | Specifies the base image to use for the build process[1](https://bing.com/search?q=list+most+used+instructions+in+dockerfile+with+table) |
| MAINTAINER  | Provides information about the person who created the Docker image[1](https://bing.com/search?q=list+most+used+instructions+in+dockerfile+with+table) |
| CMD         | Specifies what command to run within the container[1](https://bing.com/search?q=list+most+used+instructions+in+dockerfile+with+table) |
| RUN         | [Executes commands in a new layer on top of the current image](https://bing.com/search?q=list+most+used+instructions+in+dockerfile+with+table)[1](https://bing.com/search?q=list+most+used+instructions+in+dockerfile+with+table) |
| LABEL       | [Adds metadata to an image](https://bing.com/search?q=list+most+used+instructions+in+dockerfile+with+table)[1](https://bing.com/search?q=list+most+used+instructions+in+dockerfile+with+table) |
| EXPOSE      | [Informs Docker that the container listens on the specified network ports at runtime](https://bing.com/search?q=list+most+used+instructions+in+dockerfile+with+table)[1](https://bing.com/search?q=list+most+used+instructions+in+dockerfile+with+table) |
| ENV         | Sets environment variables[1](https://bing.com/search?q=list+most+used+instructions+in+dockerfile+with+table) |
| ADD         | Copies new files, directories or remote file URLs from `<src>` and adds them to the filesystem of the image at the path `<dest>`[1](https://bing.com/search?q=list+most+used+instructions+in+dockerfile+with+table) |
| COPY        | Copies a local file to a specified directory in the image    |
| ENTRYPOINT  | Set image start command when the container use the image     |

#### 示例Dockerfile：

```dockerfile
#设置基础镜像
FROM ubuntu:22.04

#配置环境变量，JDK的安装目录
ENV JAVA_DIR=/usr/local

#拷贝jdk和java项目的包
COPY ./jdk-20_linux-x64_bin.tar.gz $JAVA_DIR/
COPY ./docker-demo.jar /tmp/app.jar

#安装Java环境
RUN cd $JAVA_DIR \
&& tar -xf ./jdk-20_linux-x64_bin.tar.gz \
&& mv ./jdk-20 ./java20

# 配置环境变量
ENV JAVA_HOME=$JAVA_DIR/java20
ENV PATH=$PATH:$JAVA_HOME/bin
# 暴露端口
EXPOSE 4231
# 设置启动的入口命令
ENTRYPOINT java -jar /tmp/app.jar
```

#### 在Dockerfile所在目录下build:

```bash
docker build -t demo . #注意最后有一个.其表示当前目录(Dockerfile的目录)demo即为名称，此时未指定版本，因此未默认的laested;-t表示标识
```

我们注意到，我们前面构建一个镜像中配置环境的部分是固定的，而这些固定的配置在我们部署多个相同环境的项目时，其会导致多次重复的操作，因此为了避免这一重复的构建，我们可以直接使用一个构建完成的Java8环境的镜像:**java:8-alpine**，当我们使用了该镜像后，其将会默认的Java8环境配置完成我们便仅需设置以下配置:

```dockerfile
#拷贝Java项目
COPY ./docker-demo.jar /tmp/app.jar
# 暴露端口
EXPOSE 4231
# 设置启动的入口命令
ENTRYPOINT java -jar /tmp/app.jar
```

### DockerCompose

**注:需要安装DockerCompose插件:sudo apt-get install docker-compose-plugin**

在前面我们运行镜像时(**docker run**)，我们是指定了一个镜像进行运行，此时一两个镜像如此没有什么问题，但是一旦我们拥有数十上百个集群镜像要同时运行时，则该种方法便不可行了，为此我们可以使用DockerCompose的方法进行集群部署，而这种部署实际上也就是将单个镜像的部署指令进行了转换:

#### 转换前(指定镜像部署)

```shell
docker run \
 --name mysql \
 -e MYSQL_ROOT_PASSWORD=root \
 -v /tmp/mysql/my.cnf:/etc/mysql/conf.d/my.cnf \
 -v /tmp/mysql/data:/var/lib/mysql \
 -d \
 mysql:5.7.25
```

#### 转换后(指定镜像部署)

```yaml
version: "3.8"

services:
	mysql: #指定容器名称
		image: mysql:5.7.25 #指定镜像
		environment: #指定环境
			MYSQL_ROOT_PASSWORD=root
		volumes: #指定数据卷(/目录映射)
			- /tmp/mysql/my.cnf:/etc/mysql/conf.d/my.cnf
			- /tmp/mysql/data:/var/lib/mysql
```

#### 转换前(依赖于Dockerfile临时创建并部署)

```shell
docker build -t web:1.0 .
docker run --name web -p 8080:8080 -d web:1.0
```

#### 转换后(依赖于Dockerfile临时创建并部署)

```yaml
web:
	build: .
	prots:
		- 8080:8080
```

**启动命令**:

```shell
docker-compose up -d
```



**注：**

* DockerCompose文件应当命名为docker-compose.yml

* 当使用DockerCompose部署微服务时，微服务之间的访问均需要以服务名(Dockercompose中规定的服务名)进行访问:

  * ```yaml
    spring:
      application:
        name: gateway
      cloud:
        nacos:
          discovery:
            server-addr: nacos:8848 #localhost->nacos
    ```

* 在docker-compose文件中我么可以通过**$PWD**来获取当前的位置

### 搭建私有docker仓库

#### 无图形化界面

```shell
docker run -d \
	--restart=always \
	--name registry\
	-p 5000:5000 \ #registry默认暴露端口为5000
	-v registry-data:/var/lib/registry \
	registry
```

#### 有图形化界面(使用docker-compose搭建)

```yaml
version: '3.0'
services:
	registry:
		image: registry
		volumes:
			- ./registry-data:/var/lib/registry
	ui:
		image: joxit/docker-registry-ui:static
		ports:
			-8280:80
		environment:
			- REGISTRY_TITLE=MY PRIVATE REGISTRY
			- REGISTRY_URL=http://registry:5000 #registry默认端口为5000
		depends_on:
			- registry #UI依赖于docker仓库
```

### 使docker信任我们的仓库

默认情况下，docker是不信任没有https协议的仓库的(**如果拥有https协议则跳过**)，为了使其信任我们的仓库，我们需要修改daemon.json：

```shell
vim /etc/docker/daemon.json
```

**daemon.json:**

```jso
"insecure-registries":["仓库地址:端口号"]  
```

#### 从推送和拉取镜像

##### 推送镜像：

1. 推送镜像至私有仓库服务前首先需要进行tag操作，名称前缀为私有仓库的地址:端口/：

   ```shell
   docker tag nginx:latest 镜像仓库地址:端口/nginx:latest
   ```

2. 推送镜像

   ```shell
   docker push 镜像仓库地址:端口/nginx:latest
   ```

##### 拉取镜像:

```shell
docker pull 镜像仓库地址:端口/nginx:latest
```

## 消息队列

![""](.\消息队列.png)

我们的消息队列实际上就相当于是前端中的异步操作，当我们使用openfeign等进行请求的发送时，其调用的便是同步请求，这也就意味着，当我们的支付服务如果希望在用户支付成功后更新订单服务，仓储服务...则必须按需进行：支付服务成功->订单服务->订单服务响应->仓储服务->仓储服务响应...

这也就意味着，我们的服务越多，则响应时间越长，因此支付服务的花费时间会越来越长。而且在支付服务在等待订单服务响应时是占用资源但是没有任何操作的状态，所以这还造成了资源的浪费。除此之外，如果其中一个服务崩溃，则会间接导致支付服务，而后导致整个微服务的崩溃。

因此为了解决这一问题，我们引入了消息队列的概念，消息队列是一个以事件驱动的模型。我们在支付服务和其成功后所调用的服务间添加一个Broker,它会在支付服务成功后，便将这条支付成功的消息**“广播”**到所有的订阅了该事件(**支付成功的事件**)的服务上，以进行操作。而后支付服务便不用再理会其他服务了，这便**减少了响应时间**，**降低了系统的损耗**，而且还可以**实现服务的解耦**，因为我无需再支付服务中修改代码了，如果有新的需求被添加到了支付成功后。我们仅需该服务订阅该事件即可，也实现了**故障隔离**，甚至可以**实现流量削峰**

### docker启动rabbitmq

```shell
docker run \
-e RABBITMQ_DEFAULT_USER=rongxin \ #设置管理用户名
-e RABBITMQ_DEFAULT_PASS=904912 \  #设置管理密码
--name mq \						   
--hostname mqAlpha \               #设置集群部署的名称
-p 15672:15672 \				   #设置管理界面的端口
-p 5672:5672 \					   #消息通信的端口
-d rabbitmq:3-ma'na
```

### 简单队列模型

![""](.\简单队列.jpg)

在简单队列模型中，我们的发布者将消息上传至队列，而后消费者便将队列中的消息提取，此时需要注意的是，队列中的消息一旦被取出便会被销毁，这也就意味着，一条消息仅能被一个消费者消费

### 消息队列执行流程

#### 发送消息流程

1. 通过ConnectionFactory建立connection
2. 通过connection来创建一个channel
3. 使用channel声明一个队列
4. 通过channel向通道推送消息

#### 接收消息流程

1. 通过ConnectionFactory建立connection
2. 通过connection来创建一个channel
3. 使用channel声明队列
4. 定义一个consumer的消费行为handleDelivery
5. 利用channel将消费者和队列绑定

### 消息队列使用(简单队列)

#### 发送消息

```java
 		/*创建连接工厂*/
        ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("139.224.198.54");
        connectionFactory.setPort(5672);
		/*设置虚拟主机*/
        connectionFactory.setVirtualHost("/");
        connectionFactory.setUsername("rongxin");
        connectionFactory.setPassword("904912");
        /*创建连接*/
        Connection connection = connectionFactory.newConnection();
        /*创建连接通道*/
        Channel channel = connection.createChannel();
        String queueName="simple.queue";
        channel.queueDeclare(queueName,false,false,false,null);

        String message="hello, rabbitmq!";
        channel.basicPublish("",queueName,null,message.getBytes(StandardCharsets.UTF_8));
        System.out.println("Message sent");

        channel.close();
        connection.close();
```

#### 接收消息

```java
		ConnectionFactory connectionFactory = new ConnectionFactory();
        connectionFactory.setHost("139.224.198.54");
        connectionFactory.setPort(5672);
        connectionFactory.setVirtualHost("/");
        connectionFactory.setUsername("rongxin");
        connectionFactory.setPassword("904912");
        Connection connection = connectionFactory.newConnection();
        Channel channel = connection.createChannel();
        String queueName="simple.queue";
        channel.queueDeclare(queueName,false,false,false,null);

        channel.basicConsume(queueName,true,new DefaultConsumer(channel){
            @Override
            public void handleDelivery(String consumerTag, Envelope envelope, AMQP.BasicProperties properties, byte[] body) 			throws IOException {
                String message = new String(body);
                System.out.println("Received message:"+message);
            }
        });
        System.out.println("Waiting for new message!");

        channel.close();
        connection.close();
```

### AMQP协议

前面我们使用RabbitMQ的API实现了消息的发送和接受，很显然这样写会很麻烦，为此我们的Spring规定了一个AMQP协议(Advanced Message Queue Protocol)，而RabbitMQ便实现了该协议，因此我们可以在SpringBoot项目中直接引入AMQP的依赖即可直接使用RabbitMQ的Template:

```xml 
	   <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-amqp</artifactId>
            <version>2.7.2</version>
        </dependency>
```

#### 配置文件

```yaml
spring:
  rabbitmq:
    host: 139.224.198.54
    virtual-host: /
    port: 5672
    username: rongxin
    password: 904912

```

#### 发送消息(简单队列)

```java
 	@Autowired
    private RabbitTemplate template;
    @Test
    public void simple(){
        template.convertAndSend("simple.queue","Hello World!"); /*(队列名称，消息)*/
    }
```

#### 接受消息(简单队列)

```java
@Component
public class SpringRabbitListener {
    @RabbitListener(queues = "simple.queue")
    public void listenSimpleMessage(String message) throws Exception{
        System.out.println("Message received:"+message);
    }
}
```

#### 工作队列

实际上工作队列并非是一个新的队列，其依然是一个简单队列，只是，原本的简单队列是个发布者和一个消费者，此时，该消费者将会接受所有由发布者发送的消息。而该种处理方案将可能会导致消息的堆积(**当一个消费者的处理能力有限时，如果它每秒仅能处理50条消息，但是生产的发布者每秒产生了60甚至100条消息，则多余的消息便会产生堆积**)，而这种堆积过多将会导致消息的丢弃。为此我们采用工作队列的方式，也就是一个发布者对多个消费者，那么此时每秒钟，两个消费者便可以处理的消息的数量便是两者能力之和了，此时便避免了消息的堆积。

##### 发送消息(工作队列)

```java
 	@Test
    public void work() throws InterruptedException {
        for (int i = 1; i <= 50; i++) {
            template.convertAndSend("simple.queue","Message NO."+i);
            System.out.println("Sent--->"+"Message NO."+i);
            Thread.sleep(20);
        }
    }
```

##### 接受消息(工作队列)

```java
 	@RabbitListener(queues = "simple.queue")
    public void listenWorkAlpha(String message) throws Exception {
        System.out.println("Message received:" + message+ new Date());
        Thread.sleep(20);
    }

    @RabbitListener(queues = "simple.queue")
    public void listenWorkBeta(String message) throws Exception {
        System.err.println("Message received:" + message+new Date());
        Thread.sleep(200);
    }
```

但是需要注意的一点是，默认情况下，rabbitmq将会采取消息预取的手段进行操作，也就是说监听者并不在意是否已经处理完成当前的消息便会从队列中来预取消息(默认是无限大)，而后再依次处理，因此这也就会造成一个问题:性能不同的两台机器预取的消息数是一致的，而为了解决这个问题，我们可以手动的设置预取最大值，那么此时监听者仅在处理完成了所有消息后才能继续预取最大数量的消息。我们可以这样配置预取的最大值:

```yaml
spring:
  rabbitmq:
    host: 139.224.198.54
    virtual-host: /
    port: 5672
    username: rongxin
    password: 904912
    listener:		
      simple:
        prefetch: 1 #设置最大预取值为1
```

#### 发布-订阅模型

![""](D:\WorkStation\Notes\发布者订阅模式.jpg)

前面的消息队列模型无法满足我们将同一个消息分发给多个消费者的要求，于是我们可以采用发布-订阅模型，该模型通过一个交换机将一条消息分发给多个队列而后再分发给多个消费者。

##### fanout交换机

该交换机会将接受到的消息分给每一个**与其绑定的queue**

**消费者**

```java
/*配置类*/
@Configuration
public class BindingConfig {
    /*声明交换机*/
    @Bean
    public FanoutExchange fanoutExchange(){
        return new FanoutExchange("rongxin.fanout");
    }
    /*声明队列Alpha*/
    @Bean
    public Queue fanoutQueueAlpha(){
        return new Queue("fanout.queueAlpha");
    }
    /*声明队列Alpha与rongxin.fanout交换机的绑定*/
    @Bean
    public Binding bindingQueueAlpha(Queue fanoutQueueAlpha,FanoutExchange fanoutExchange){
        return BindingBuilder.bind(fanoutQueueAlpha).to(fanoutExchange);
    }
    /*声明队列Beta*/
    @Bean
    public Queue fanoutQueueBeta(){
        return new Queue("fanout.queueAlpha");
    }
    /*声明队列Beta与rongxin.fanout交换机的绑定*/
    @Bean
    public Binding bindingQueueBeta(Queue fanoutQueueBeta,FanoutExchange fanoutExchange){
        return BindingBuilder.bind(fanoutQueueBeta).to(fanoutExchange);
    }
}

/*接收监听器与前面的用法一致*/
```

**发布者:**

```java
...
template.convertAndSend(exchangeName,"",message); /*(交换机名，routingKey,消息)*/
...
```

##### direct交换机

我们的fanout交换机会将消息推送给所有与之绑定的队列中，而direct则可以通过routingKey来指定将消息推送至指定的消息队列之中

**消费者**

```java
/*前面我们通过创建bean的方法来声明队列，交换机和交换机与队列的绑定关系很麻烦，因此我们可以使用注解来解决*/
@RabbitListener(bindings=@QueueBinding( /*声明队列与交换机的绑定关系*/
	value=@Queue(name="direct.queue"), /*声明队列*/
    exchange=@Exchange(name="rongxin.direct",type="direct"),/*声明交换机，默认类型为direct*/
    key={"red","yellow"} /*声明routingKey*/
))
public void listenerDirect(String msg){
    ...
}
```

**发布者**

```java
...
template.convertAndSend(exchangeName,"red",message); /*(交换机名，routingKey,消息)*/
...
```

##### topic交换机

我们的direct是通过多个不同的routingKey来实现分类的。而topic则是使用一个routingKey便可以实现了，只是这个单个的routingKey可以包含多个单词，每个单词由**"."**进行分隔，除此之外其可以使用通配符: **<u>#表示0或多个单词，*代表单个单词</u>**。

```shell
us.news     #美国新闻
us.weather  #美国天气
uk.news		#英国新闻
uk.weather	#英国天气
*.weather	#所有国家的天气
us.#		#美国的所有话题
```

其发布和订阅的用法与前者的direct一致

#### 消息转换器

我们的消息队列是存在一个消息转换器的对象，该对象默认是JDK中的对象。而我们同时也注意到我们当我们使用rabbitTemplate时，其是可以支持发送Object对象的，但是默认情况下，我们的消息队列是采用的JDK的默认的转换器，因此在对于消息的序列化和反序列化上易读性是很差的，但传递的消息是复杂类型时。为此，我们可以通过修改其MessageConverter的bean来实现，如将默认的转换器转为json格式

```java
@Bean
public MessageConverter jsonMessageConverter(){
    return new Jackson2JsonMessageConverter();
}
```

## ElasticSearch(ES技术)

在传统的搜索中，如果说我们希望通过每个关键字来实现对于数据库中某个字段的查询，那么我们所使用的技术是**正向索引**，也就是说首先我们会查询数据库中的所有数据(**逐行查询**)，而将每一条中所需查询的字段提取出来，在其中检索是否包含所查询的关键字(**模糊查询，而模糊查询会导致索引失效**)，如果存在则加入结果集，否则忽略。很显然，这样查询的效率很低。

为了解决上述问题，我们的ElasticSearch技术采用了**反向索引**的技术,其在存储时会将包含所将被视为关键字的字段进行语义拆分，将其拆为多个词语，也就是说，其会将该字段的内容中所包含的**词语(单词)**单独提取出来，而后将其单独存于一个表中，将其与ID进行绑定，此时随着数据的增加，该关键字所关联的ID会越来越多，但是需要注意的是词语部分永远是唯一的，只是每次与该词语相关的数据被添加后，其所对应的ID列都会被更新:

**商品表**

| id   | name     |
| ---- | -------- |
| 1    | 苹果手机 |
| 2    | 苹果手表 |
| 3    | 苹果耳机 |
| ...  | ...      |

此时其会创建一个表格:

| keys | ids     |
| ---- | ------- |
| 苹果 | 1，2，3 |
| 手机 | 1       |
| 手表 | 2       |
| 耳机 | 3       |
| ...  | ...     |

此时如果我们的关键字为"手表"，则我们可以快速地得到商品表的id为2。

此时商品表中的每一条记录即为我们的文档(document),而我们的keys即为我们每个文档的词条，我们可以通过词条判断某条记录是否包含所需的词条，以实现关键字的查询。

在传统的查询中，我们是通过关键字与每一条记录进行比较以进行的查询，也就是模糊查询**某一条记录是否包含该关键字**，这种方式我们可以称之为**正向索引**。而使用上述方法，我们则是直接通过关键字来查询，即**通过查询与关键字匹配的记录中是否包含了某一条记录的id**，因此我们称之为**倒排索引**。

### ElasticSearch与MySQL的

| MySQL  | ElasticSearch |                             说明                             |
| :----: | :-----------: | :----------------------------------------------------------: |
| Table  |     Index     | 索引即为文档的集合，具有相同结构的索引可以被视为同一个索引，因此索引类似于MySql的表 |
|  Row   |   Document    |   文档实际上即为一个表中的每一条记录，一条记录即为一个文档   |
| Column |     Field     | ElasticSearch中，文档是以JSON形式序列化的，因此每一个文档都是一个JSON对象，而这个对象内的属性(字段名)便相当于我们MySql中的各个单独的字段 |
| Schema |    Mapping    | 所谓的Schema即为定义我们MySql表的一个模板，而Mapping也就是相当于我们的Schema |
|  SQL   |      DSL      | DSL是ElasticSearch提供的JSON风格的请求语句，可以使用其来实现CRUD操作 |

**Docker创建网络:**

```shell
docker network create es-net #创建一个es-net网络
```

启动elasticsearch：

```shell
docker run -d \
	--name es \                       				#定义容器名  
	-e "ES_JAVA_OPTS=-Xms512m -Xmx512m"\   			#配置Java最大和最小内存，默认为1024
	-e "discovery.type=single-node" \				#单点运行
	-v es-data:/usr/share/elasticsearch/data \		#文件映射
	-v es-plugins:/usr/share/elasticsearch/plugins	#文件映射
	--privileged \									
	--network es-net \								#加入到es网络
	-p 9200:9200 \									#我们访问elasticsearch的端口
	-p 9300:9300 \									#节点间互联的端口
	elasticsearch:8.7.0
```

### 设置ES的密码(该功能在以后将会被废除)

```shell
# 进入容器
docker exec -it elasticsearch /bin/bash
# 设置密码-随机生成密码
# elasticsearch-setup-passwords auto
# 设置密码-手动设置密码，如设置为123456，此时用户名即为elastic,密码便为123456
elasticsearch-setup-passwords interactive
# 访问
curl 127.0.0.1:9200 -u elastic:123456
```

#### elastic参数含义

|                参数                 |                  含义                  |
| :---------------------------------: | :------------------------------------: |
| -e "cluster.name=es-docker-cluster" |      设置集群的es-docker-cluster       |
|       -e "http. host=0.0.0.0"       | 监听的地址，此时设置为任意地址均可访问 |
|             -privileged             |            授予逻辑卷访问权            |
|          --network es-net           |          加入一个es-net的网络          |

