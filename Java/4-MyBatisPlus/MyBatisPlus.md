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

2. 创建MP总工程，pom文件添加好所有所需坐标

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

​		2.2~2.4的所有工程都是这个总工程的子模块，继承了这个pom文件，maven的继承



## 2.2 Mybatis

* 在MP工程中创建子模块MP1，自动继承MP

* 在MP1的resource下添加log4j.properties文件

  ```properties
  log4j.rootLogger=DEBUG,Al
  
  log4j.appender.Al=org.apache.log4j.ConsoleAppender
  log4j.appender.Al.layout=org.apache.log4j.PatternLayout
  log4j.appender.Al.layout.ConversionPattern=[%t] [%c]-[%p] %m%n
  ```

* MP1的resource下添加mybatis-config.xml文件

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



## 2.3 MyBatis+MP

* 基础与Mybatis工程一样

* UserMapper接口继承BaseMapper类

  ```java
  public interface UserMapper extends BaseMapper<User> {}
  ```

* 在获取mapper代理对象时，SqlSessionFactoryBuilder换成**MybatisSqlSessionFactoryBuilder**，后续不变

  ```java
  SqlSessionFactory sqlSessionFactory = new MybatisSqlSessionFactoryBuilder()
                                                  .build(Resources.getResourceAsStream("mybatis-config.xml"));
  ```

* 同时，调用方法时，调用的是BaseMapper里面已经有的方法，如查询所有，SelectList

  ```java
  List<User> users = userMapper.selectList(null);
  ```

* 报错：

  * 表找不到：就是实体类名和表名不一致，需要在实体类上加上注解

    ```java
    @TableName("tb_user")
    ```

  * Create breakPoint，MyBatisPlus版本太高，降低版本解决问题



## 2.4 Spring+Mybatis+MP(配置文件版)

* 首先创建jdbc.properties文件，配置数据源druid的配置信息

  ```properties
  jdbc.driver=com.mysql.cj.jdbc.Driver
  jdbc.url=jdbc:mysql://localhost:3306/mp?useServerPrepStmts=true
  jdbc.username=root
  jdbc.password=*
  ```

* 创建Spring的核心配置文件application.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="
         http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd
         http://www.springframework.org/schema/context
         http://www.springframework.org/schema/context/spring-context.xsd">
  <!--    增加命名空间：context-->
  
      <!--    增加properties文件的扫描器，为读取数据源的配置信息-->
      <context:property-placeholder location="jdbc.properties"/>
  
      <!--    定义数据源:druid 替换默认的数据源-->
      <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource" destroy-method="close">
          <property name="driverClassName" value="${jdbc.driver}"/>
          <property name="url" value="${jdbc.url}"/>
          <property name="username" value="${jdbc.username}"/>
          <property name="password" value="${jdbc.password}"/>
          <property name="maxActive" value="10"/>
          <property name="minIdle" value="5"/>
      </bean>
  
      <!--    定义MP的MybatisSqlSessionFactory的bean-->
      <bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
          <!--        给这个bean注入依赖属性：数据源-->
          <property name="dataSource" ref="dataSource"/>
      </bean>
  
      <!--    添加扫描mapper接口的bean，为mybatis原生的扫描器-->
      <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
          <!--        指定扫描的包是哪一个-->
          <property name="basePackage" value="com.wyh.mapper"/>
      </bean>
  </beans>
  ```

* 创建User实体类与UserMapper接口，注意UserMapper接口继承BaseMapper接口

* 创建测试类测试MP功能即可，注意测试类所需Spring整合JUnit的注解

  ```java
  @RunWith(SpringJUnit4ClassRunner.class)
  @ContextConfiguration(locations = "classpath:application.xml")
  ```



## 2.5 SpringBoot+MyBatis+MP

* 创建SpringBoot工程，IDEA的Spring initializer

* 勾选所需的起步依赖：Mybatis框架 MySQL连接

* 添加额外依赖

  ```xml
  		<!--SpringBoot 的起步依赖-->
  		<dependency>
              <groupId>com.baomidou</groupId>
              <artifactId>mybatis-plus-boot-starter</artifactId>
              <version>3.5.2</version>
          </dependency>
  		<!--数据源-->
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
              <groupId>org.slf4j</groupId>
              <artifactId>slf4j-log4j12</artifactId>
              <version>2.0.1</version>
          </dependency>
  ```

* 添加log4j.properties文件、添加SpringBoot核心配置文件application.properties文件，内置数据源配置信息

  ```properties
  spring.application.name=wyh-mp-springboot
  
  spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
  spring.datasource.url=jdbc:mysql://localhost:3306/mp?userServerPrepStmts=true
  spring.datasource.username=root
  spring.datasource.password=020920
  ```

* 添加实体类User和UserMapper接口，接口继承BaseMapper，接口上方加上Mybatis提供的注解**@Mapper**，SpringBoot扫描到这个注解，就会为其创建mapper代理对象的bean

  ```java
  @Mapper
  public interface UserMapper extends BaseMapper<User> {
  }
  ```

* 添加SpringBoot启动类（自动创建的）

* 测试接口功能





# 3.通用CRUD

## 3.1 插入操作

* ```java
  int insert(T entity);//返回值为影响的行数
  ```

* id的生成策略：

  * @TableId注解，为实体类id属性设置生成策略
  * 内部属性type取值：（IdType枚举类）
    * **AUTO**（0）：使用数据库id自增策略控制id生成
    * NONE（1）：不设置id生成策略
    * **INPUT**（2）：用户手工输入id
    * **ASSIGN_ID**（3）（默认的）：雪花算法生成id，可兼容数值型与字符串型，如果自己手动输入了，那么用你输入的
    * ASSIGN_UUID（4）：以UUID生成算法作为id生成策略

  * 雪花算法：生成一个64位二进制数（long值）

    ```ABAP
    0|00100101001001010010010100100101001001010|10000|10001|000000000010
    占位符（1）|时间戳（41）|机器码（5+5）|序列号（12）
    ```

  * @TableId(IdType = IdType.ASSIGN_ID)等同于在yml文件中配置MP全局属性

    ```yml
    	global-config:
    		banner: false
    		db-config:
    		  id-type: assign_id
    ```

* id回填：MP在插入完数据后，会从数据库获取对应的id值回填到传入的实体类中

* 返回值为影响的行数，也就是插入的数据条数



## 3.2 @TableField

* MP通过@TableField注解可以指定字段的一些属性
* 常用于解决两个问题：
  * 对象中的属性名和数据库中字段名不一致，也就是非驼峰式命名
  * 对象中的属性在表中不存在

* 用法：

  * ```java
    @TableField(select = false)//表示查询时不会查询这个属性
    ```

  * ```java
    @TableField(exist = false)//表示这个属性在数据库中不存在
    ```

  * ```java
    @TableField("email")//属性名与表名不一致，映射到表中的email字段
    ```



## 3.3 更新操作

* MP的更新有两种

### 3.3.1 根据id更新

* ```java
  int updateById(@Param("et") T entity);//返回值为影响的行数
  ```

* 传入的实体类必须要有id，这是必须条件
* 更新的字段可以不是全部字段，可以自行选择想要更新的字段更新



### 3.3.2 根据条件更新

* ```java
  int update(@Param("et") T entity, @Param("ew") Wrapper<T> updateWrapper);//返回值为影响的行数
  ```

* 第一个参数为更新的实体类，内搭更新后的字段内容

* 第二个参数为更新条件

* QueryWrapper：

  ```java
  User user = new User();
  user.setPassword("020920");//设置更新的字段内容
  QueryWrapper<User> wrapper = new QueryWrapper<>();
  wrapper.eq("user_name","CaoC");//设置更新的条件 列名，具体数据名
  System.out.println("impact "+userMapper.update(user,wrapper)+" rows in the table");//返回更新的条数
  ```

* UpdateWrapper：

  ```java
  UpdateWrapper<User> updateWrapper = new UpdateWrapper<>();
  updateWrapper.set("password","020920")//设置更新字段内容 那么无需在创建实体类对象参数传入 直接传入null
               .set("age",21)
               .eq("user_name","TEST3");//设置更新字段列名条件 列名，具体数据名
  System.out.println("impact "+userMapper.update(null,updateWrapper)+" rows in the table");
  ```



## 3.4 删除操作

* MP有多种删除操作

### 3.4.1 根据id删除单条数据

* ```java
  int deleteBatchIds(@Param("coll") Collection<?> idList);
  ```

* ```java
  	@Test
      public void testDeleteById() {
          //根据id删除，传入id值即可
          System.out.println("impact "+userMapper.deleteById(8L)+" rows in the table");
      }
  ```



### 3.4.2 根据Map删除数据

* ```java
  int deleteByMap(@Param("cm") Map<String, Object> columnMap);
  ```

* ```java
  	@Test
      public void testDeleteByMap() {
          //根据map删除，map内的装的是多条件，各条件之间为and关系
          HashMap<String, Object> map = new HashMap<>();
          map.put("user_name","LiuB");//多条数据都符合这些条件的话，全部都会被删除
          map.put("password","123456");
          System.out.println("impact "+userMapper.deleteByMap(map)+" rows in the table");
      }
  ```



### 3.4.3 根据包装条件删除

与Map删除的区别就是，包装条件是可以设置范围删除的，将一个范围内的全部删除

* ```java
  int delete(@Param("ew") Wrapper<T> queryWrapper);
  ```

* ```java
  	@Test
      public void testDelete() {
          //根据包装的条件进行删除 -- QueryWrapper的使用方式一
          QueryWrapper<User> wrapper = new QueryWrapper<>();
          wrapper.eq("user_name","LiuB")//与deleteByMap一样，如果多条数据符合条件，那么都会被删除
                  .eq("password","123456");
          System.out.println("impact "+userMapper.delete(wrapper)+" rows in the table");
  
          //QueryWrapper的使用方式二 推荐使用方式二，因为不用手写数据库字段名，防止写错
          User user = new User();
          user.setUserName("LiuB2");//与deleteByMap一样，如果多条数据符合条件，那么都会被删除
          user.setPassword("123456");
          QueryWrapper<User> queryWrapper = new QueryWrapper<>(user);//将刚刚设置的user对象封装为一个QueryWrapper对象
          System.out.println("impact "+userMapper.delete(queryWrapper)+" rows in the table");
      }
  ```



### 3.4.4 批量删除

* ```java
  int deleteBatchIds(@Param("coll") Collection<?> idList);
  ```

* ```java
  	@Test
      public void testDeleteBatchIds() {
          //多id批量删除，也就是在网页中的多条选中删除
          System.out.println("impact "+userMapper.deleteBatchIds(Arrays.asList(15L,16L,17L,18L))+" rows in the table");
      }
  ```



## 3.5 查询操作

### 3.5.1 根据id查询一条数据

* ```java
  T selectById(Serializable id);
  ```

* ```java
  	@Test
      public void testSelectById() {
          System.out.println(userMapper.selectById(2L));
      }
  ```



### 3.5.2 根据条件查询一条数据

* ```java
  T selectOne(@Param("ew") Wrapper<T> queryWrapper);
  ```

* ```java
  	@Test
      public void testSelectOne() {
          QueryWrapper<User> userQueryWrapper = new QueryWrapper<>();
          userQueryWrapper.eq("user_name","CaoC");//封装查询条件去查找一条数据，或许能叫精准查询 当匹配条件的数据不止一条，则会报错 未查到到则返回null
          User user = userMapper.selectOne(userQueryWrapper);
          System.out.println(user);
      }
  ```

  

### 3.5.3 根据条件查询数据有多少条

* ```java
  Long selectCount(@Param("ew") Wrapper<T> queryWrapper);
  ```

* ```java
  	@Test
      public void testSelectCount() {//根据条件，查询符合条件的数据有多少条
          Integer count = userMapper.selectCount(null);//条件封装对象为空，那么就是查询数据控总计多少条数据
          System.out.println(count);
          QueryWrapper<User> userQueryWrapper = new QueryWrapper<>();
          userQueryWrapper.gt("age",20);//封装条件 年龄字段值大于20
          Integer count1 = userMapper.selectCount(userQueryWrapper);
          System.out.println(count1);
      }
  ```

  

### 3.5.4 根据id批量查询

* ```java
  List<T> selectBatchIds(@Param("coll") Collection<? extends Serializable> idList);
  ```

* ```java
  	@Test//根据id批量查询
      public void testSelectBatchIds() {
          List<User> users = userMapper.selectBatchIds(Arrays.asList(3L, 4L, 5L, 100L));//未查找到的数据null都不会显示
          for (User user : users) {
              System.out.println(user);
          }
      }
  ```

  

### 3.5.5 根据条件查询多条数据

* ```java
  List<T> selectList(@Param("ew") Wrapper<T> queryWrapper);
  ```

* ```java
  	@Test
      public void TestSelectList() {
          List<User> users = userMapper.selectList(null);//条件包装对象传入null时，是查询所有数据
          for (User user : users) {
              System.out.println(user);
          }
          QueryWrapper<User> userQueryWrapper = new QueryWrapper<>();
          userQueryWrapper.eq("password", "123456");//设置需要匹配的字段以及值
          List<User> users1 = userMapper.selectList(userQueryWrapper);//根据条件查询，查询所有符合条件的数据
          for (User user : users1) {
              System.out.println(user);
          }
      }
  ```

  

### 3.5.6 分页查询

* ```java
  <P extends IPage<T>> P selectPage(P page, @Param("ew") Wrapper<T> queryWrapper);
  ```

* 配置MP专属的分页插件，先配置MP配置类（MpConfig），内置MP的分页拦截器，返回一个分页插件，且将返回值设为bean

  ```java
  @Configuration
  public class MybatisPlusConfig {
      @Bean//配置分页插件 --分页拦截器
      public PaginationInterceptor paginationInterceptor() {
          return new PaginationInterceptor();
      }
  }
  ```

* ```java
  	@Test
      public void testSelectPages() {
          //分页查询
          Page<User> userPage = new Page<>(0,2);//设置分页参数，查第几页，每页多少条
          QueryWrapper<User> userQueryWrapper = new QueryWrapper<>();
          userQueryWrapper.eq("password","123456");//封装匹配条件参数
          Page<User> page = userMapper.selectPage(userPage, userQueryWrapper);
          System.out.println("数据总条数: " + page.getTotal());
          System.out.println("数据总页数: " + page.getPages());
          System.out.println("数据当前页: " + page.getCurrent());
          System.out.println("每页多少条数据: " + page.getSize());
          List<User> records = page.getRecords();//查询出来的所有数据
          for (User record : records) {
              System.out.println(record);
          }
      }
  ```

  

## 3.6 SQL语句注入原理

MP在启动之后，会将BaseMapper中的一系列方法注册到meppedStatements中

**ISqlInjector**接口负责SQL语句注入工作，**AbstractSqlInjector**为这个接口实现类

主要由AbstractSqlInjector的**inspectInject()方法**进行注入

方法内的**methodList.forEach(m->m.inject(builderAssistant，mapperClass，modeClass，tableInfo))；**是关键，循环遍历方法进行注入

最终**调用抽象方法：injectMappedStatement**进行真正地注入

injectMappedStatement被多种方法重写实现，比如：selectById等

将需要的SQL语句封装，然后我们给他传递关键参数，最后注入到MP的容器中



# 4.MyBatisPlus的配置

## 4.1 基本配置

### 4.1.1 configLocation

**MP配置文件的位置**，如果有单独的MyBatis配置文件，那么就将其路径配置到configLocation中，MyBatis的核心配置文件mybatis--config.xml

SpringBoot：

```properties
mybatis-plus.config-location = classpath:mybatis-config.xml
```

比如：在mybais-config.xml内添加分页插件，在SpringBoot中添加了数据库的基本配置信息，那么就无需再添加，只需再添加一些插件信息

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
<!--    将分页插件转移到Mybatis核心配置文件中 -->
    <plugins>
        <plugin interceptor="com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor"></plugin>
    </plugins>
</configuration>
```

SpringMVC：

在Spring的核心核心配置文件中。我们需要在MP提供的整合Spring+MP的SqlSessionFactory的bean中添加属性configLocation

```xml
<!--    使用MP提供的sqlSessionFactory，完成Spring+MP的整合-->
    <bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <!--添加额外的属性注入，configLocation，也就是添加MyBatis的核心配置-->
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
    </bean>
```

如果MyBatis没有其他的额外配置，那么就为空

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

</configuration>
```



### 4.1.2 mapperLocation

**MyBatis的Mapper所对应的XML文件的位置**，如果在Mapper中有自定义的方法，XML中有自定义的实现，那么就需要进行这个配置，告诉Mapper所对应的XML文件位置

SpringBoot：

```properties
mybatis-plus.mapper-locations = classpath*:mybatis/*.xml
```

* **classpath和classpath\*区别：** 

  classpath：只会到你的class路径中查找找文件。

  classpath*：不仅包含class路径，还包括jar文件中（class路径）进行查找。

  注意： 用classpath\*:需要遍历所有的classpath，所以加载速度是很慢的；因此，在规划的时候，应该尽可能规划好资源文件所在的路径，尽量避免使用classpath\*。

  **classpath\*的使用：**

  maven多模块项目的扫描路劲需以classpath*开头，也就是多jar包下扫描xml文件

  当项目中有多个classpath路径，并同时加载多个classpath路径下（此种情况多数不会遇到）的文件，\*就发挥了作用，如果不加\*，则表示仅仅加载第一个classpath路径。

SpringMVC：

```xml
    <!--    使用MP提供的sqlSessionFactory，完成Spring+MP的整合-->
    <bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath*:mybatis/*.xml"/>
    </bean>
```



不管是SpringBoot还是SpringMVC，都是在对应的resource目录下创建mybatis目录放入UserMapper.xml文件，内置自定义方法

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.wyh.mapper.UserMapper">

    <select id="findById" resultType="com.wyh.domain.User">
        select * from tb_user where id = #{id}
    </select>

</mapper>
```



### 4.1.3 typeAliasesPackage

MyBatis别名包扫描路径，通过该属性，给包中的类注册别名，注册后，在Mapper对应的xml文件中可以直接使用类名，无需再使用全限定类名

SpringBoot：

```properties
mybatis-plus.type-aliases-package = com.wyh.domain
```

SpringMVC：

```xml
	<bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath*:mybatis/*.xml"/>
        <property name="typeAliasesPackage" value="com.wyh.domain"/>
    </bean>
```



## 4.2 进阶配置

这些配置大部分是MyBatis的原生配置，可以通过Mybatis的xml文件进行配置

### 4.2.1 mapUnderscoreToCamelCase

* 类型：boolean
* **默认值：true**

是否开启自动驼峰命名规则映射，也就是**从数据库的下划线命名到驼峰命名的映射**

**这个属性在MyBatis默认为false**，MP中，**此属性将用于生成最终的SQL语句的select body**

@TableField注解用于指定数据库字段名。也就是下划线和驼峰的映射

SpringBoot：

```properties
mybatis-plus.configuration.map-underscore-to-camel-case=false
#关闭自动驼峰，该参数不能与mybatis-plus.config-location同时存在
```



### 4.2.2 cacheEnabled

* 类型boolean
* 默认为true

全局域开启或关闭配置文件中的所有映射器已经配置好的任何缓存

SpringBoot：

```properties
mybatis-plus.configuration.cache-enabled=false
#禁用缓存
```



## 4.3 DB策略配置

### 4.3.1 idType

* 类型：com.baomidou.mybatisplus..annotation.idType
* 默认值：ID_WORKER

**全局默认主键类型**，设置后，**即可省略实体对象中的@TableId(type=AUTO)配置**

SpringBoot：

```properties
mybatis-plus.global-config.db-config.id-type=auto
#全局id增长策略 -- 自动增长
```

SpringMVC：

```xml
	<bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath*:mybatis/*.xml"/>
        <property name="typeAliasesPackage" value="com.wyh.domain"/>
        <property name="gloablConfig">
        	<bean class="com.baomidou.mybatisplus.core.config.GlobalConfig">
            	<property name="dbConfig">
                	<bean class="com.baomidou.mybatisplus.core.config.GlobalConfig">
                    	<property name="idType" value="AUTO"/>
                    </bean>
                </property>
            </bean>
        </property>
    </bean>
```



### 4.3.2 tablePrefix

* 类型：String
* 默认值：null

表名前缀，**全局配置**后**可省略@TableName()配置**

SpringBoot：

```properties
mybatis-plus.global-config.db-config.table-prefix=tb_
#指明表的前缀名
```

SpringMVC：

```xml
	<bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource"/>
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="mapperLocations" value="classpath*:mybatis/*.xml"/>
        <property name="typeAliasesPackage" value="com.wyh.domain"/>
        <property name="gloablConfig">
        	<bean class="com.baomidou.mybatisplus.core.config.GlobalConfig">
            	<property name="dbConfig">
                	<bean class="com.baomidou.mybatisplus.core.config.GlobalConfig">
                    	<property name="idType" value="AUTO"/>
                        <property name="tablePrefix" value="tb_"/>
                    </bean>
                </property>
            </bean>
        </property>
    </bean>
```





# 5.条件构造器

MP的条件构造器，也就是**Wrapper接口**

其下有许多实现类，其中**AbstractWrapper**（是抽象类）和AbstractChainWrapper是重点实现



## 5.1 AbstractWrapper

是QueryWrapper（LambdaQueryWrapper）和UpdateWrapper（LambdaUpdateWrapper）的父类

用于生成SQL的where条件

entity属性也用于生成SQL的where条件，但是生成的where条件与使用各个API生成的where条件没有任何关联行为

### 5.1.1 allEq

```java
allEq(Map<R, V> params)
allEq(Map<R, V> params, boolean null2IsNull)
allEq(boolean condition, Map<R, V> params, boolean null2IsNull)
```

- 全部eq(或个别isNull)

  个别参数说明:

  `params` : `key`为数据库字段名,`value`为字段值
  `null2IsNull` : 为`true`则在`map`的`value`为`null`时调用 [isNull](https://baomidou.com/pages/10c804/#isnull) 方法,为`false`时则忽略`value`为`null`的

- 例1: `allEq({id:1,name:"老王",age:null})`--->`id = 1 and name = '老王' and age is null`

- 例2: `allEq({id:1,name:"老王",age:null}, false)`--->`id = 1 and name = '老王'`

```java
allEq(BiPredicate<R, V> filter, Map<R, V> params)
allEq(BiPredicate<R, V> filter, Map<R, V> params, boolean null2IsNull)
allEq(boolean condition, BiPredicate<R, V> filter, Map<R, V> params, boolean null2IsNull) 
```

个别参数说明:

`filter` : 过滤函数,是否允许字段传入比对条件中
`params` 与 `null2IsNull` : 同上

- 例1: `allEq((k,v) -> k.indexOf("a") >= 0, {id:1,name:"老王",age:null})`--->`name = '老王' and age is null`
- 例2: `allEq((k,v) -> k.indexOf("a") >= 0, {id:1,name:"老王",age:null}, false)`--->`name = '老王'`

测试用例：

```java
	@Test
    public void testAllEq() {
        HashMap<String, Object> params = new HashMap<>();
        params.put("name","Tom");
        params.put("age",21);
        params.put("password",null);//这里设置完，那么SQL语句的条件为三个，且都为并列语句，也就是，这里的null就是password=null的数据 所以待会查不出数据
        QueryWrapper<User> wrapper = new QueryWrapper<>();
//        wrapper.allEq(params);
//        wrapper.allEq(params,false);//这里的false指的是，是否将条件里的null作为条件，所以待会SQL语句中没有为null的条件，可以查出数据
        wrapper.allEq((k,v)->(k.equals("name")||k.equals("id")), params);//这里的前面的Lambda简化后的语句的意思是：看传入的params中的key与设置的两个条件是否匹配，不匹配则不会作为SQL的查询条件
        User user = userMapper.selectOne(wrapper);
        System.out.println(user);
    }
```



### 5.1.2 基本比较操作

eq

```java
eq(R column, Object val)
eq(boolean condition, R column, Object val)
```

- 等于 =
- 例: `eq("name", "老王")`--->`name = '老王'`

ne

```java
ne(R column, Object val)
ne(boolean condition, R column, Object val)
```

- 不等于 <>
- 例: `ne("name", "老王")`--->`name <> '老王'`

gt

```java
gt(R column, Object val)
gt(boolean condition, R column, Object val)
```

- 大于 >
- 例: `gt("age", 18)`--->`age > 18`

ge

```java
ge(R column, Object val)
ge(boolean condition, R column, Object val)
```

- 大于等于 >=
- 例: `ge("age", 18)`--->`age >= 18`

lt

```java
lt(R column, Object val)
lt(boolean condition, R column, Object val)
```

- 小于 <
- 例: `lt("age", 18)`--->`age < 18`

le

```java
le(R column, Object val)
le(boolean condition, R column, Object val)
```

- 小于等于 <=
- 例: `le("age", 18)`--->`age <= 18`

between

```java
between(R column, Object val1, Object val2)
between(boolean condition, R column, Object val1, Object val2)
```

- BETWEEN 值1 AND 值2
- 例: `between("age", 18, 30)`--->`age between 18 and 30`

notBetween

```java
notBetween(R column, Object val1, Object val2)
notBetween(boolean condition, R column, Object val1, Object val2)
```

- NOT BETWEEN 值1 AND 值2
- 例: `notBetween("age", 18, 30)`--->`age not between 18 and 30`



### 5.1.3 模糊查询

like

```java
like(R column, Object val)
like(boolean condition, R column, Object val)
```

- LIKE '%值%'
- 例: `like("name", "王")`--->`name like '%王%'`

notLike

```java
notLike(R column, Object val)
notLike(boolean condition, R column, Object val)
```

- NOT LIKE '%值%'
- 例: `notLike("name", "王")`--->`name not like '%王%'`

likeLeft

```java
likeLeft(R column, Object val)
likeLeft(boolean condition, R column, Object val)
```

- LIKE '%值'
- 例: `likeLeft("name", "王")`--->`name like '%王'`

likeRight

```java
likeRight(R column, Object val)
likeRight(boolean condition, R column, Object val)
```

- LIKE '值%'
- 例: `likeRight("name", "王")`--->`name like '王%'`



### 5.1.4 排序条件

orderByAsc

```java
orderByAsc(R... columns)
orderByAsc(boolean condition, R... columns)
```

- 排序：ORDER BY 字段, ... ASC
- 例: `orderByAsc("id", "name")`--->`order by id ASC,name ASC`

orderByDesc（倒序）

```java
orderByDesc(R... columns)
orderByDesc(boolean condition, R... columns)
```

- 排序：ORDER BY 字段, ... DESC
- 例: `orderByDesc("id", "name")`--->`order by id DESC,name DESC`

orderBy

```java
orderBy(boolean condition, boolean isAsc, R... columns)
```

- 排序：ORDER BY 字段, ...
- 例: `orderBy(true, true, "id", "name")`--->`order by id ASC,name ASC`

having

```java
having(String sqlHaving, Object... params)
having(boolean condition, String sqlHaving, Object... params)
```

- HAVING ( sql语句 )
- 例: `having("sum(age) > 10")`--->`having sum(age) > 10`
- 例: `having("sum(age) > {0}", 11)`--->`having sum(age) > 11`

func

```java
func(Consumer<Children> consumer)
func(boolean condition, Consumer<Children> consumer)
```

- func 方法(主要方便在出现if...else下调用不同方法能不断链)
- 例: `func(i -> if(true) {i.eq("id", 1)} else {i.ne("id", 1)})`



### 5.1.5 逻辑查询

or

```java
or()
or(boolean condition)
```

- 拼接 OR

  注意事项:

  主动调用`or`表示紧接着下一个**方法**不是用`and`连接!(不调用`or`则默认为使用`and`连接)

- 例: `eq("id",1).or().eq("name","老王")`--->`id = 1 or name = '老王'`

```java
or(Consumer<Param> consumer)
or(boolean condition, Consumer<Param> consumer)
```

- OR 嵌套
- 例: `or(i -> i.eq("name", "李白").ne("status", "活着"))`--->`or (name = '李白' and status <> '活着')`

and（没有or那么就是默认and）

```java
and(Consumer<Param> consumer)
and(boolean condition, Consumer<Param> consumer)
```

- AND 嵌套
- 例: `and(i -> i.eq("name", "李白").ne("status", "活着"))`--->`and (name = '李白' and status <> '活着')`



### 5.1.6 其他查询

isNull

```java
isNull(R column)
isNull(boolean condition, R column)
```

- 字段 IS NULL
- 例: `isNull("name")`--->`name is null`

isNotNull

```java
isNotNull(R column)
isNotNull(boolean condition, R column)
```

- 字段 IS NOT NULL
- 例: `isNotNull("name")`--->`name is not null`

in

```java
in(R column, Collection<?> value)
in(boolean condition, R column, Collection<?> value)
```

- 字段 IN (value.get(0), value.get(1), ...)
- 例: `in("age",{1,2,3})`--->`age in (1,2,3)`

```java
in(R column, Object... values)
in(boolean condition, R column, Object... values)
```

- 字段 IN (v0, v1, ...)
- 例: `in("age", 1, 2, 3)`--->`age in (1,2,3)`

notIn

```java
notIn(R column, Collection<?> value)
notIn(boolean condition, R column, Collection<?> value)
```

- 字段 NOT IN (value.get(0), value.get(1), ...)
- 例: `notIn("age",{1,2,3})`--->`age not in (1,2,3)`

```java
notIn(R column, Object... values)
notIn(boolean condition, R column, Object... values)
```

- 字段 NOT IN (v0, v1, ...)
- 例: `notIn("age", 1, 2, 3)`--->`age not in (1,2,3)`

inSql

```java
inSql(R column, String inValue)
inSql(boolean condition, R column, String inValue)
```

- 字段 IN ( sql语句 )
- 例: `inSql("age", "1,2,3,4,5,6")`--->`age in (1,2,3,4,5,6)`
- 例: `inSql("id", "select id from table where id < 3")`--->`id in (select id from table where id < 3)`

notInSql

```java
notInSql(R column, String inValue)
notInSql(boolean condition, R column, String inValue)
```

- 字段 NOT IN ( sql语句 )
- 例: `notInSql("age", "1,2,3,4,5,6")`--->`age not in (1,2,3,4,5,6)`
- 例: `notInSql("id", "select id from table where id < 3")`--->`id not in (select id from table where id < 3)`

groupBy

```java
groupBy(R... columns)
groupBy(boolean condition, R... columns)
```

- 分组：GROUP BY 字段, ...
- 例: `groupBy("id", "name")`--->`group by id,name`

nested

```java
nested(Consumer<Param> consumer)
nested(boolean condition, Consumer<Param> consumer)
```

- 正常嵌套 不带 AND 或者 OR
- 例: `nested(i -> i.eq("name", "李白").ne("status", "活着"))`--->`(name = '李白' and status <> '活着')`

apply

```java
apply(String applySql, Object... params)
apply(boolean condition, String applySql, Object... params)
```

- 拼接 sql

  注意事项:

  该方法可用于数据库**函数** 动态入参的`params`对应前面`applySql`内部的`{index}`部分.这样是不会有sql注入风险的,反之会有!

- 例: `apply("id = 1")`--->`id = 1`

- 例: `apply("date_format(dateColumn,'%Y-%m-%d') = '2008-08-08'")`--->`date_format(dateColumn,'%Y-%m-%d') = '2008-08-08'")`

- 例: `apply("date_format(dateColumn,'%Y-%m-%d') = {0}", "2008-08-08")`--->`date_format(dateColumn,'%Y-%m-%d') = '2008-08-08'")`

last

```java
last(String lastSql)
last(boolean condition, String lastSql)
```

- 无视优化规则直接拼接到 sql 的最后

  注意事项:

  只能调用一次,多次调用以最后一次为准 有sql注入的风险,请谨慎使用

- 例: `last("limit 1")`

exists

```java
exists(String existsSql)
exists(boolean condition, String existsSql)
```

- 拼接 EXISTS ( sql语句 )
- 例: `exists("select id from table where age = 1")`--->`exists (select id from table where age = 1)`

notExists

```java
notExists(String notExistsSql)
notExists(boolean condition, String notExistsSql)
```

- 拼接 NOT EXISTS ( sql语句 )
- 例: `notExists("select id from table where age = 1")`--->`not exists (select id from table where age = 1)`

having

```java
having(String sqlHaving, Object... params)
having(boolean condition, String sqlHaving, Object... params)
```

- HAVING ( sql语句 )
- 例: `having("sum(age) > 10")`--->`having sum(age) > 10`
- 例: `having("sum(age) > {0}", 11)`--->`having sum(age) > 11`

func

```java
func(Consumer<Children> consumer)
func(boolean condition, Consumer<Children> consumer)
```

- func 方法(主要方便在出现if...else下调用不同方法能不断链)
- 例: `func(i -> if(true) {i.eq("id", 1)} else {i.ne("id", 1)})`



## 5.2 QueryWrapper

**继承自 AbstractWrapper** ,自身的内部属性 entity 也用于生成 where 条件
及 LambdaQueryWrapper, 可以通过 new QueryWrapper().lambda() 方法获取



### 5.2.1 select

在MP中查询，默认查询所有的字段，也可以通过select设置指定字段查询，那么就有一些是不会查询显示的，查询出来时为null

```java
select(String... sqlSelect)
select(Predicate<TableFieldInfo> predicate)
select(Class<T> entityClass, Predicate<TableFieldInfo> predicate)
```

- 设置查询字段

  说明:

  以上方法分为两类.
  第二类方法为: 过滤查询字段(主键除外),入参不包含 class 的调用前需要`wrapper`内的`entity`属性有值! 这两类方法重复调用以最后一次为准

- 例: `select("id", "name", "age")`

- 例: `select(i -> i.getProperty().startsWith("test"))`



## 5.3 UpdateWrapper

说明:

继承自 `AbstractWrapper` ,自身的内部属性 `entity` 也用于生成 where 条件
及 `LambdaUpdateWrapper`, 可以通过 `new UpdateWrapper().lambda()` 方法获取

set

```java
set(String column, Object val)
set(boolean condition, String column, Object val)
```

- SQL SET 字段
- 例: `set("name", "老李头")`
- 例: `set("name", "")`--->数据库字段值变为**空字符串**
- 例: `set("name", null)`--->数据库字段值变为`null`

setSql

```java
setSql(String sql)
```

- 设置 SET 部分 SQL
- 例: `setSql("name = '老李头'")`

lambda

- 获取 `LambdaWrapper`
  在`QueryWrapper`中是获取`LambdaQueryWrapper`
  在`UpdateWrapper`中是获取`LambdaUpdateWrapper`





# 6. ActiveRecord

> ActiveRecord，简称AR，受动态语言（PHP，Ruby）等喜爱
>
> Java作为准静态语言，MP基于Java在AR道路上也有一定的探索

ActiveRecord属于ORM（对象关系映射）层，有Rails提出，遵循标准的ORM模型，表映射到记录，记录映射到对象，字段映射到对象属性

ActiveRecord主要思想：

* 每一个数据库表对应创建一个类。
* 类的每个对象实例对应数据库表的每一条数据记录。
* 表的每个字段都对应自己的Field
* **ActiveRecord同时负责把自己持久化，在ActiveRecord中封装了对数据库的访问，也就是CRUD**。意思就是，把实体类和Mapper一起封装，直接使用类就可以进行CRUD操作
* ActiveRecord是一种领域模型（Domain Model），封装了部分业务逻辑



## 6.1 AR初尝试

只需要将实体类继承Model\<T>即可

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
@TableName("tb_user")
public class User extends Model<User> {
    @TableId(type = IdType.AUTO)
    private Long id;
    private String userName;
    @TableField(select = false)//表示查询时不会查询这个属性
    private String password;
    private String name;
    private Integer age;
```

测试：

```java
@SpringBootTest
public class TestAR {
    @Test
    public void FirstTest() {
        User user = new User();
        User select = user.selectById(2L);//都不需要显示的注入mapper
        System.out.println(select);
    }
}
```

虽然这里没有显示的注入mapper，但是却不能没有mapper，会隐式的调用，这相当于将mapper和User封装在了一起，可以不注入，但是必须要有





## 6.2 添加数据

```java
 	@Test
    public void testAdd() {
        User user = new User();
        user.setUserName("LiuBei");
        user.setPassword("123456");
        user.setName("刘备");
        user.setAge(30);
        user.setMail("test7@test.com");
        boolean insert = user.insert();
        System.out.println("插入是否成功: "+insert);
        System.out.println("插进去的数据的id: " + user.getId());
    }
```





## 6.3 更新操作

```java
	@Test
    public void testUpdate() {
        User user = new User();
        user.setId(19L);//更新条件
        user.setMail("test7@wyh.com");
        boolean update = user.updateById();
        System.out.println("是否更新成功：" + update);

        user.setUserName("LiuB");
        QueryWrapper<User> userQueryWrapper = new QueryWrapper<>();
        userQueryWrapper.eq("user_name","LiuBei");
        user.update(userQueryWrapper);

        User selectById = user.selectById(19L);
        System.out.println(selectById);
    }
```



## 6.4 删除操作

```java
	@Test
    public void testDelete() {
        User user = new User();
//        user.setId(19L);//设置之后下面的删除语句就不需要再添加id
        boolean b = user.deleteById(19L);
        System.out.println("删除是否成功: " + b);
    }
```



## 6.5 根据条件查询

```java
	@Test
    public void testSelect() {
        User user = new User();
        QueryWrapper<User> userQueryWrapper = new QueryWrapper<>();
        userQueryWrapper.ge("age",20);

        List<User> users = user.selectList(userQueryWrapper);
        for (User user1 : users) {
            System.out.println(user1);
        }
    }
```





# 7. 插件

## 7.1 MyBatis插件机制

> MyBatis允许你在已经映射语句执行过程中的某一点进行拦截调用。
>
> 默认情况下，MyBatis允许使用插件来拦截的方法调用包括：
>
> 1. **Executor（update，query，flushStatsement，commit，rollback，getTransaction，close，isClosed）也就是执行时**
> 2. **ParameterHandler（getParamterObject，setParamters）参数处理**
> 3. **ResultSetHandler（handleResultSets，handleOutputParamters）结果处理**
> 4. **StatementHandler（prepare，paramterize，batch，update，query）执行的语句处理**
>
> 总体概括为：
>
> * 拦截**执行器**方法
> * 拦截**参数**处理
> * 拦截**结果集**处理
> * 拦截**SQL语法构建**的处理
>
> 这四个对象都会在plugin的方法内执行，plugin方法总共执行四次
>
> **MyBatis也叫这些插件为拦截器**
>
> MP就是一个插件

插件示例：

```java
@Intercepts({@Signature(
        type = Executor.class,//拦截的类型 -- 执行器
        method = "update",//拦截的是哪个方法 -- 执行器中的那个方法
        args = {MappedStatement.class,Object.class}
)})
public class MyInterceptor implements Interceptor {
    @Override
    public Object intercept(Invocation invocation) throws Throwable{
        //拦截方法，具体业务编写逻辑的位置
        return invocation.proceed();
    }

    @Override
    public Object plugin(Object target) {
        //创建target对象的代理对象，目的是将当前拦截器加入到该对象中去
        //Executor ParameterHandler ResultSetHandler StatementHandler 
        //这四个对象都会在plugin的方法内执行，plugin方法总共执行四次
        return Plugin.wrap(target,this);
    }

    @Override
    public void setProperties(Properties properties) {
        //设置属性
    }
}
```

注入插件到Spring容器中：

```java
@Configuration
public class MybatisPlusConfig {
//    @Bean//配置分页插件 --分页拦截器
//    public PaginationInterceptor paginationInterceptor() {
//        return new PaginationInterceptor();
//    }

    @Bean//注入自定义的拦截器 也就是插件 也可以通过XML文件的方式配置 在mybatis-config.xml文件中
    public MyInterceptor myInterceptor() {
        return new MyInterceptor();
    }
}
```

或者通过XML文件的配置方式：

```xml
    <plugins>
        <!--    将分页插件转移到Mybatis核心配置文件中 -->
        <plugin interceptor="com.baomidou.mybatisplus.extension.plugins.PaginationInterceptor">

        </plugin>
        <!--添加拦截插件-->
        <plugin interceptor="com.wyh.plugins.MyInterceptor">
            
        </plugin>
    </plugins>
```





## 7.2 执行分析插件

> MP提供的插件，对SQL执行的分析插件
>
> 用作阻断全表更新，删除的一些操作
>
> 该插件仅适用于开发环境，不适用于生产环境

示例：添加在MyBatisPlusConfig中

```java
	@Bean
    public SqlExplainInterceptor sqlExplainInterceptor() {
        SqlExplainInterceptor sqlExplainInterceptor = new SqlExplainInterceptor();
        ArrayList<ISqlParser> sqlParsers = new ArrayList<>();
        sqlParsers.add(new BlockAttackSqlParser());//全表更新删除的阻断器
        sqlExplainInterceptor.setSqlParserList(sqlParsers);

        return sqlExplainInterceptor;
    }
```



## 7.3 性能分析插件

> 性能分析插件，用于输出每条SQL语句以及它的执行时间，可以设置最大的执行时间，超过时间就会抛出异常
>
> **该插件只用于开发，不适用于生产**

```xml
		<!--性能分析插件-->
        <plugin interceptor="com.baomidou.mybatisplus.extension.plugins.PerformanceInterceptor">
            <property name="maxTime" value="100"/><!--maxTime 指的是最大运行时间 单位毫秒-->
            <property name="format" value="true"/><!--是否进行输出的SQL语句的格式化-->
        </plugin>
```





## 7.4 乐观锁插件

> 意图：当更新一条数据时，希望这是没有被别人更改过的
>
> 也就是用于解决秒杀情况时，并发问题
>
> 实现方式：
>
> * 取出记录时，获取当前的version
> * 更新时，要带上这个version
> * 执行更新时，**set version = newVersion where version = oldVersion**
> * 如果version不对，那么就会更新失败



配置：

* Spring：xml

  ```xml
  <bean class="com.baomidou.mybatisplus.extension.plugins.OptimisticLockerInterceptor"/>
  ```

* Spring Boot

  ```java
  @Bean
  public OptimisticLockerInterceptor optimisticLockerInterceptor() {
  	return new OptimisticLockerInterceptor();
  }
  ```



使用：

* 需要在数据库中有对应的字段映射version
* 然后在库表对应的实体类中的对应属性上添加@Version注解



原理：

* 更新Version字段是由MP来实现的。我们要实现的就是先查询到版本信息，然后才能更新
* 所以就避免了多人同时更新时的冲突问题
* 如果version不对，那么就会更新失败



特比说明：

* 支持的数据类型只支持int，Integer，Long，Date，Timestamp，LocalDateTime
* 整数类型下，newVersion=oldVersion + 1
* newVersion会回写到entity（对应的实体类中）中
* 仅支持updateById(id) 和update(entity，wrapper) 方法
* 在update方法下，entity不能复用



## 7.5 SQL语句注入器

> 在MP中，通过**AbstractSqlInjector**将BaseMapper中的方法注入到了Mybatis容器中，然后这些方法才会正常执行
>
> 如果我们想要**扩充BaseMapper中的方法**
>
> 那么我们需要自己写一个

以findAll方法为例

首先，我们需要在mapper包下编写MyBasemapper

```java
public interface MyBaseMapper<T> extends BaseMapper<T> {
	List<T> findAll();
}
```

这样，其他的mapper都可以继承这个，实现了统一的扩展

然后，编写MySqlInjector，如果直接继承AbstractSqlInjector的话，那么原有的BaseMapper中的方法就会失效，所以选择继承它的一个实现类，DefaultSqlInjector

```java
public class MySqlInjector extends DefaultSqlInjector {
	@Override
    public List<AbstractMethod> getMethodList() {
        List<AbstractMethod> methodsList = super.getMethodsList();
        //先添加父类默认的那些CRUD方法
        methodsList.addAll(methodList);
        //再添加自己添加的自定义方法
        methodsList.add(new FindAll());
        return methodsList;
    }
}
```

然后编写自己的FIndAll，参考MP中的SelectById方法

```java
public class FindAll extends Abstractmethod {
    @Override
    public MappedStatement injectMappedStatement(Class<?> mapperClass, Class<?> modelClass, TableInfo tableInfo) {
        String sqlMethod = "findAll";
        String sql = "select * from " + tableInfo.getTableName();
        SqlSource sqlSource = languageDriver.creatSqlSource(configuration,sql,modelClass);
        return this.addSelectMappedStatement(mapperClass,sqlMethod,sqlSource,modelClass,tableInfo);
    }
}
```

再然后，将自己写的SQL注入器，注入到Spring容器中

```java
@Bean
public MySqlInjector mySqlInjector() {
	return new MySqlInjector();
}
```

然后自己测试。



## 7.6 自动填充功能

> 需求：插入或者更新的时候，希望有些**字段可以自动填充数据**，比如密码，version

首先，在想让他自动填充的属性字段上面添加@TableField注解

```
@TableField(fill = FieldFill.INSERT)
private String password;
```

然后注解里面的属性fill，选择自动填充的策略，比如默认不自动填充，或者插入时，更新时，插入更新时。

再然后编写MyMetaObjectHandler

```java
@Component
public class MyMetaObjectHandler implements MetaObjectHandler {
	Object password = getFieldValByName("password",metaObject);
    //字段为空那么就填充，不为空就不填充
    if (null == password) {
        setFieldValByName("password","123456",metaObject);
    }
    
    @Override
    public void updateFill(MetaObject metaObject) {
        
    }
}
```

然后测试。



## 7.7 逻辑删除

> 开发系统时，有时候不是真正意义上的物理删除，我们需要保留一些用户数据，比如我们需要进行一些统计，或者购物订单之类的
>
> 也就是在查询的时候带上一种状态，被标记的数据不会被查询，这样的目的就是为了防止真正地删除

首先修改表的结构：给数据表加上一个字段 deleted，用于表示数据是否会被删除，比如，1为删除，0为未删除

同时，也要修改实体类中属性，添加一个deleted属性，并添加@TableLogic注解

```java
@TableLogic
private Integer deleted;
```

然后再配置好

```properties
#逻辑被删除
mybatis-plus.global-config.db-config.logic-deleted-value=1
#逻辑未删除
mybatis-plus.global-config.db-config.logic-not-deleted-value=0
```

然后测试。



## 7.8 通用枚举

解决繁琐的配置，让MyBatis更优雅的使用枚举属性

以设置性别为例

首先修改表结构，添加一个性别sex字段

同时实体类中也要有对应的属性sex

然后定义枚举(IEnum是MP提供的)

```java
public enum SexEnum implements IEnum<Integer> {
	MAN(1,"男");
    WOMEN(2,"女");
    private int value;
    private String desc;
    SexEnum(int value,String desc) {
        this.value = value;
        this.desc = desc;
    }
    @Override
    public Integer getValue() {
        return this.value;
    }
    @Override
    public String toString() {
        return this.desc;
    }
}
```

然后配置好扫描枚举，会将这些枚举加载到MP中

```properties
#枚举包扫描
mybatis-plus.type-enums-package=com.wyh.enums
```

然后测试。



## 7.9 代码生成器

AutoGenerator是MP的代码生成器，可以快速地生成实体类，mapper，mapper.xml，service，controller等各模块

我自认为了解即可，不要太懒，不然自己代码能力会退化



## 7.10 MyBatisX

> 基于IDEA的快速开发插件
>
> 实现：
>
> java与xml的来回跳转
>
> mapper方法自动生成xml

IDEA安装该插件即可



> ©iWyh2😊
