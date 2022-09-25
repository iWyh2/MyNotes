# MyBatisPlus



# 1.了解MyBatisPlus

## 1.1 介绍

* MyBatis的增强工具，只做增强，不做改变，简化开发，提高效率
* 官网：https://mp.baomidou.com



## 1.2 特性

* **无侵入**：只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
* **损耗小**：启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
* **强大的 CRUD 操作**：**内置通用 Mapper**、**通用 Service**，仅仅通过少量配置即可实现单表大部分 CRUD 操作，更有强大的条件构造器，满足各类使用需求
* **支持 Lambda 形式调用**：通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
* **支持主键自动生成**：支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解决主键问题
* **支持 ActiveRecord 模式**：支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操作
* **支持自定义全局通用操作**：支持全局通用方法注入（ Write once, use anywhere ）
* **内置代码生成器**：采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码，支持模板引擎，更有超多自定义配置等您来使用
* **内置分页插件**：基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通 List 查询
* **分页插件支持多种数据库**：支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、SQLServer 等多种数据库
* **内置性能分析插件**：可输出 SQL 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
* **内置全局拦截插件**：提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作



## 1.3 架构

![framework](https://baomidou.com/img/mybatis-plus-framework.jpg)



## 1.4 作者介绍

* 苞米豆组织，国人组织，挺牛的





# 2.整合MyBatisPlus

* MyBatis整合MP有三种方式：
  1. MyBatis+MP
  2. Spring+MyBatis+MP
  3. SpringBoot+MyBatis+MP

## 2.1 快速开始

1. 创建数据库以及表

   ```mysql
   #----------------------------------------2022/9/23 MP课程创建
   drop database if exists mp;
   create database mp;
   
   use mp;
   create table tb_user (
       id bigint(20) NOT NULL AUTO_INCREMENT COMMENT '主键ID',
       user_name varchar(20) NOT NULL COMMENT '用户名',
       password varchar(20) NOT NULL COMMENT '密码',
       name varchar(30) DEFAULT NULL COMMENT '姓名',
       age int(11) DEFAULT NULL COMMENT '年龄',
       email varchar(50) DEFAULT NULL COMMENT '邮箱',
       PRIMARY KEY (id)
   )ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET = utf8;
   
   INSERT INTO tb_user (id, user_name, password, name, age, email) VALUES
   (1, 'TEST1','123456','Jone', 18, 'test1@wyh.com'),
   (2, 'TEST2','123456','Jack', 20, 'test2@wyh.com'),
   (3, 'TEST3','123456','Tom', 28, 'test3@wyh.com'),
   (4, 'TEST4','123456','Sandy', 21, 'test4@wyh.com'),
   (5, 'TEST5','123456','Billie', 24, 'test5@wyh.com');
   ```

2. 创建MP工程

   ```xml
       <dependencies>
   <!--        MyBatis+MP-->
           <dependency>
               <groupId>com.baomidou</groupId>
               <artifactId>mybatis-plus</artifactId>
               <version>3.5.2</version>
           </dependency>
           <dependency>
               <groupId>mysql</groupId>
               <artifactId>mysql-connector-java</artifactId>
               <version>8.0.30</version>
           </dependency>
           <dependency>
               <groupId>com.alibaba</groupId>
               <artifactId>druid</artifactId>
               <version>1.2.11</version>
           </dependency>
   <!--        简化Bean代码-->
           <dependency>
               <groupId>org.projectlombok</groupId>
               <artifactId>lombok</artifactId>
               <version>1.18.24</version>
           </dependency>
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
               <version>4.13.2</version>
               <scope>test</scope>
           </dependency>
           <dependency>
               <groupId>org.slf4j</groupId>
               <artifactId>slf4j-log4j12</artifactId>
               <version>2.0.1</version>
           </dependency>
       </dependencies>
   ```



## 2.2 MyBatis+MP

### 2.2.1 Mybatis完成查询

* 在MP工程中创建子模块MP1，自动继承MP

* 在MP1的resource下添加log4j.properties文件

  ```properties
  log4j.rootLogger=DEBUG,Al
  
  log4j.appender.Al=org.apache.log4j.ConsoleAppender
  log4j.appender.Al.layout=org.apache.log4j.PatternLayout
  log4j.appender.Al.layout.ConversionPattern=[%t] [%c]-[%p] %m%n
  ```

* MP1的resource下添加mybatis-config..xml文件

* 添加UserMapper.xml文件

* 添加User实体类，用lombok直接设置

  ```java
  @Data
  @NoArgsConstructor
  @AllArgsConstructor
  public class User {
      private Long id;
      private String userName;
      private String password;
      private String name;
      private Integer age;
      private String email;
  }
  ```

* 添加UserMapper接口类，创建findAll方法

* 添加测试类测试这个接口，添加SqlSessionFactory，添加SqlSession，获取mapper代理对象

  ```java
  		SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder()
                                                  .build(Resources.getResourceAsStream("mybatis-config.xml"));
          try (SqlSession sqlSession = sqlSessionFactory.openSession(true)) {
              UserMapper mapper = sqlSession.getMapper(UserMapper.class);
              List<User> users = mapper.findAll();
              for (User user : users) {
                  System.out.println(user);
              }
          }
  ```

* 注：表列名与实体类属性名不一致设置UserMapper.xml的resultMap属性

  ```xml
  <resultMap id="UserResult" type="com.wyh.domain.User">
       <result column="user_name" property="userName"/>
  </resultMap>
  ```

### 2.2.2 MyBatis+MP完成查询





# 3.通用CRUD



# 4.MyBatisPlus的配置



# 5.条件构造器

