---
layout: post
title:  "Springboot项目搭建"
categories: Java
tags:  Java
author: FTX
---

* content
{:toc}

### 1 spring-boot介绍

 Spring Boot是由spring的Pivotal团队提供的全新框架，其设计目的是用来简化新Spring应用的初始搭建以及开发过程。该框架使用了特定的方式来进行配置，从而使开发人员不再需要定义样板化的配置。用我的话来理解，就是spring boot其实不是什么新的框架，它默认配置了很多框架的使用方式，就像maven整合了所有的jar包，spring boot整合了所有的框架。





其实就是简单、快速、方便！平时如果我们需要搭建一个spring web项目的时候需要怎么做呢？

1）配置web.xml，加载spring和spring mvc

2）配置数据库连接、配置spring事务

3）配置加载配置文件的读取，开启注解

4）配置日志文件

…

配置完成之后部署tomcat 调试

…

现在非常流行微服务，如果我这个项目仅仅只是需要发送一个邮件，如果我的项目仅仅是生产一个积分；我都需要这样折腾一遍!

 

但是如果使用spring boot呢？

很简单，我仅仅只需要非常少的几个配置就可以迅速方便的搭建起来一套web项目或者是构建一个微服务

 

Spring Boot 具有以下特点：

1 内嵌 Servlet 容器

   Spring Boot 使用嵌入式的 Servlet 容器（例如 Tomcat、Jetty 或者 Undertow 等），应用无需打成 WAR 包 。

2 提供 starter 简化 Maven 配置

   Spring Boot 提供了一系列的“starter”项目对象模型（POMS）来简化 Maven 配置。

3 提供了大量的自动配置

   Spring Boot 提供了大量的默认自动配置，来简化项目的开发，开发人员也通过配置文件修改默认配置。

4 自带应用监控

   Spring Boot 可以对正在运行的项目提供监控。

5 无代码生成和 xml 配置

   Spring Boot 不需要任何 xml 配置即可实现 Spring 的所有配置

### 2 spring-boot 项目创建

 

搭建一个springboot 项目步骤如下：

1    新建maven项目

2 在pom.xml 中添加下面的依赖：

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.3.2.RELEASE</version>

</parent>
<dependencies>
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
</dependency>
      <!--spring boot web集成依赖  -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>

<!--data注解所需依赖-->
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
</dependency>

    <!--springboot的junitt测试依赖-->
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-test</artifactId>
        <scope>test</scope>
        <exclusions>
            <exclusion>
                <groupId>org.junit.vintage</groupId>
                <artifactId>junit-vintage-engine</artifactId>
            </exclusion>
        </exclusions>
    </dependency>

  
</dependencies>
```



**spring-boot-starter作用**

可以理解为这是一个启动器，这个启动器会自动依赖其他组件，一站式获取你需要的与spring有关的组件。

例如：如果你想开发一个web应用程序，只需要你的项目包含 `spring-boot-starter-web` 依赖项就可以了。

而不是像spring那样，你需要在pom单独引入spring-core、spring-beans、spring-aop 等等，最最最重要的是，**可以省去了版本冲突的问题。**还可以通过`spring-boot-starter-parent` 统一控制版本。

3  添加启动类



```
@SpringBootApplication
public class MyApp {

    public static void main(String[] args) {
        SpringApplication.run(MyApp.class,args);
    }
}
```



**springboot单元测试**

```java
@RunWith(SpringRunner.class)//spring测试的运行器注解
@SpringBootTest(classes = MyApp.class) //springboot测试类注解
public class JavaConfigTest {

    @Autowired
    TestService service;

    @Test
    public void test(){
        service.add();
    }

}
```



### 3 spring-boot application配置文件

Spring Boot 提供了大量的自动配置，极大地简化了spring 应用的开发过程，当用户创建了一个 Spring Boot 项目后，即使不进行任何配置，该项目也能顺利的运行起来。当然，用户也可以根据自身的需要使用配置文件修改 Spring Boot 的默认设置。

SpringBoot 默认使用以下 2 种全局的配置文件，其文件名是固定的。

- application.properties
- application.yml


其中，application.yml 是一种使用 YAML 语言编写的文件，它与 application.properties 一样，可以在 Spring Boot 启动时被自动读取，修改 Spring Boot 自动配置的默认值。

.properties 文件我们都熟知了，这里主要介绍下.yml 文件的语法以及使用



### 4 整合mybatis-puls

Mybatis-Plus（简称MP）是一个 Mybatis 的增强工具，在 Mybatis 的基础上只做增强不做改变，为简化开发、提高效率而生。

 

**谁适合阅读本教程？**

本教程适合所有学习过 mybatis（能熟练使用 mybatis，否则体会不到它的方便和高效），但是需要对 mybatis-plus 了解和应用的同学。

特性

·    **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑

·    **损耗小**：启动即会自动注入基本 CRUD，性能基本无损耗，直接面向对象操作

·    **强大的 CRUD 操作**：内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求

·    **支持主键自动生成**：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题

·    **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用

·    **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询

·    **分页插件支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库



1 引入依赖

```xml
<!--mybatis-plus依赖-->
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.3.2</version>
</dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>8.0.27</version>
        </dependency>

```



2  配置数据源和sql,sql打印, 映射文件配置



mysql.8的配置

```properties
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.url=jdbc:mysql://localhost:3306/shiyoui?useUnicode=true&characterEncoding=utf-8&useSSL=false&serverTimezone=GMT%2B8 
spring.datasource.username=root
spring.datasource.password=1234
#sql 打印
mybatis-plus.configuration.log-impl=org.apache.ibatis.logging.stdout.StdOutImpl
#mybatis-plus的 sql映射文件配置
mybatis-plus.mapper-locations=classpath:mapper/*.xml
```

### 5 mybatis-plus 基础学习

课堂演示 数据表的表结构

 

DROP TABLE IF EXISTS `user_info`;

CREATE TABLE `user_info` (

 `user_id` int(11) NOT NULL auto_increment,

 `user_name` varchar(40) default NULL,

 `user_pwd` varchar(40) default NULL,

 `user_detail_id` int(11) default NULL,

 PRIMARY KEY (`user_id`)

) ENGINE=InnoDB DEFAULT CHARSET=utf8;



### 定义pojo 类

```java
@Data
@TableName("user_info")
public class UserInfo extends MyPage implements Serializable {

    private static final long serialVersionUID=1L;

    @TableId(value = "user_id", type = IdType.AUTO)
    private Integer userId;

    private String userName;

    private String userPwd;
    
    private String userDetailId;


}
```

 1 @TableName 注解用来将指定的数据库表和 JavaBean 进行映射。

 2  @TableId 注解的用法，该注解用于将某个成员变量指定为数据表主键.

​       value指定数据表主键字段名称，不是必填的，默认为空字符串

​      type 指定数据表主键类型，如：ID自 增、UUID等。该属性的值是一个 IdType 枚举类型，默认为             IdType.NONE。

​      AUTO     数据库 ID 自增。如果我们创建数据库表时使用了 AUTO_INCREMENT 修饰主键

3  @TableField 字段注解，该注解用于标识非主键的字段。将数据库列与 JavaBean 中的属性进行映射，例如：

​     exist 是否为数据库表字段，默认为 true。





### 定义BaseMapper接口

例如：

```java
public interface UserInfoMapper extends BaseMapper<UserInfo> {
}

 
```

你会发现，我们在 UserInfoMapper中没有声明任何方法。但是我们在 UserInfoMapper测试类中却使用了 insert()、selectById()、updateById() 和 deleteById() 方法。而这些方法均来至 BaseMapper 接口



### 创建单元测试类

```javascript
 **/
@RunWith(SpringRunner.class)//spring测试的运行器注解
@SpringBootTest(classes = MyApp.class) //springboot测试类注解
public class UserInfoDaoTest {
  
}

```

### Insert保存数据

```java
//insert新增
@Test
public void add(){
    UserInfo user = new UserInfo();
    user.setUserName("lisi");
    user.setUserPwd("123");
    int num = userInfoMapper.insert(user);
    System.out.println("num="+num);
}
```

### Update修改数据

```java
//根据id修改
@Test
public void update(){
    UserInfo user = new UserInfo();
    //定义修改的值
    user.setUserName("admin");
    user.setUserPwd("123");
    //定义修改条件的id
    user.setUserId(1);
    int num = userInfoMapper.updateById(user);
    System.out.println("num="+num);

}
//根据自定义条件修改
@Test
public void update02(){
    //创建修改对象
    UpdateWrapper<UserInfo> wrapper = new UpdateWrapper<>();
    //定义修改条件
    wrapper.ge("user_id",1);
    //创建要修改的值
    UserInfo user = new UserInfo();
    user.setUserPwd("123");
    int num = userInfoMapper.update(user,wrapper);
    System.out.println("num="+num);

}
```

### Delete删除数据

 

```java
//根据id删除
@Test
public void del01(){
    int rs = userInfoMapper.deleteById(6);
}
//根据 Wrapper 查询对象删除数据
@Test
public void del02(){
    //定义查询对象
    QueryWrapper<UserInfo> w = new QueryWrapper<>();
    w.ge("user_id",8);
    int rs = userInfoMapper.delete(w);
}
//批量删除数据
@Test
public void del03(){
    //定义要删除的id集合
    List<Integer> ids = Arrays.asList(3,4,6);
    int rs = userInfoMapper.deleteBatchIds(ids);
    System.out.println("rs="+rs);

}

```



### Select查询数据

#### 根据id查询

```java
//根据id查询
@Test
public void search01(){
    UserInfo u = userInfoMapper.selectById(1);
    System.out.println(u.toString());
}
 
```

#### Wrapper 查询且只返回一条记录

```java
//根据 Wrapper 查询，且只返回一条记录
@Test
public void search02(){
    QueryWrapper<UserInfo> w = new QueryWrapper<>();
    w.eq("user_id",1);
    UserInfo u = userInfoMapper.selectOne(w);
    System.out.println(u.toString());
}
```

#### Wrapper 查询且只返回多条记录

```java
//根据 Wrapper 查询，返回多条记录
@Test
public void search03(){
    QueryWrapper<UserInfo> w = new QueryWrapper<>();
    w.ge("user_id",1);
    List<UserInfo> listu = userInfoMapper.selectList(w);
    System.out.println(listu.size());
}
```

#### 批量查询

```
//批量查询
@Test
public void search04(){
    //定义要查询的id集合
    List<Integer> ids = Arrays.asList(3,4,6);
    List<UserInfo> listu = userInfoMapper.selectBatchIds(ids);
    System.out.println(listu.size());
}
```

#### 动态查询

```java
//动态查询
@Test
public void search05(){
    //定义要传入的参数
    Integer queryId=1;
    String queryName=null;

    QueryWrapper<UserInfo> w = new QueryWrapper<>();
    // 第一个参数为是否执行条件，为true则执行该条件
    // 下面实现根据 user_id 和 user_name 动态查询
    w.eq(queryId!=null,"user_id",queryId);
    w.eq(StringUtils.isEmpty(queryName)&&queryName!=null,"user_name",queryName);
    //查询结果
    List<UserInfo> listu = userInfoMapper.selectList(w);
    System.out.println(listu.size());
}
```

#### 分页查询

分页插件配置

```java
/**
 * @Description 分页插件配置
 * @Autor 伍军
 * @Date 2021/8/6 13:50
 * @Version 1.0
 **/
@Configuration
public class MybatisPlusPaginationConfig {

    @Bean
    public PaginationInterceptor paginationInterceptor() {
        return new PaginationInterceptor();
    }

}
```

```java
//分页查询
@Test
public void search06(){
    //定义分页对象（1表示第一页；2表示每页大小为2）
    Page<UserInfo> page = new Page<>(1,2);
    //定义查询条件
    QueryWrapper<UserInfo> w = new QueryWrapper<>();
    //查询分页结果对象
    Page<UserInfo> pageList = userInfoMapper.selectPage(page,w);
    System.out.println("每页集合" + pageList.getRecords());
    System.out.println("总页数: " + pageList.getPages());
    System.out.println("总条数: " + pageList.getTotal());
}
```

### 排序查询