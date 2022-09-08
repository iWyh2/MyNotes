# SSM基础框架

框架市场占有率极高，反正差不多都需要。

Java开发者写程序必备的技能

## 一.Spring

Spring技术是JavaEE开发的必备技能



### 1. 初识Spring

Spring技术官网[https://spring.io](https://spring.io)

Spring已经成为了一种生态圈，Spring提供了若干的项目，每个项目提供了特定的功能。也就是Spring全家桶。

如：**Spring Framework** -Spring最底层的框架，其他的所有Spring项目都基于这个来运行

​		Spring Boot -加速开发

​		Spring Cloud -分布式开发

EJB思想演化而来，如今为Spring版本为5.0 支持JDK8及以上



### 2. Spring Framework系统架构

Spring Framework是Spring生态圈中最基础、最顶级的项目，是其他项目的根基。

现在的Spring Framework架构图已趋于稳定 为4系列

* Core Container：核心容器。是Spring的核心技术。其他的都是基于此来完成实现的。-也就是IoC容器。
* Data Access：数据访问。
* Data Integration：数据集成。也就是可以和其他的技术框架配合使用。
  * Transactions：事务。Spring的重大技术突破，提高开发效率。
* AOP：面向切面编程思想。
* Aspects：AOP思想的实现。被Spring收录，因为他做的比Spring好，Spring也服气，也推荐你用这个。
* Test：单元测试与集成测试。
* Web：Web开发。



### 3. 核心概念

代码书写现状：耦合度偏高。如：业务层的实现与数据层的实现。

解决方案：使用对象时，在程序中不要主动使用new来产生对象，转换为**由外部提供对象**。

**IoC**：Inversion of Control **控制反转**

* **对象的创建控制权**由程序转移到**外部**，这种思想叫IoC。
* Spring对IoC思想进行了实现，提供了一个容器，为**IoC容器**，也就是Spring的**Core Container**，用来**充当IoC思想中的外部**
* **IoC容器负责对象的创建，初始化等一系列操作工作**，被创建或被管理的对象在IoC容器中被称为**Bean**

**DI**：Dependency Injection **依赖注入**

* 在容器中**建立Bean与Bean之间的依赖关系的整个过程**，被称为依赖注入
  * 如：service层的对象在实现功能时，需要有dao层的对象，所以，在IoC容器中，service会和dao绑定依赖的关系，同时被提供去实现功能

这所做的一切的**最终目标**：就是为了**充分解耦**

* 实现：
  * 使用IoC容器管理Bean(IoC)
  * 在IoC容器内将所有有依赖关系的Bean进行绑定(DI)
* 最终效果：使用对象时，不仅可以直接从IoC容器中获取，并且还可以获取和已获取的Bean绑定了所有依赖关系的其他Bean



### 4.入门案例

#### 4.1 IoC入门案例(XML配置版)

##### 4.1.1 案例思路分析

* 首先，需要管理的是那些一个个耦合度高的Bean，也就是service和dao层对象
* 然后，要将管理的对象放入IoC容器中，那么目前的基础方法为配置xml实现
* 想要获取这个IoC容器，那么需要用Spring提供的接口去获取
* 容器得到后，再用方法去获取这些被管理的Bean对象
* 要使用Spring的技术，那么IDEA肯定要导入坐标

##### 4.1.2 案例实现

* 导入Spring的坐标：

  ```xml
  <dependency>						                 
      <groupId>org.springframework</groupId>           
      <artifactId>springcontext</artifactId>     
      <version>5.2.10.RELEASE</version>     
  </dependency>
  ```

* 要有需要Spring管理的Bean类（service、dao）

* 在resource目录下创建Spring的配置文件：applicationContext.xml

* 在配置文件中配置需要的Bean对象：

  ```
  <bean class="com.wyh.dao.impl.BookDaoImpl" id="bookDao"/>
  <bean class="com.wyh.service.impl.BookServiceImpl" id="bookService"/>
  ```

  * `<bean>`是指配置Bean
  * class属性是给Bean定义类型
  * id属性是给这个bean起名字

* ```java
  //获取IoC容器
  ApplicationContext applicationContext = new ClassPathXmlApplicationContext
      ("applicationContext.xml");
  //获取Bean对象
  BookDao bookDao = (BookDao) applicationContext.getBean("bookDao");
  //执行对应的方法
  bookDao.save();
  ```

  * 配置文件名叫applicationContext.xml就是因为这个Spring提供的接口就叫ApplicationContext，它的实现类为ClassPathXmlApplicationContext
  * 然后调用getBean方法，参数为前面配置的id属性

#### 4.2 DI入门案例

##### 4.2.1 DI入门案例思路分析

* 首先是基于IoC来管理这些Bean的
* 因为耦合度高，所以service中以new形式创建的Dao对象不能保留了
* 那么service中需要的Dao对象就需要我们提供方法让其进入到service中去
* 同时，我们还要用配置，去绑定好service与dao之间的关系

##### 4.2.2 案例实现

1. 删去上个IoC案例中的service层的，以new形式创建的Dao对象

   ```java
   private BookDao bookDao;
   ```

2. 同样的，在service层中生成一个set方法，能初始化这个dao对象，这个set方法由Spring来调用

   ```java
   public void setBookDao(BookDao bookDao) {
       this.bookDao = bookDao;
   }
   ```

3. 在applicationContext.xml中配置service与dao对象之间的关系

   ```xml
   <bean class="com.wyh.service.impl.BookServiceImpl" id="bookService">
           <property name="bookDao" ref="bookDao"/>
   </bean>
   ```

* property表示给当前的Bean配置属性，也就是给这个类的对象绑定关系
* name属性表示 选择具体的 要绑定的 是类中的哪个成员变量对象 的名称
* ref属性表示name选择的这个对象要参照的具体类型是什么，具体是哪一个bean，也就是另一个Bean的id属性



### 5.Bean

#### 5.1 Bean的配置

##### 5.1.1 Bean的基础配置

* 名称：bean

* 类型：标签

* 所属：beans标签

* 功能：定义Spring的核心容器管理的对象

* 格式：

  ```xml
  <beans>
  	<bean/>
      <bean></bean>
  </beans>
  ```

  

* 属性列表：
  * id：bean的id，使用容器可以通过id值获取对应的bean，在一个容器中id值唯一
  * class：bean的类型，配置bean的全路径类名

##### 5.1.2 Bean的别名配置

* 属性名称：name

* 所属：bean标签

* 功能：定义bean的别名，可定以多个，用逗号，分号，空格分隔这些别名

* 例如：

  ```xml
  <bean class="com.wyh.service.impl.BookServiceImpl" id="bookService" name="service serviceEBi"/>
  ```

* 注意事项：获取bean时，无论是通过id还是name，如果无法获取，那么会抛出异常：NoSuchBeanDefinitionException，找不到这个名字的bean，也就是名字写错了

##### 5.1.3 Bean的作用范围

* 属性名称：scope

* 所属：bean标签

* 功能：定义bean的作用范围，也就是创建的bean的数目

  * 取值：
    * singleton：单例（默认）
    * prototype：非单例

* 如：

  ```xml
  <bean class="com.wyh.service.impl.BookServiceImpl" id="bookService" name="service serviceEBi" scope="prototype"/>
  ```

* 作用范围说明：
  * bean默认为单例是因为，管理的是能复用的bean，这样开发速度才快
  * 适合交给IoC容器管理的bean：
    * 表现层对象
    * 业务层对象
    * 数据层对象
    * 一些工具对象
  * 不适合交给IoC容器管理的bean：
    * 封装实体的域对象（也就是这里面它含有一些特定的数据）

#### 5.2 Bean的实例化

实例化Bean的三种方法：

1. 构造方法（最常用）

   bean的本质就是一个对象，所以创建bean使用构造方法来完成

   也就是必须要有**无参构造器**在所管理的Bean的类中，必须是无参，**自定义无参或者默认无参都可以**

   如果无参构造器不存在，会报**BeanCreationException**

   构造方法由Spring来调用

2. 静态工厂（了解）

   创建对象的方式变成了创建一个工厂对象，调用工厂类的静态方法来获取需要的Bean

   ```java
   public class OrderDaoFactory {
       public static OrderDao getOrderDao() {
           return new OrderDaoImpl();
       }
   }
   ```

   对于配置的改变：从配置这个Bean类，变为了**配置他的专属工厂类**,同时还要配置调用工厂方法的属性**factory-method**，值为工厂中的获取对象的静态方法

   ```xml
   <bean
   	id="orderDao"
   	factory-method="getOrderDao"
   	class="com.wyh.factory.OrderDaoFactory"/>
   ```

3. 实例工厂（了解）

   类似于静态工厂，但是那个获取对象的方法不是静态的

   ```java
   public class UserDaoFactory {
       public UserDao getUserDao() {
           return new UserDaoImpl();
       }
   }
   ```

   因为这个方法不是静态的，所以需要你实例化这个工厂才能调用这个获取对象方法去获取bean，那么配置也需要改变：

   ```xml
   <bean
         id="userDaoFactory"
         class="com.wyh.factory.UserDaoFactory"/>
   <bean
         id="userDao"
         factory-bean="userDaoFactory"
         factory-method="getUserDao"/>
   ```

   也就是，需要先配置这个工厂为bean，然后在需要的bean内加上属性**factory-bean**，值为配置的工厂bean的id，然后再加上属性**factory-method**，取值为这个工厂内的获取bean的非静态方法

   但由此发现了很多鸡肋，比如配置了一个无用`<bean>`，后面的factory-bean和factory-method每次都要更改，不是很方便，由此，Spring对其优化，将这个实例工厂方法改良

4. 实例工厂—改良升级版（实用）

   定义这个工厂类时，需要实现Spring提供的接口**FactoryBean**，且这是个泛型，要指定这个工厂获取的bean对象是哪一类，还要至少重写其里面的三个方法中的前两个，获取bean对象，获知bean对象类型，指定是否为单例创建（此方法可不重写，默认为true，也就是默认为单例创建，需要非单例时才重写并改为false）

   ```java
   public class UserDaoFactoryBean implements FactoryBean<UserDao> {
       public UserDao getObject() throws Exception {
           return new UserDaoImpl();
       }
       public Class<?> getObjectType() {
           return UserDao.class;
       }
       //指定创建的对象是单例还是非单例，此方法可不重写
       public boolean isSingleton() {
           return false;
       }
   }
   ```

   配置文件的改变：从配置所需bean，变为了直接配置这个创建的Factorybean

   ```xml
   <bean
         id="userDao"
         class="com.wyh.factory.UserDaoFactoryBean"/>
   ```

   那么在IoC容器中获取bean对象时，getBean的值填这个FactoryBean的id值，Spring就会去调用里面的getObject方法，获取bean对象

#### 5.3 Bean的生命周期

bean的生命周期：bean从创建到销毁的整体过程

bean的生命周期的控制：在bean创建后到销毁前做的一些事情

* 自己提供生命周期的控制方法

  ```java
  public class BookDaoImpl implements BookDao {
      ...
      public void init() {}
      public void destory() {}
  }
  ```

* 然后把这些生命周期的控制方法配置到`<bean>`中

  ```xml
  <bean ... init-method="init" destory-method="destory"/>
  ```

以上方法可以替换为用实现接口，然后重写对应的方法来实现：

* 比如初始时和销毁时：

  ```java
  public class BookServiceImpl implements BookService, InitializingBean, DisposableBean {
  ...
  	public void afterPropertiesSet() throws Exception {}
  	public void destory() throws Exception {}
  }
  ```

* bean生命周期：

  * 初始化容器
    1. 创建对象（分配内存）
    2. 执行构造方法
    3. 执行属性注入（set操作）
    4. 执行bean初始化方法
  * 使用bean
    * 执行业务操作
  * 关闭/销毁容器
    * 执行bean的销毁方法

* bean的销毁时机

  * 容器（不是java虚拟机）关闭前触发bean的销毁

  * 关闭容器的方式：

    * 手工关闭容器（暴力关闭）

      调用`ConfigurableApplicationContext`（容器）接口中的`close()`方法

    * 注册关闭钩子（函数方法），在java虚拟机退出前先关闭容器再推出虚拟机

      调用`ConfigurableApplicationContext`的`registerShutdownHook()`方法



### 6. DI 依赖注入

向一个类中传递数据的方式：普通方法（set），构造方法

此时这个数据可以为 **一个对象**

依赖注入描述了容器建立bean与bean之间的依赖关系的过程

原理也就是利用Java的反射机制，在加载时去调用set方法或构造方法为这个对象注入另一个所需对象。调用由Spring去调用。

但如果bean运行需要的是数字或者是字符串时，则原来的无参方法不行了

所以分了依赖注入的几种方式

#### 6.1 setter注入-引用类型

* 在bean的定义中定义引用类型属性成员，并提供可访问的set方法

  ```java
  private BookDao bookDao;
  public void setBookDao(BookDao bookDao) {
      this.bookDao = bookDao;
  }
  ```

* 配置中在bean标签中使用property标签中的ref属性，注入具体的引用类型的对象类型，是容器中的哪一个bean，也就是那一个bean的id，name属性表示 选择具体的 要绑定的 是 需要注入的类中的哪个成员变量对象 的名称

  ```xml
  <bean class="com.wyh.service.impl.BookServiceImpl" id="bookService">
          <property name="bookDao" ref="bookDao"/>
  </bean>
  ```

  注：可配置多个property标签，内置多个引用类型

#### 6.2 setter注入-简单类型

* 在bean的定义中定义简单类型的引用，并提供可访问的set方法

  ```java
  private int connectionNum;
  public void setconnectionNum(int connectionNum) {
      this.connectionNum = connectionNum;
  }
  ```

* 配置bean标签中使用property标签，给value属性设置简单类型的数据

  ```xml
  <bean class="com.wyh.service.impl.BookServiceImpl" id="bookService">
          <property name="connectionNum" 
                    value="10"/>
  </bean>
  ```

  Spring会自动给你转换数据类型

#### 6.3 构造器注入-引用类型（了解）

* 在bean的定义中定义引用类型属性成员，并提供可访问的构造器方法

  ```java
  private BookDao bookDao;
  public BookServiceImpl(BookDao bookDao) {
      this.bookDao = bookDao;
  }
  ```

* 配置中在bean标签中使用**constructor-arg**标签中的ref属性注入引用类型的对象，name为构造器的形参名称

  ```xml
  <bean class="com.wyh.service.impl.BookServiceImpl" id="bookService">
          <constructor-arg name="bookDao" ref="bookDao"/>
  </bean>
  ```

  注：可配置多个constructor-arg标签，内置多个构造器参数

#### 6.4 构造器注入-简单类型（了解）

* 在bean的定义中定义简单类型属性成员，并提供可访问的构造器方法

  ```java
  private int connectionNum;
  public BookServiceImpl(int connectionNum) {
      this.connectionNum = connectionNum;
  }
  ```

* 配置中在bean标签中使用**constructor-arg**标签中的value属性注入简单类型的值，name为构造器的形参名称

  ```xml
  <bean class="com.wyh.service.impl.BookServiceImpl" id="bookService">
          <constructor-arg name="connectionNum" value="10"/>
  </bean>
  ```

##### 6.4.1 构造器注入的参数适配（了解）

* 配置中使用constructor-arg标签的type属性设置按形参类型注入

  ```xml
  <bean class="com.wyh.service.impl.BookServiceImpl" id="bookService">
          <constructor-arg type="int" value="10"/>
          <constructor-arg type="java.lang.String" value="mysql"/>
  </bean>
  ```

* 配置中使用constructor-arg标签的index属性设置按形参位置注入

  ```xml
  <bean class="com.wyh.service.impl.BookServiceImpl" id="bookService">
          <constructor-arg index="0" value="10"/>
          <constructor-arg index="1" value="mysql"/>
  </bean>
  ```

#### 6.5 依赖注入的方式选择

1. 强制依赖 使用构造器进行，使用setter注入会有概率不进行注入导致null对象出现
2. 可选依赖使用setter注入进行，灵活性强
3. Spring框架倡导使用构造器注入，因为严谨
4. 有必要时可以两种一起搭配使用
5. 实际开发要根据实际情况分析，没有setter就必须用构造器
6. **自己开发的模块推荐用setter注入**

#### 6.6 依赖的自动装配

* 配置中使用bean标签的**autowire**属性设置自动装配的类型，要提供set方法，替换掉之前的property标签

  ```xml
  <bean id="bookDao" class="com.wyh.dao.impl.BookDaoImpl"/>
  <bean id="bookService" class="com.wyh.service.impl.BookServiceImpl"
        autowire="byType"/>
  ```

  * autowire取值：
    * byType：按类型
    * byName：按名称（setXxx方法中的Xxx）
    * default：默认
    * no：不自动装配
  * 注：按类型时，如果这个类型是别的Bean，那么需要在容器中先配置这个bean，不然找不到

* 自动装配特征：

  * 自动装配用于引用类型的注入，不能对简单类型进行操作
  * 使用byType时，必须保证容器中bean类型要唯一，只有一个，且要有对应的set方法，一般推荐使用这个
  * 使用byName时，要保证容器内要有指定名称的bean，由于变量名与配置耦合，所以不推荐
  * 自动装配优先级低于setter注入与构造器注入

#### 6.7 集合的注入

直接在property标签内部加上对应的标签即可，且很少用来注入引用类型，一般都是注入简单类型

1. 注入数组对象

   ```xml
   <property name="array"
             <array>
   			<value>100</value>
   			<value>10</value>
   		</array>
   </property>
   ```

2. 注入List对象（用的多一丢丢）

   ```xml
   <property name="list"
             <list>
   			<value>100</value>
   			<value>10</value>
   		</list>
   </property>
   ```

3. 注入Set对象

   ```xml
   <property name="set"
             <set>
   			<value>100</value>
   			<value>10</value>
   		</set>
   </property>
   ```

4. 注入map对象

   ```xml
   <property name="map"
             <map>
   			<entry key="contry" value="China"/>
   		</map>
   </property>
   ```

5. 注入properties对象

   ```xml
   <property name="properties"
             <props>
   			<prop key="contry">China</prop>
   		</props>
   </property>
   ```



### 7. Spring管理第三方资源Bean案例

如：Druid（阿里巴巴创建的数据库连接池）

* 首先导入Druid的坐标

  ```xml
  <dependency>
              <groupId>com.alibaba</groupId>
              <artifactId>druid</artifactId>
              <version>1.2.11</version>
  </dependency>
  ```

* 然后在applicationContext.xml配置文件中配置所要管理的Druid的类

  ```xml
  <bean id="dataSources" class="com.alibaba.druid.pool.DruidDataSource">
          <property name="driverClassName" value="com.mysql.cj.jdbc.Driver"/>
          <property name="url" value="jdbc:mysql://localhost:3306/db1"/>
          <property name="username" value="root"/>
          <property name="password" value="****"/>
  </bean>
  ```

* 然后根据id来创建连接池对象即可

这样的方式来管理不是自己写的第三方Bean，可能有些相关信息是需要在搜索引擎中自己查询的，比如c3p0（也是一个数据库连接池），但发现，这里面用setter注入的数据，大部分是不能直接写在程序中的。

#### 7.1 加载properties配置文件信息

* 开启context命名空间

  ```xml
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                             http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd">
  ```

* 使用context命名空间，加载指定的properties文件

  ```xml
  <context:property-placeholder location="jdbc.properties"/>
  ```

* 使用${}（读取占位符）读取加载配置文件里面的属性值

  ```xml
  <property name="driverClassName" value="${jdbc.driver}"/>
  ```

* 注：

  * 不加载系统的环境变量：

    ```xml
    <context:property-placeholder location="jdbc.properties" system-properties-mode="NEVER"/>
    ```

  * 加载多个properties文件

    ```xml
    <context:property-placeholder location="jdbc.properties，msg.properties"/>
    ```

  * 加载所有properties文件

    ```xml
    <context:property-placeholder location="*.properties"/>
    ```

  * 加载properties文件的标准格式

    ```xml
    <context:property-placeholder location="classpath:*.properties"/>
    ```

  * 从类路径或jar包中搜索并加载properties文件

    ```xml
    <context:property-placeholder location="classpath*:*.properties"/>
    ```



### 8. IoC容器

#### 8.1 创建容器

* 类路径加载配置文件（相对路径）

  ```java
  ApplicationContext ctx = new ClassPathXMLApplicationContext("applicationContext.xml");
  ```

* 文件路径加载配置文件（绝对路径）

  ```java
  ApplicationContext ctx = new FileSystemXMLApplicationContext("D:\\applicationContext.xml");
  ```

* 以上两种都可以加载多个配置文件，相当于合并在了一起

  ```java
  ApplicationContext ctx = new ClassPathXMLApplicationContext("bean1.xml"，"bean2.xml");
  ```

#### 8.2 获取bean

* 使用bean的id名获取

  ```java
  BookDao bookDao = (BookDao) ctx.getBean("bookDao");
  ```

* 使用bean的id名称并指定其类型

  ```java
  BookDao bookDao = ctx.getBean("bookDao",BookDao.class);
  ```

* 使用bean的类型自动寻找

  ```java
  BookDao bookDao = ctx.getBean(BookDao.class);
  ```

  但是这个需要你的bean类型要唯一不能有多个

#### 8.3 容器类层次结构

* **最顶层接口**：BeanFactory
* 常用接口：**ApplicationContext**
* 提供关闭功能的接口：ConfigurableApplicationContext
* 常用实现类：**ClassPathXmlApplicationContext**

##### 8.3.1 BeanFactory的初始化

* 类路径加载配置文件

  ```java
  Resource resources = new ClassPathResource("applicationContext.xml");
  BeanFactory bf = new XmlBeanFactory(resources);
  BookDao bookDao = bf.getBean("bookDao",BookDao,class);
  bookDao.save();
  ```

  BeanFactory创建完毕后，所有的bean都是**延迟加载**，不会执行它的构造方法，ApplicationContext是Spring的核心接口，最常用，且为**立即加载**。



### 9. 注解开发

Spring2.0开始提供的方法，简化开发

#### 9.1 注解开发定义Bean

* 使用@Component定义bean(component-组件)

  ```java
  @Component("bookDao")//给bean起了id名称
  public class BookDaoImpl implements BookDap{}
  @Component//未起id名
  public class BookServiceImpl implements BookService{}
  ```

* 核心文件applicationContext.xml中通过组件扫描加载bean

  ```xml
  <context:component-scan base-package="com.wyh">
  ```

* 注：未起id名，则在getBean时，用类型去获取，且这个组件扫描是在context命名空间里面的

* Spring提供@Component注解的三个衍生注解：

  * **@Controller**：用于表现层bean定义
  * **@Service**：用于业务层bean定义
  * **@Repository**：用于数据层bean定义
  * 注：功能与@Component一样，放便于我们理解

#### 9.2 纯注解开发

Spring3.0开始升级了纯注解开发模式，使用java类替代配置文件。

* java类代替Spring的核心配置文件applicationContext.xml，建议新创建一个包叫config专门放置配置类

  ```java
  @Configuration//这是替换的所有的基础配置
  @ComponentScan("com.wyh")//这是替换的组件扫描
  public class SpringConfig{}
  ```

  注：**@Configuration**注解用于设定当前类为配置类

  ​		**@ComponentScan**注解用于设定扫描路径，此注解只能添加一次，多个包需要扫描，则用字符串数组格式

  如：

  ```java
  @ComponentScan({"com.wyh.service","com.wyh.dao"})
  ```

* 读取Spring核心配置文件初始化容器对象切换为**读取Java配置类初始化容器对象**

  ```java
  ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
  ```

#### 9.3 bean管理

##### 9.3.1 bean作用范围

* 使用@Scope定义bean的作用范围，是单例还是非单例

  ```java
  @Repository
  @Scope("singleton")
  public class BookDaoImpl implements BookDao{}
  ```

##### 9.3.2 bean生命周期

* 使用@PostConstruct、@PreDestroy定义bean的生命周期

  ```java
  @Repository
  @Scope("singleton")
  public class BookServiceImpl implements BookService{
      public BookServiceImpl(){}
      @PostConstruct
      public void init(){}
      @PreDestroy
      public void destroy(){}
  }
  ```

#### 9.4 依赖注入

纯注解开发的依赖注入为自动装配

* 使用@Autowired注解开启自动装配（默认按类型）

  ```java
  @Service
  public class BookServiceImpl implements BookService {
      @Autowired
      private BookDao bookDao;
      public void save() {}
  }
  ```

  * 自动装配基于反射原理。创建对象时暴力反射对应的私有属性并初始化数据，因此无需提供setter方法
  * 自动装配建议使用无参的构造方法，这是默认的，且无参构造要唯一

* 使用@Qualifier注解开启按指定名称装配bean

  ```java
  @Service
  public class BookServiceImpl implements BookService {
      @Autowired
      @Qualifier("bookDao")
      private BookDao bookDao;
      public void save() {}
  }
  ```

  注：@Qualifier注解无法单独使用，必须配合@Autowired注解使用

* 使用@Value注解实现简单类型的注入

  ```java
  @Repository("bookDao")
  public class BookDaoImpl implements BookDao {
      @Value("wyh666")
      private String name;
  }
  ```

  注：注入简单类型时，可不用再加@Autowired，一般这样的格式，都是为了配合外部的properties文件使用的

* 在配置类上使用**@PropertySource**注解加载properties文件

  ```java
  @Configuration
  @ComponentScan("com.wyh")
  @PropertySource("classpath:jdbc.properties")
  public class SpringConfig{}
  ...
      
  @Value("${name}")
  private String name;
  ```

  注：加载的文件可以是多个，但要用{}包裹起来，为字符串数组。且此处**不可使用通配符***。注入的数据用${}包裹。

#### 9.5 注解开发管理第三方bean

例如管理Druid

* 使用**@Bean**配置第三方的bean

  ```java
  @Configuration
  public class SpringConfig{
      @Bean
      public DataSource dataSource() {
          DruidDataSource ds = new DruidDataSource();
          ds.setDriverClassName("com.mysql.cj.jdbc.Driver");
          ds.setUrl("jdbc:mysql://localhost:3306/db1");
          ds.setUsername("root");
          ds.setPassword("xxxx");
          return ds;
      }
  }
  ```

  注：@Bean是给这个方法定义，返回值是一个bean。这是一个工厂模式。不推荐直接把这个方法写在SpringConfig配置类中，很冗杂。

* 所以推荐使用**独立的配置类管理第三方bean**

  ```java
  public class JdbcDruidConfig{
      @Bean
      public DataSource dataSource() {
          DruidDataSource ds = new DruidDataSource();
          ds.setDriverClassName("com.mysql.cj.jdbc.Driver");
          ds.setUrl("jdbc:mysql://localhost:3306/db1");
          ds.setUsername("root");
          ds.setPassword("xxxx");
          return ds;
      }
  }
  ```

  * 然后将独立的配置类加入到核心配置类SpringConfig中

    * 方式一：导入式。使用@Import注解手动加入到核心配置类，此注解只能写一次，所以**多个其他的配置类，要用数组格式**

      ```java
      @Configuration
      @Import(JdbcDruidConfig.class)
      public class SpringConfig{}
      ```

    * 方式二：扫描式。隐藏性强，不是很推荐。使用@ComponentScan注解扫描配置类所在的包（config），加载对应的配置类信息

      ```java
      @Configuration
      @ComponentScan({"com.wyh.config"})
      public class SpringConfig{}
      ```

      注：对应的配置类上要有对应的@Configuration注解，表示这是配置类

      ```java
      @Configuration//方式二必须要有这个，方式一没有
      public class JdbcDruidConfig{
          @Bean
          public DataSource dataSource() {
              DruidDataSource ds = new DruidDataSource();
              ds.setDriverClassName("com.mysql.cj.jdbc.Driver");
              ds.setUrl("jdbc:mysql://localhost:3306/db1");
              ds.setUsername("root");
              ds.setPassword("xxxx");
              return ds;
          }
      }
      ```

* 获取bean的方式为**按类型获取**：**getBean(DruidConfig.class)**，因为没有类似方法设置配置类的id名，也没办法按名称获取。

##### 9.5.1 第三方bean的依赖注入

* 简单类型依赖注入，用@Value注入

  ```java
  public class JdbcDruidConfig{
      @Value("com.mysql.cj.jdbc.Driver")
      private String driver;
      @Value("jdbc:mysql://localhost:3306/db1")
      private String url;
      @Value("root")
      private String username;
      @Value("xxxx")
      private String password;
      
      @Bean
      public DataSource dataSource() {
          DruidDataSource ds = new DruidDataSource();
          ds.setDriverClassName(driver);
          ds.setUrl(url);
          ds.setUsername(username);
          ds.setPassword(password);
          return ds;
      }
  }
  ```

* 引用类型依赖注入

  ```java
  public class JdbcDruidConfig{
      @Bean
      public DataSource dataSource(BookService bookService) {}
  }
  ```

  引用类型的依赖注入只需要为这个配置bean的获取方法**设置形参**即可，**Spring容器会根据类型自动装配对象**。



### 10. Spring整合MyBatis(重点)

曾经使用Mybatis时，是需要加载MyBatis的核心配置文件（mybatis-config.xml）的。

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--起别名-->
    <typeAliases>
        <package name="com.wyh.pojo"/>
    </typeAliases>

    <environments default="development">
        <environment id="development">
            <!--处理事务的类型-->
            <transactionManager type="JDBC"/>
            <!--数据源（数据库连接池）配置-->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql:///db1?useServerPrepStmts=true"/>
                <property name="username" value="root"/>
                <property name="password" value="xxxx"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <!--扫描mapper-->
        <package name="com.wyh.mapper"/>
    </mappers>
</configuration>
```

这很麻烦，每次都要复制粘贴这个文件。

现在Spring整合Mybatis了，就可以省去这一步。

* 首先，要省事，肯定要导入相应的坐标才行

  ```xml
          <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-context</artifactId>
              <version>5.2.20.RELEASE</version>
          </dependency>
          <dependency>
              <groupId>com.alibaba</groupId>
              <artifactId>druid</artifactId>
              <version>1.2.11</version>
          </dependency>
          <dependency>
              <groupId>org.mybatis</groupId>
              <artifactId>mybatis</artifactId>
              <version>3.5.6</version>
          </dependency>
          <dependency>
              <groupId>mysql</groupId>
              <artifactId>mysql-connector-java</artifactId>
              <version>8.0.30</version>
          </dependency>
          <dependency>
              <groupId>org.springframework</groupId>
              <artifactId>spring-jdbc</artifactId>
              <version>5.2.20.RELEASE</version>
          </dependency>
          <dependency>
              <groupId>org.mybatis</groupId>
              <artifactId>mybatis-spring</artifactId>
              <version>1.3.0</version>
          </dependency>
  ```

  后面两个坐标是关键，一个具有处理jdbc事务的能力，一个是mybatis整合Spring的jar包，里面有相关的重要接口和类。

* 然后，我们经过分析，mybatis里面需要放入IoC容器里面管理的bean，首先就有一个**SqlSessionFactory**。其次，我们是为了省去加载配置mybatis核心文件的步骤，那么我们需要把mybatis的核心配置文件转换成一个**配置类MyBatisConfig**。

  ```java
  public class MyBatisConfig {
      @Bean
      public SqlSessionFactoryBean sqlSessionFactory(DataSource dataSource) {
          SqlSessionFactoryBean ssfb = new SqlSessionFactoryBean();
          ssfb.setTypeAliasesPackage("com.wyh.pojo");
          ssfb.setDataSource(dataSource);
          return ssfb;
      }
  
      @Bean
      public MapperScannerConfigurer mapperScannerConfigurer() {
          MapperScannerConfigurer msc = new MapperScannerConfigurer();
          msc.setBasePackage("com.wyh.dao");
          return msc;
      }
  }
  ```

  在经过分析，我们可以知道，除了SqlSessionFactory之外，**SqlSession**（映射对应的mapper执行对应的SQL功能）也是需要的，正好MyBatis整合Spring的包中，有提供便捷的**SqlSessionFactoryBean**和**MapperScanerConfigurer**。这两个就是快速创建SqlSessionFactory和SqlSession的。所以将这两个都放入IoC容器。我们**只需为他们添加关键的属性**即可。**如需要的实体类包的位置，和mapper层的类所在的包的位置**。对于SqlSessionFactory，我们通过Mybatis的核心配置文件可以知道，它**还需要一个dataSource**，所以这是一个依赖bean，需要我们也放入IoC容器，我们选择阿里巴巴的Druid，配置一个**JdbcConfig配置类**，里面配置mybatis所需要的关键信息（如数据库用户名密码以及访问哪个表)这些信息可以放在一个外部文件**jdbc.properties**中，然后SpringConfig加载即可。最后这个数据源dataSource属于第三方Bean的引用依赖，我们直接把他当成SqlSessionFactory的获取方法的参数即可，Spring自动装配。

  ```java
  public class JdbcConfig {
      @Value("${jdbc.driver}")
      private String driver;
      @Value("${jdbc.url}")
      private String url;
      @Value("${jdbc.username}")
      private String username;
      @Value("${jdbc.password}")
      private String password;
  
      @Bean
      public DataSource dataSource() {
          DruidDataSource ds = new DruidDataSource();
          ds.setDriverClassName(driver);
          ds.setUrl(url);
          ds.setUsername(username);
          ds.setPassword(password);
          return ds;
      }
  }
  ```

  ```java
  @Configuration
  @ComponentScan("com.wyh")
  @PropertySource("classpath:jdbc.properties")
  @Import({JdbcConfig.class,MyBatisConfig.class})
  public class SpringConfig {
  }
  ```

* 这样也就整合完了Mybatis。然后我们就能和之前一样创建dao层，service层，dao层访问数据库，mapper接口里面写对应的注解类型的SQL，service层写业务逻辑。在此不再阐述。



### 11.Spring整合JUnit

* 使用Spring整合JUnit专用的类加载器

  ```java
  @RunWith(SpringJUnit4ClassRunner.class)
  @ContextConfiguration(classes = SpringConfig.class)
  public class TestAccountService {
      @Autowired
      private AccountService accountService;
  
      @Test
      public void testSelectById() {
          System.out.println(accountService.findById(1));
      }
  
      @Test
      public void testSelectAll() {
          System.out.println(accountService.findAll());
      }
  }
  ```

  **@RunWith**：加载Spring整合JUnit的专用类加载器 SpringJUnit4ClassRunner.class

  **@ContextConfiguration**：配置Spring的核心配置类，里面的属性名为**classes**

  需要测试哪个层，那么就把那个层的对象设为属性，让Spring自动装配（Autowired），然后去调用里面的功能去测试即可。**@Test**还是需要的



### 12. AOP 

* AOP：Aspect Oriented Programming 面向切面编程，是一种编程范式，指导开发者如何组织程序结构，是编程思想
* 作用：在**不惊动原始设计**的基础上，为其**进行功能增强**。
* Spring理念：无入侵式/无侵入式功能增强
* AOP是一个大的概念，很多地方都有
* 核心概念：
  * **连接点**（JoinPoint）：程序执行过程中的任意位置，可以是执行的方法，抛出异常，设置变量等
    * 而在Spring中，AOP的连接点是：**方法的执行**，**也就是所有的方法**
  * **切入点**（PointCut）：<u>匹配连接点</u>的式子
    * 在Spring中，AOP的切入点是：**一个切入点可以描述一个具体方法，也可以匹配多个方法**，就是**具体匹配的一个或多个连接点**，也就是**需要增强功能的方法**
      * 一个具体方法：com.wyh.dao包下的BookDao接口中的无形参无返回值的save方法
      * 多个方法：所有的save方法，所有get开头的方法，所有以Dao结尾的接口中的任意方法，所有带有一个参数的方法
      * 也就是**连接点们**
  * **通知**（Advice）：在切入点处执行的操作（共性功能）
    * 在Spring中，AOP的通知是：**以方法呈现的共性功能**
  * **通知类**：内部定义通知的类
  * **切面**（Aspect）：**描述通知与切入点的对应关系的东西**

#### 12.1 AOP入门案例

* 导入AOP相关坐标

  ```xml
  <dependency>
      <groupId>org.aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
      <version>1.9.4</version>
  </dependency>
  ```

  注：spring-context坐标里依赖了spring-aop坐标，所以不需要导

* 与之前一样要有Dao层类与实现，也就是要有连接点

* 然后定义通知类，内部定义通知（共性功能）

  ```java
  public class MyAdvice {
      public void method() {
          System.out.println(System.currentTimeMillis());
      }
  }
  ```

* 定义切入点

  ```java
  public class MyAdvice {
      public void method() {
          System.out.println(System.currentTimeMillis());
      }
  
      @Pointcut("execution(void com.wyh.dao.BookDao.update())")
      private void pointCutFunc() {}
  }
  ```

  注：切入点的定义，**依托于**一个不具有实际意义的方法，即一个**无参数无返回值无方法体逻辑的方法**，在上面添加**@Pointcut**注释，内填对需要连接的连接点的描述**execution（）**。说白了**这个注释才是切入点**，只是这个切入点不能单独存在，要依托于一个空壳方法

* **绑定切入点与通知的关系，并指定通知添加到原始连接点的具体执行位置**

  然后**定义这个通知类受Spring容器管理**（**@Component**），并**定义当前这个通知类为切面类**（**@Aspect**）

  ```java
  @Component
  @Aspect
  public class MyAdvice {
      @Before("pointCutFunc()")
      public void method() {
          System.out.println(System.currentTimeMillis());
      }
  
      @Pointcut("execution(void com.wyh.dao.BookDao.update())")
      private void pointCutFunc() {}
  }
  ```

* 开启Spring对AOP注解驱动支持（**@EnableAspectJAutoProxy**）

  ```java
  @Configuration
  @ComponentScan("com.wyh")
  @EnableAspectJAutoProxy
  public class SpringConfig {
  }
  ```

#### 12.2 AOP工作流程

1. Spring容器启动
2. 读取所有切面配置中被配置了的切入点
3. 初始化bean，判断bean对应的类中的方法（连接点）是否匹配到任意切入点
   * 如果匹配失败，那么**正常的创建bean对象**
   * 然后获取这个bean，调用方法并执行，完成操作
   * =====================================
   * 如果匹配成功，那么创建这个bean（原始对象）也就是（**目标对象**）的**代理对象**
   * 然后获取的bean是**代理对象**，**根据代理对象的运行模式**运行原式原始方法与增强内容，完成操作

* 目标对象（Target）：原始功能去掉共性功能能之后对应的类产生的对象，这种对象是无法直接完成最终工作
* 代理（Proxy）：目标对象无法直接完成工作，需要对其进行功能回填，通过原始对象的代理对象实现

#### 12.3 AOP切入点表达式

* 切入点：要进行增强的方法

* 切入点表达式：要进行增强的方法的描述方式

  ```java
  execution(void com.wyh.dao.BookDao.update())//接口中的
  或
  execution(void com.wyh.dao.impl.BookDaoImpl.update())//实现类中的
  ```

  * 切入点表达式标准格式：

    动作关键词（访问修饰符 返回值 包名.类名/接口名.方法名（参数）异常名）

    * 动作关键词：描述切入点的行为动作，一般几乎都是**execution**，**表示执行到指定切入点**
    * 访问修饰符：public等，**可省略**
    * 异常名：方法定义中抛出指定的异常，**可省略**

* 可以**使用通配符描述**切入点，快速描述

  * （*）：单个独立的任意符号，可以独立出现，可作为前缀或后缀的匹配符

    `execution(* com.wyh.*.BookDaoImpl.find*(*))`

    匹配com.wyh包下的**任意包下的**BookDaoImpl类或接口中的**所有以find开头**的**带有一个任意类型参数**的方法

  * （..）：多个连续的任意符号，可以独立出现，常用于简化包名与参数的书写

    `execution(User com..UserService.findById(..))`

    匹配com包下的**任意包中的**UserService类或接口中的**所有名为findById方法**，**参数任意**

  * （+）：专用于匹配子类类型

    `execution(* *..*Service+.*(..))`

* 书写技巧与规则：

  * 要按代码标准
  * **切入点通常描述接口**，不描述实现类，降低耦合
  * 一般都是public描述，也可省略
  * 返回值的书写，对于增删改查则精准书写匹配，查询则*快速描述
  * **常用*做单个包名的描述**，或者精准匹配
  * **接口或类名与模块相关的，采用*匹配**，如UserService等，写为\*Service，绑定业务层接口名
  * 方法名可以以动词开头，名词用*匹配，如getById，可以为getBy\*。selectAll就是selectAll
  * 通常不使用异常作为匹配

#### 12.4 AOP通知类型

 AOP通知描述了抽取的共性功能，根据共性功能抽取的位置不同，最终运行代码时要将其加入到合理的位置

AOP通知分为五类：

* **@Before**（前置通知）：通知方法在切入点方法前运行

  ```java
  @Before("pointCutFuncOnUpdate()")
  public void before() {
  	System.out.println("advice before...");
  }
  ```

* **@After**（后置通知）：通知方法在切入点方法后运行

  ```java
  @After("pointCutFuncOnUpdate()")
      public void after() {
          System.out.println("advice after...");
      }
  ```

* **@Around**（环绕通知）（**重点掌握**）：通知方法在切入点方法前后运行，自定义位置

  ```java
      @Around("pointCutFuncOnSelect()")
      public Object around(ProceedingJoinPoint pjp) throws Throwable {
          System.out.println("around before advice...");
          Object o = pjp.proceed();
          System.out.println("around after advice...");
          return o;
      }
  ```

  注：环绕通知执行时，**原始连接点的方法内部执行**由**ProceedingJoinPoint类**的**对象调用proceed方法**执行，这个类要作为这个通知的参数。

  **环绕通知，要有返回值**，哪怕是void类型，通配的返回值也要有，**一般返回值为Object**，如果连接点也有返回值，那么需要我们**手动返回这个返回值**，自己return，proceed方法有返回值，返回值就是原始连接点的执行后的返回值，我们**只需要返回这个procee的返回值即可**。**void则压根不会显示有返回值**，不用担心会出现null。同时**还需要我们抛出异常**，因为人家只是帮我们增强，不负责处理我们自己写的连接点的异常。

* @AfterReturning（返回后通知）（了解）：通知方法在切入点方法正常执行完后运行

  ```java
      @AfterReturning("pointCutFuncOnSelect()")
      public void afterReturning() {
          System.out.println("afterReturning advice...");
      }
  ```

* @AfterThrowing（抛出异常后通知）（了解）：通知方法在切入点方法抛出异常后运行

  ```java
      @AfterThrowing("pointCutFuncOnSelect()")
      public void afterThrowing() {
          System.out.println("afterThrowing advice...");
      }
  ```

这些注解内的属性名为value，可省略不写，值为定义的**切入点空方法壳名称**，或通知类名.空方法壳名

#### 12.5 案例：测量业务层接口万次执行效率

```java
@Around("pointcut()")
    public void  around(ProceedingJoinPoint pjp) throws Throwable {
        //获取执行时签名信息 -- 可获取执行的方法名 方法所属的类名 等等
        Signature signature = pjp.getSignature();

        long beginTime = new Date().getTime();
        for (int i = 0; i < 10000; i++) {
            pjp.proceed();
        }
        long endTime = new Date().getTime();
        long time = endTime - beginTime;
        System.out.println("执行一万次"+signature.getDeclaringTypeName()+"."+signature.getName()+"经历了:" + time + "ms");
        //return pjp.proceed();
    }
```

#### 12.6 AOP通知获取数据

* 获取切入点方法的参数

  * JointPoint：适用于前置、后置、返回后、抛出异常后通知

  * **ProceedJointPoint**：适用于**环绕通知**

    * JoinPoint对象描述了连接点方法的运行状态，可以获取到原始连接点的调用参数

      ```java
      @Before("pt()")
      public void before(JoinPoint jp) {
          Object[] args = jp.getArgs();
          System.out.println(Arrays.toString(args));
      }
      ```

    * ProceedJointPoint是JoinPoint的子类

      ```java
      @Around("pt()")
      public Object around(ProceedJointPoint pjp) {
          Object[] args = pjp.getArgs();
          args[0] = 666;
          Object o = pjp.proceed(args);
          System.out.println(Arrays.toString(args));
          return o;
      }
      ```

      注：以上两个参数都必须是各自方法中的**第一位**

* 获取切入点方法的返回值

  * 用于返回后通知

  * 或者**环绕通知**

    * 返回后通知用形参接收返回值

      ```java
      @AfterReturning(value = "pt()",returning = "ret")
      public void before(JoinPoint jp,Object ret) {
          System.out.println(ret);
      }
      ```

    * 环绕通知调用原始方法则就可以获取返回值

      ```java
      @Around("pt()")
      public Object around(ProceedJointPoint pjp) {
          Object[] args = pjp.getArgs();
          Object o = pjp.proceed(args);
          return o;
      }
      ```

      

* 获取切入点方法的运行异常信息(了解一下即可，在此不叙述了)

  * 用于抛出异常后通知
  * **环绕通知**

#### 12.7 案例：百度网盘密码数据兼容处理

不只是百度网盘，一些密码输入时，尾部多输入了空格，我们要做兼容处理，去掉这些多余空格

在业务方法执行前对所有输入的参数进行格式处理：**trim()**

使用处理后的参数再去调用原始方法->环绕通知



### 13.Spring事务

* 事务作用：在数据层保障一系列的数据库操作的同成功同失败

* Spring事务作用：在数据层或**业务层**保障一系列的数据库操作的同成功同失败

  ```java
  public interface PlatformTransactionManager{
      void commit(TransactionStatus status) trows TransactionException;
      void rollback(TransactionStatus status) trows TransactionException;
  }
  ```

  ```java
  public class DataSourceTransactionManager{}
  ```

* 事务案例：银行转账

  实现两个账户之间的转账操作

  也就是A减钱，B加钱。

  案例实现之后。发现一件事，当在两个独立的操作（A减钱，B加钱）之间若是出现了异常。使得之后的操作（B加钱）未成功，整体的业务就会失败，那就等着坐牢吧。

  所以，Spring的事务可以帮我们解决这一问题，避免吃牢饭。

1. 在业务层上添加Spring的事务管理

   这是一个注解式事务

   **@Transactional**：通常添加在业务层接口中，不会添加在业务层实现类中，这是降低耦合。同时可以添加到业务方法上表示当前方法开启事务，也可以添加到接口上，表示当前接口所有方法都开启事务。

   ```java
   //@Transactional
   public interface AccountService {
       @Transactional
       public void transfer(){}
   }
   ```

2. 设置事务管理器

   事务管理器要根据实现技术进行选择，如MyBatis框架使用的是JDBC的事务。

   那么操作数据库的这些事务应当为JDBC事务，那么应该在MyBatis配置类中添加一个Bean

   ```java
   @Bean
   public PlatformTransactionManager transactionManager(DataSource dataSource) {
       DataSourceTransactionManager ptm = new DataSourceTransactionManager();
       ptm.setDataSource(dateSource);
       return ptm;
   }
   ```

   这个**PlatformTransactionManager**是Spring提供的接口，**DataSourceTransactionManager**是Spring提供的实现类。DataSourceTransactionManager是实现JDBC事务的专用事务管理器，以后使用其他的事务管理器，只需要更改DataSourceTransactionManager，PlatformTransactionManager不需要更改。DataSourceTransactionManager还需要我们给它注入数据源**DataSource**，设为形参即可。

3. 开启注解式事务驱动

   也就是直接在Spring的核心配置类上加上注解**@EnableTransactionManagement**

   ```java
   @Configuration
   @ComponentScan("com.wyh")
   @PropertySource("classpath:jdbc.properties")
   @Import({MyBatisConfig.class,DruidConfig.class})
   @EnableTransactionManagement
   public class SpringConfig {
   }
   ```


#### 13.1 Spring事务角色

一个调用数据库的dao层操作就会开启一个事务。

**各个不同操作之间也就是开启了不同的事务**，所以当一系列操作中出现了异常，无法一起回滚数据。

所以Spring的事务管理提出了解决：

由Spring开启一个事务，其他的操作事务加入到这个事务中，Spring开启的这个事务叫事务管理员，而加入到这个Spring大事务的这些操作事务叫事务协调员。

* 事务管理员：发起事务的一方。在Spring中通常指代业务层开启事务的方法。也就是有着@Transactional注解的方法。
* 事务协调员：加入事务的一方。在Spring中通常指代数据层方法。也可以是业务层方法。**也就是可以是另一个事务管理员**。

注：DataSourceTransactionManager注入的dataSource与我们MyBatis使用的dataSource是一致的，都是JDBC事务的。

#### 13.2 Spring事务的相关配置

属性：

* readOnly：设置是否为只读事务。（readOnly=true 只读事务）

* timeout：设置事务超时时间。（timeout=-1 永不超时）

* **rollbackFor**：**设置事务回滚异常**。（rollbackFor={NullPointException.class}）这里填写的是类的字节码。因为有些异常他是不会回滚的，需要自己手动设置。

* rollbackForName：设置事务回滚异常。（这里与上面一样，只不过这里写的是字符串格式）

* noRollbackFor：设置事务不回滚异常。（noRollbackFor=NullPointException.class）

* **propagation**：**设置事务传播行为**。（propagation=Propagation.REQUIRES_NEW）

  * 传播属性取值：
    * **REQUIRED（默认）**：设置这个值，事务管理员有事务T，事务协调员的事务则加入这个T。若事务管理员没事务，则事务协调员的事务自成一个事务T2。（也就是所有事务**都会成为一个事务**）
    * **REQUIRES_NEW**：设置这个值，事务管理员有一个事务T。而事务协调员则会开启一个新的事务T2，自成一个另外的事务。若事务管理员没事务，那么事务协调员的事务也会成为一个新的事务。（也就是设置这个值之后，**不管事务管理员有没有事务，事务协调员的事务都和事务管理员的事务没有关系**）
    * SUPPORTS：设置这个值，事务管理员有事务那么事务协调员的事务就加入这个事务。若事务管理员没事务，那么事务协调员也没有事务了。（也就是这是支持事务模式，有就加入事务管理员事务，没有则不成立事务）
    * NOT_SUPPORTED：设置这个值，事务管理员有事务但事务协调员的事务也不加入这个事务，且不会成为事务。若事务管理员没事务，那么事务协调员也没有事务。（也就是这是不支持事务模式，有也不加入且不成为事务，没有就没有）
    * MANDATORY：设置这个值，事务管理员有事务那么事务协调员的事务就加入这个事务。若事务管理员没事务，那么会报错。（也就是这是**必须有事务模式**，有就加入事务管理员事务，没有则报错）
    * NEVER：设置这个值，事务管理员有事务就会报错。若事务管理员没事务，那么事务协调员也没有事务。（也就是这是**必须没有事务模式**，没有就没有，有就会报错）

  * 案例：转账业务追加日志

    * 需求：实现任意两个账户之间的转账操作，并要求对每次转账操作在数据库进行留痕。

    * 分析：

      1. 基于转账操作案例添加日志，实现数据库中记录日志
      2. 业务层转账操作（transfer），调用减钱加钱与记录日志留痕。

    * 实现效果预期：无论转账操作是否成功，均进行转账操作的日志留痕

    * 存在的问题：日志记录与转账操作隶属于同一个事物，同成功同失败。

    * **事务传播行为**：事务协调员对事务管理员所携带事务的处理态度。

    * 一些实现细节：

      * 这里的日志选择成为一张新表（tb_logForAccount），且这是一个业务操作（LogService）

      * try-finall语句，finally必定执行。但报了异常就不会。

      * 一个业务层接口的方法（LogService的log）可以为一个事务管理员，也可以是另一个事务管理员中的事务协调员

      * 将当前时间格式转为自己想要的（如yyyy-mm-dd HH：mm：ss）字符串的实现语句：

        ```java
        String date = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss").format(new Date());
        ```

      * SQL语句中的VALUES中，填值除了#{date}，还可以填**now()**，获取当前时分秒时间。牛逼，新学一招。



## 二.SpringMVC

### 1. SpringMVC简介

**SpringMVC隶属于Spring技术**，是Spring技术的一部分。这里将其单独抽取出来做深度探讨。

* 概述：SpringMVC技术与Servlet技术功能相同，均属于web层开发技术。

  也就是简化Servlet的开发代码量，并且让其降低维护成本。（比如JavaWeb课程的最后一个大案例**JavaWeb_FinalCase**，里面的代码优化阶段的思想就是SpringMVC的模拟实现）

  是一种**基于Java实现的MVC模型**的轻量级Web框架，用于表现层，**相当于替换了Servlet**。

* 优点：

  * 使用性强，开发相较于Servlet简便
  * 灵活性强

#### 1.1 SpringMVC入门案例

1. 要使用那肯定是要先导入坐标了啊

   **springmvc坐标** 与 servlet坐标（要设置依赖范围为provided）

   spring-webmvc这个依赖中还依赖了spring的其他六个核心依赖，所以不用再导springframework的坐标了

   ```xml
   <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-webmvc</artifactId>
         <version>5.2.10.RELEASE</version>
   </dependency>
   <dependency>
         <groupId>javax.servlet</groupId>
         <artifactId>javax.servlet-api</artifactId>
         <version>3.1.0</version>
         <scope>provided</scope>
   </dependency>
   ```

2. 创建SpringMVC的表现层的控制类（也就是以前的servlet类，功能相同）

   ```java
   @Controller//将它定义为Spring的Bean，表现层用@Controller
   public class UserController {
       //设置当前操作的访问路径
       @RequestMapping("/save")
       //设置当前操作的返回值类型为响应体
       @ResponseBody()
       public String save() {
           System.out.println("userController.save is running...");
           return "{'module': 'springmvc'}";
       }
   }
   ```

3. SpringMVC技术属于Spring技术，还得加载Bean，所以要初始化SpringMVC的环境（同Spring环境），设定SpringMVC加载对应的bean

   这也就是代替了之前的Spring的核心配置类SpringConfig

   ```java
   @Configuration
   @ComponentScan("com.wyh.controller")
   public class SpringmvcConfig {
   }
   ```

4. 同时你要让web服务器知道你SpringMVC的配置，所以初始化Servlet容器（web服务器），加载SpringMVC环境，并设置SpringMVC技术处理的请求

   ```java
   public class TomcatConfig extends AbstractDispatcherServletInitializer {
       //加载SpringMVC容器的配置
       @Override
       protected WebApplicationContext createServletApplicationContext() {
           AnnotationConfigWebApplicationContext applicationContext = new AnnotationConfigWebApplicationContext();
           applicationContext.register(SpringmvcConfig.class);//注册自己刚刚配置的SpringMVC环境
           return applicationContext;
       }
   
       //设置哪些请求归SpringMVC处理
       @Override
       protected String[] getServletMappings() {
           return new String[]{"/"};//这个格式是管理所有请求
       }
   
       //加载Spring容器的配置
       @Override
       protected WebApplicationContext createRootApplicationContext() {
           return null;
       }
   }
   ```

* **@Controller**：设定SpringMVC的核心控制器bean（也就是相当于之前的**@Repository**和**@Service**）

  ```
  @Controller//将它定义为Spring的Bean，表现层用@Controller
  public class UserController {
  }
  ```

* **@RequestMapping**：**设置**当前控制器的某方法的**访问请求路径**（requestmapping翻译过来就是路径）

  * 相关属性：value（默认）= 请求访问路径

  ```java
  @RequestMapping("/save")
  public String save() {
  }
  ```

* **@ResponseBody**：**设置**当前控制器的某方法**响应内容为当前方法返回值**，**无需解析**

  ```java
  @RequestMapping("/save")
  //设置当前操作的返回值类型为响应体
  @ResponseBody
  public String save() {
     System.out.println("userController.save is running...");
     return "{'module': 'springmvc'}";
  }
  ```

* SpringMVC的开发：

  * 一次性工作：
    * 创建工程，设置服务器，加载工程
    * 导入坐标
    * 创建web容器启动类，加载SpringMVC配置，并设置SpringMVC的请求拦截路径
    * SpringMVC核心配置类（设置配置类，扫描controller包，加载Controller的bean）
  * 多次工作：
    * **定义处理请求的控制器类**
    * 定义控制请求的的控制器方法，并配置映射路径（@RequestMapping）和返回JSON数据（@ResponseBody）

* **AbstractDispatcherServletInitializer**类是SpringMVC提供的快速初始化Web3.0容器的抽象类

  * 需要我们**自定义Web容器配置类并继承这个类**，且**重写实现三个方法**

    * **createServletApplicationContext()**方法，创建Servlet容器时，加载SpringMVC对应的bean并放入WebApplicationContext对象的范围中，而WebApplicationContext的作用范围为ServletContext范围，继而为整个web容器范围。

      ```java
      protected WebApplicationContext createServletApplicationContext() {
              AnnotationConfigWebApplicationContext applicationContext = new AnnotationConfigWebApplicationContext();
              applicationContext.register(SpringmvcConfig.class);//注册自己刚刚配置的SpringMVC环境
              return applicationContext;
          }
      ```

    * **getServletMappings()**方法，设定SpringMVC对应的请求映射路径，设置为‘/’，表示拦截所有的请求，任意的请求都将转入到SpringMVC进行处理。

      ```java
          @Override
          protected String[] getServletMappings() {
              return new String[]{"/"};//这个格式是管理所有请求
          }
      ```

    * **createRootApplicationContext()**方法，如果创建的Servlet容器需要加载非SpringMVC对应的bean，也就是Spring中的bean时，使用当前这个方法进行，使用方式与**createServletApplicationContext()**方法一致。

      ```java
      @Override
          protected WebApplicationContext createRootApplicationContext() {
              return null;
          }
      ```


#### 1.2 工作流程分析

* 启动服务器初始化的过程
  1. 服务器（Tomcat）启动，执行**继承AbstractDispatcherServletInitializer**的TomcatConfig类，初始化web容器
  2. 执行里面的**createServletApplicationContext()**方法，创建WebApplicationContext对象，放入到了ServletContext中
  3. 执行ServletApplicationContext.**register(SpringmvcConfig.class)**，加载SpringmvcConfig类
  4. 执行@ComponentScan加载对应的bean（各个@Controller）
  5. 加载UserController，每个@RequestMapping的内部值对应了一个具体的方法
  6. 执行**getServletMappings()**方法，定义所有的请求都通过SpringMVC
* 单次请求过程
  1. 发送请求/localhost/save
  2. web容器发现所有的请求都要经过SpringMVC，将请求交给了SpringMVC
  3. SpringMVC解析请求路径（/save）
  4. 知道了对应的方法是save()
  5. 执行save（）
  6. 检测到**@ResponseBody**，将save（）的返回值作为响应体响应返回给请求方

#### 1.3 Controller的加载控制与Service/Dao的加载控制

* SpringMVC控制表现层的bean（@Controller）

* Spring控制业务层与数据层bean（@Service/@Repository）

  * SpringMVC相关bean的加载控制：

    * 都放在com.wyh.controller包内即可

  * Spring相关bean的加载控制：

    * 方式一：Spring加载bean的设定扫描范围为com.wyh，要排除掉controller包的bean

      ```java
      @ComponentScan(value="com.wyh",excludeFilters = @ComponentScan.Filter(
              type = FilterType.ANNOTATION,
              classes = Controller.class
      ))
      //这种方式在SpringBoot中会见到，用来控制加载的bean的细粒度。
      ```

    * 方式二：Spring加载的bean设定扫描范围为精准范围。如指定service包，dao包。

      ```java
      @ComponentScan({"com.wyh.service","com.wyh.dao"})
      ```

    * 方式三：不区分Spring与SpringMVC的环境，加载到同一环境。

  * @ComponentScan的属性：

    * excludeFilters：排除扫描路径中加载的某bean，需要指定类别（type）和具体项（classes）
    * includeFilters：加载指定的bean，需要指定类别（type）和具体项（classes）

* 对于web服务器（servlet容器）的**完整开发**应该是**即配置了Spring容器，又配置了SpringMVC容器**

  * 完整开发：

    ```java
    public class TomcatConfig extends AbstractDispatcherServletInitializer {
        //加载SpringMVC容器的配置
        @Override
        protected WebApplicationContext createServletApplicationContext() {
            AnnotationConfigWebApplicationContext applicationContext = new AnnotationConfigWebApplicationContext();
            applicationContext.register(SpringmvcConfig.class);
            return applicationContext;
        }
    
        //设置哪些请求归SpringMVC处理
        @Override
        protected String[] getServletMappings() {
            return new String[]{"/"};//这个格式是管理所有请求
        }
    
        //加载Spring容器的配置
        @Override
        protected WebApplicationContext createRootApplicationContext() {
            AnnotationConfigWebApplicationContext applicationContext = new AnnotationConfigWebApplicationContext();
            applicationContext.register(SpringConfig.class);
            return applicationContext;
        }
    }
    ```

  * 简化开发（继承的换成了**AbstractAnnotationConfigDispatcherServletInitializer**）（掌握这个即可，但上面的原理要掌握）

    ```java
    public class TomcatConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
        @Override
        protected Class<?>[] getRootConfigClasses() {
            return new Class[]{SpringConfig.class};
        }
    
        @Override
        protected Class<?>[] getServletConfigClasses() {
            return new Class[]{SpringmvcConfig.class};
        }
    
        @Override
        protected String[] getServletMappings() {
            return new String[]{"/"};
        }
    }
    ```

#### 1.4 PostMan（邮差）

* 是一款功能强大（吹牛的）的网页调试与发送网页HTTP请求的Chrome插件
* 作用：常用于进行接口测试
* 我直接用的网页版postman.com
* 基本使用：
  * 注册登录
  * 创建工作空间/进入工作空间
  * 发起请求测试即可（还可以保存这次请求测试）



### 2. 请求与响应

#### 2.1 请求映射路径设置

* 团队多人开发，每人设置的请求路径应当不同，**设置模块名作为请求路径的前缀**解决冲突问题

* **@RequestMapping**：

  * 既可以是**方法注解**也可以是**类注解**

  * 属性为value，默认的

  * 设置当前控制器的方法的请求路径，如果设置在类上，那么则是设置当前控制器所有方法的请求路径前缀

    * 例如

      ```java
      @Controller
      @RequestMapping("/user")//这里作为所有方法的前缀，访问时要加上这个前缀
      public class UserController {
          @RequestMapping("/save")
          @ResponseBody
          public String save() {...}
      }
      ```

#### 2.2 请求方式

* GET请求

  * Get请求传参：

    * 普通参数：url地址传参，地址参数名与要与方法形参变量名相同，定义形参即可接收参数

      PostMan的请求路径：

      ```
      http://localhost:8080/SpringMVC/user/welcome?name=wyh&age=20
      ```

      ```java
      @RequestMapping("/welcome")
          @ResponseBody
          public String welcome(String name, String age) {
              System.out.println(name+"<===>"+age);
              return "<h1>Welcome to access this web page</h1>" +
                      "<p>welcome user "+name+"</p>" +
                      "<p>your age is "+age+"</p>";
          }
      ```

* POST请求

  * Post请求参数：
    * 普通参数：form表单post请求传参，表单参数名要与形参变量名相同，定义形参接收参数即可，又由于我们后端这个代码**不区分post和get**，比如javaWeb时，我们也是如果是post请求，直接调用get里面的方法即可。所以**定义一个操作方法带上参数，两个请求都可以使用**。在PostMan中发送post请求我们要在Body里选择x-www-form-urlencoded编写参数发送。

  * Post请求中文乱码处理:

    * 为Web容器添加过滤器并指定字符集，spring-web包中提供了专用的字符过滤器。直接在servlet容器（web服务器）中**重写getServletFilters()**方法，创建**CharacterEncodingFilter**对象，设置字符集为UTF-8，即可解决post的中文乱码问题。（get没有解决）

      ```java
      public class TomcatConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
      	...
      
          //解决post中文乱码
          @Override
          protected Filter[] getServletFilters() {
              CharacterEncodingFilter filter = new CharacterEncodingFilter();
              filter.setEncoding("UTF-8");
              return new Filter[]{filter};
          }
      }
      ```

      注：PostMan中，要重新设置请求头（header）的Accept字段，值为`text/plain;charset=UTF-8`，否则中文会显示???（取消勾选之前的Accept）



### 3. REST风格



### 4. SSM整合



### 5. 拦截器

## 三.Maven高级

## 四.SpringBoot

## 五.MybatisPlus