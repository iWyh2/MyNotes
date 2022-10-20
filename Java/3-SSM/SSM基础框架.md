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
  <bean class="com.wyh.dao.BookDao" id="bookDao"/>
  <bean class="com.wyh.service.impl.BookServiceImpl" id="bookService"/>
  ```

  * `<bean>`是指配置Bean
  * class属性是给Bean定义类型（类名）
  * id属性是给这个bean起名字（对象名）

* ```java
  //获取IoC容器
  ApplicationContext applicationContext = new ClassPathXmlApplicationContext("applicationContext.xml");
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

1. 删去上个IoC案例中的service层的，以new形式创建的Dao对象，使有关系的对象成为对象属性（只需要一个引用）

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

* property表示给当前的Bean**配置属性**，也就是**给这个类的对象绑定关系**
* name属性表示 选择具体的 要绑定的 是类中的哪个**成员变量对象** 的名称（创建的对象名）
* ref属性表示name选择的这个对象要参照的具体类型是什么，具体是哪一个bean，也就是另一个Bean的id属性（ref指向的bean需要容器里面有，然后根据bean的id查找）



### 5.Bean

#### 5.1 Bean的配置

##### 5.1.1 Bean的基础配置

* 名称：bean

* 类型：标签

* 所属：beans标签

* 功能：定义Spring的核心容器管理的**对象**

* 格式：

  ```xml
  <beans>
  	<bean/>
      <bean></bean>
  </beans>
  ```

* 属性列表：
  * id：bean的id，使用容器可以通过id值获取对应的bean，在一个容器中id值唯一
  * class：bean的类型，配置bean的**全路径类名**

##### 5.1.2 Bean的别名配置（了解）

* 属性名称：name

* 所属：bean标签

* 功能：定义bean的别名，可定以多个，用逗号，分号，空格分隔这些别名

* 例如：

  ```xml
  <bean class="com.wyh.service.impl.BookServiceImpl" id="bookService" name="service serviceEBi"/>
  ```

* 注意事项：获取bean时，无论是通过id还是name，如果无法获取，那么会抛出异常：**NoSuchBeanDefinitionException**，找不到这个名字的bean，也就是名字写错了

##### 5.1.3 Bean的作用范围

* 属性名称：scope

* 所属：bean标签

* 功能：定义bean的作用范围，也就是创建的bean的数目

  * 取值：
    * singleton：单例（默认）
    * prototype（单词意为 范例）：非单例

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

   也就是**必须要有无参构造器**在所管理的Bean的类中，必须是无参，**自定义无参或者默认无参都可以**

   如果无参构造器不存在，会报**BeanCreationException**

   构造方法由Spring来调用

2. 静态工厂（了解）

   创建对象的方式变成了创建一个工厂对象，调用工厂类的静态方法来获取需要的Bean

   ```java
   public class OrderDaoFactory {
       public static OrderDao getOrderDao() {
           return new OrderDao();
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
           return new UserDao();
       }
   }
   ```

   因为这个方法不是静态的，所以需要你实例化这个工厂才能调用这个获取对象方法去获取bean，也就是需要你先去将工厂配置为bean，同时配置也需要改变：

   ```xml
   先配置工厂为bean
   <bean
         id="userDaoFactory"
         class="com.wyh.factory.UserDaoFactory"/>
   再配置实例工厂方法
   <bean
         id="userDao"
         factory-bean="userDaoFactory"
         factory-method="getUserDao"/>
   ```

   也就是，需要先配置这个工厂为bean，然后在需要的bean内加上属性**factory-bean**，值为配置的工厂bean的id，然后再加上属性**factory-method**，取值为这个工厂内的获取bean的非静态方法

   但由此发现了很多鸡肋，比如配置了一个无用`<bean>`，后面的factory-bean和factory-method每次都要更改，不是很方便，由此，Spring对其优化，将这个实例工厂方法改良

4. **实例工厂—改良升级版（实用）**

   定义这个工厂类时，需要实现Spring提供的接口**FactoryBean**，且这是个**泛型**，要指定这个工厂获取的bean对象是哪一类，还要至少重写其里面的三个方法中的前两个，获取bean对象，获知bean对象类型，指定是否为单例创建（此方法可不重写，默认为true，也就是默认为单例创建，需要非单例时才重写并改为false）

   ```java
   public class UserDaoFactoryBean implements FactoryBean<UserDao> {
       public UserDao getObject() throws Exception {
           return new UserDao();
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

   配置文件的改变：从配置所需bean，变为了**直接配置这个创建的Factorybean**

   ```xml
   <bean
         id="userDao"
         class="com.wyh.factory.UserDaoFactoryBean"/>
   ```

   那么在IoC容器中获取bean对象时，getBean的值填这个FactoryBean的id值，**Spring就会去调用里面的getObject方法**，获取bean对象

#### 5.3 Bean的生命周期

bean的生命周期：bean从创建到销毁的整体过程

bean的生命周期的控制：在bean创建后到销毁前做的一些事情

* 自己提供生命周期的控制方法

  ```java
  public class BookDao {
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

* **bean生命周期：**

  * **初始化容器**
    1. **创建对象（分配内存）**
    2. **执行构造方法**
    3. **执行属性注入（set操作）**
    4. **执行bean初始化方法**
  * **使用bean**
    * **执行业务操作**
  * **关闭/销毁容器**
    * **执行bean的销毁方法**

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

原理也就是利用Java的反射机制，在加载时去调用set方法或构造方法为这个对象注入另一个所需对象。**调用由Spring去调用**。

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
          <property name="bookDao" ref="bookDao"/><!--配置依赖注入属性-->
  </bean>
  ```

  注：可配置多个property标签，内置多个引用类型
  
  * property表示给当前的Bean**配置属性**，也就是**给这个类的对象绑定关系**
  * name属性表示 选择具体的 要绑定的 是类中的哪个**成员变量对象** 的名称
  * **ref属性表示name选择的这个对象要参照的具体类型是什么，具体是哪一个bean，也就是另一个Bean的id属性**

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
          <property name="connectionNum" value="10"/>
  </bean>
  ```
  
  Spring会自动给你转换数据类型
  
  * **value：就是想要注入的简单类型的值**

【所以说，**引用类型用ref**，**简单类型用value**】

#### 6.3 构造器注入-引用类型（了解）

* 在bean的定义中定义引用类型属性成员，并提供可访问的构造器方法

  ```java
  private BookDao bookDao;
  public BookServiceImpl(BookDao bookDao) {
      this.bookDao = bookDao;
  }
  ```

* 配置中在bean标签中使用**constructor-arg**标签中的ref属性注入引用类型的对象（是一个bean的id），**name为构造器的形参名称**

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

【依然的，**引用类型为ref，简单类型为value**】

##### 6.4.1 构造器注入的参数适配（了解）

* 配置中使用constructor-arg标签的**type属性设置按形参类型注入**

  ```xml
  <bean class="com.wyh.service.impl.BookServiceImpl" id="bookService">
          <constructor-arg type="int" value="10"/>
          <constructor-arg type="java.lang.String" value="mysql"/>
  </bean>
  ```

* 配置中使用constructor-arg标签的**index属性设置按形参位置注入**

  ```xml
  <bean class="com.wyh.service.impl.BookServiceImpl" id="bookService">
          <constructor-arg index="0" value="10"/>
          <constructor-arg index="1" value="mysql"/>
  </bean>
  ```

#### 6.5 依赖注入的方式选择

1. 强制依赖使用构造器进行，使用setter注入会有概率不进行注入导致null对象出现
2. 可选依赖使用setter注入进行，灵活性强
3. Spring框架倡导使用构造器注入，因为严谨
4. 有必要时可以两种一起搭配使用
5. 实际开发要根据实际情况分析，没有setter就必须用构造器
6. **自己开发的模块推荐用setter注入**

#### 6.6 依赖的自动装配

* 配置中使用bean标签的**autowire**属性设置自动装配的类型，**要提供set方法，替换掉之前的property标签**，不用再配置property

  ```xml
  <bean id="bookDao" class="com.wyh.dao.impl.BookDaoImpl"/>
  <bean id="bookService" class="com.wyh.service.impl.BookServiceImpl" autowire="byType"/>
  ```
  
  * autowire取值：
    * **byType：按类型**
    * **byName：按名称（setXxx方法中的Xxx）**
    * default：默认
    * no：不自动装配
  * 注：**按类型时，如果这个类型是别的Bean，那么需要在容器中先配置这个bean，不然找不到**
  
* 自动装配特征：

  * **自动装配用于引用类型的注入，不能对简单类型进行操作**
  * **使用byType时，必须保证容器中bean类型要唯一，只有一个，且要有对应的set方法，一般推荐使用这个**
  * **使用byName时，要保证容器内要有指定名称的bean，由于变量名与配置耦合，所以不推荐**
  * **自动装配优先级低于setter注入与构造器注入**

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

这样的方式来管理不是自己写的第三方Bean，可能有些相关信息是需要在搜索引擎中自己查询的，比如c3p0（也是一个数据库连接池，一代数据库连接池产品，早已被淘汰，不再更新了），但发现，这里面用setter注入的数据，比如密码，大部分是不能直接写在程序的xml文件中的。所以将配置信息写在properties文件中

#### 7.1 加载properties配置文件信息

* **开启context命名空间**

  ```xml
  <beans xmlns="http://www.springframework.org/schema/beans"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xmlns:context="http://www.springframework.org/schema/context"<!--增加的命名空间-->
         xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
                             http://www.springframework.org/schema/context 							   			http://www.springframework.org/schema/context/spring-context.xsd">                                                           
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

  BeanFactory创建完毕后，所有的bean都是**延迟加载**，不会执行它的构造方法，也就是需要的时候再去创建对象，ApplicationContext是Spring的核心接口，最常用，且为**立即加载**。



### 9. 注解开发

Spring2.0开始提供的方法，简化开发

#### 9.1 注解开发定义Bean

就不需要在xml文件中定义bean标签了

* 使用@Component定义bean(component-组件)

  ```java
  @Component("bookDao")//给bean起了id名称
  public class BookDao {}
  @Component//未起id名
  public class BookServiceImpl implements BookService{}
  ```

* 核心文件applicationContext.xml中**通过组件扫描加载bean**

  ```xml
  <context:component-scan base-package="com.wyh">在新增的命名空间中 指定需要扫描的包
  ```

* 注：**未起id名，则在getBean时，用类型去获取**，且这个**组件扫描是在context命名空间里面的**

* Spring提供@Component注解的三个衍生注解：

  * **@Controller**：用于表现层bean定义
  * **@Service**：用于业务层bean定义
  * **@Repository**：用于数据层bean定义
  * 注1：**功能与@Component一样**，放便于我们理解
  * 注2：**@Service** 是把 spring 容器中的 bean 进行实例化，也就是**等同于 new 操作**，只有 实现类 是可以进行 new 实例化的，**而 接口 则不能**，所以是**加在 实现类上**

#### 9.2 纯注解开发

Spring3.0开始升级了**纯注解开发模式**，使用**java类替代配置文件**。

* **java类代替Spring的核心配置文件applicationContext.xml**，建议新创建一个包叫config专门放置配置类

  ```java
  @Configuration//这是替换的所有的基础配置
  @ComponentScan("com.wyh")//这是替换的组件扫描
  public class SpringConfig{}
  ```

  注：**@Configuration**注解用于设定当前类为**配置类**

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
  public class BookDao {}
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

  * **自动装配基于反射原理**。创建对象时暴力反射对应的私有属性并初始化数据，因此无需提供setter方法
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



### 10. Spring整合MyBatis(注解版)

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

  再经过分析，我们可以知道，除了SqlSessionFactory之外，**SqlSession**（映射对应的mapper执行对应的SQL功能）也是需要的，正好MyBatis整合Spring的包中，有提供便捷的**SqlSessionFactoryBean**和**MapperScanerConfigurer**。这两个就是快速创建SqlSessionFactory和SqlSession的。所以将这两个都放入IoC容器。我们**只需为他们添加关键的属性**即可。**如需要的实体类包的位置，和mapper层的类所在的包的位置**。对于SqlSessionFactory，我们通过Mybatis的核心配置文件可以知道，它**还需要一个dataSource**，所以这是一个依赖bean，需要我们也放入IoC容器，我们选择阿里巴巴的Druid，配置一个**JdbcConfig配置类**，里面配置mybatis所需要的关键信息（如数据库用户名密码以及访问哪个表)这些信息可以放在一个外部文件**jdbc.properties**中，然后SpringConfig加载即可。最后这个数据源dataSource属于第三方Bean的引用依赖，我们直接把他当成SqlSessionFactory的获取方法的参数即可，Spring自动装配。

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

* 这样也就整合完了Mybatis。然后我们就能和之前一样创建dao层，service层。dao层访问数据库，mapper接口里面写对应的注解类型的SQL，service层写业务逻辑。在此不再阐述。

* **Spring整合MyBatisXML配置版**请移步[MyBatisPlus.md]中查询



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

  **@RunWith**：加载Spring整合JUnit的专用类加载器 **SpringJUnit4ClassRunner.class**

  **@ContextConfiguration**：配置Spring的核心配置类，里面的属性名为**classes**

  需要测试哪个层，那么就把那个层的对象设为属性，让Spring自动装配（Autowired），然后去调用里面的功能去测试即可。**@Test**还是需要的



### 12. AOP 

* AOP：Aspect Oriented Programming 面向切面编程，是一种编程范式，指导开发者如何组织程序结构，是编程思想
* 作用：在**不惊动原始设计**的基础上，为其**进行功能增强**。
* Spring理念：无入侵式/无侵入式 功能增强
* AOP是一个大的概念，很多地方都有
* 核心概念：
  * **连接点**（JoinPoint）：程序执行过程中的任意位置，可以是执行的方法，抛出异常，设置变量等
    * 而在Spring中，AOP的连接点是：**方法的执行**，**也就是所有的方法**（方法就是连接点）
  * **切入点**（PointCut）：<u>匹配连接点</u>的式子（描述方法的式子就是切入点）
    * 在Spring中，AOP的切入点是：**一个切入点可以描述一个具体方法，也可以匹配多个方法**，就是**具体匹配的一个或多个连接点**，也就是**需要增强功能的方法**，是一个注解，注解内描述需要增强的连接点
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

* **开启Spring对AOP注解驱动支持**（**@EnableAspectJAutoProxy**）

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
   * 如果**匹配失败**，那么**正常的创建bean对象**
   * 然后获取这个bean，调用方法并执行，完成操作
   * =====================================
   * 如果**匹配成功**，那么创建这个bean（原始对象）也就是（**目标对象**）的**代理对象**
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

使用处理后的参数再去调用原始方法->**环绕通知**



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

一个调用数据库的dao层操作方法就会开启一个事务。

**各个不同操作方法之间也就是开启了不同的事务**，所以当一系列操作中出现了异常，无法一起回滚数据。

所以Spring的事务管理提出了解决：

由Spring开启一个事务，其他的操作事务加入到这个事务中，Spring开启的这个事务叫事务管理员，而加入到这个Spring大事务的这些操作事务叫事务协调员。

* **事务管理员**：发起事务的一方。在Spring中通常指代业务层开启事务的方法。也就是**有着@Transactional注解的方法**。
* **事务协调员**：加入事务的一方。在Spring中通常指代数据层方法。也可以是业务层方法。**也可以是另一个事务管理员**。

注：DataSourceTransactionManager注入的dataSource与我们MyBatis使用的dataSource是一致的，都是JDBC事务的。

#### 13.2 Spring事务的相关配置

@Transactional的属性：

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

    * 存在的问题：日志记录与转账操作隶属于同一个事务，同成功同失败。

    * **事务传播行为**：事务协调员对事务管理员所携带事务的处理态度。

    * 所以用@Transactional的属性Propagation的REQUIRES_NEW

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

   spring-webmvc这个依赖中还依赖了spring的其他六个核心依赖，所以不用再导springframework的spring-context坐标了

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

   这也就是代替了之前的Spring的核心配置类SpringConfig（这俩是可以同时存在的，且Spring容器是父类容器）

   ```java
   @Configuration
   @ComponentScan("com.wyh.controller")
   public class SpringMvcConfig {
   }
   ```

4. 同时你要让web服务器知道你SpringMVC的配置，所以初始化Servlet容器（web服务器），加载SpringMVC环境，并设置SpringMVC技术处理的请求

   ```java
   public class TomcatConfig extends AbstractDispatcherServletInitializer {
       //加载SpringMVC容器的配置
       @Override
       protected WebApplicationContext createServletApplicationContext() {
           AnnotationConfigWebApplicationContext applicationContext = new AnnotationConfigWebApplicationContext();
           applicationContext.register(SpringMvcConfig.class);//注册自己刚刚配置的SpringMVC环境
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

* **@Controller**：设定SpringMVC的核心控制器bean（也就是相当于之前的**@Repository**和**@Service**）为表现层bean

  ```java
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
              applicationContext.register(SpringMvcConfig.class);//注册自己刚刚配置的SpringMVC环境，创建SpringMVC容器
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
          protected WebApplicationContext createRootApplicationContext() {//创建Spring容器
              return null;
          }
      ```


#### 1.2 工作流程分析

* 启动服务器初始化的过程
  1. 服务器（Tomcat）启动，执行**继承AbstractDispatcherServletInitializer**的TomcatConfig类，初始化web容器
  2. 执行里面的**createServletApplicationContext()**方法，创建WebApplicationContext对象，放入到了ServletContext中
  3. 执行ServletApplicationContext.**register(SpringmvcConfig.class)**，加载SpringMvcConfig类
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
* 作用：常用于进行**表现层接口测试**
* 我直接用的网页版postman.com
* 基本使用：
  * 注册登录
  * 创建工作空间/进入工作空间
  * 发起请求测试即可（还可以保存这次请求测试）



### 2. 请求与响应

#### 2.1 请求

##### 2.1.1 请求映射路径设置

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

##### 2.1.2 请求方式

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
    * 普通参数：form表单post请求传参，表单参数名要与形参变量名相同，定义形参接收参数即可，又由于我们后端这个代码**不区分post和get**，比如javaWeb时，我们也是如果是post请求，直接调用get请求里面的方法即可。所以**定义一个操作方法带上参数，两个请求都可以使用**。在PostMan中发送post请求我们要在Body里选择x-www-form-urlencoded编写参数发送。

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

##### 2.1.3 请求参数

* Get请求

* 普通参数：url地址传参

  * 地址参数名与形参变量名相同，定义形参即可接收参数

  * 请求参数名与形参名不同时，使用@RequestParam绑定参数关系

    ```java
    @RequestMapping("/welcome")
    @ResponseBody
        public String welcome(@RequsetParam("name")String userName, String age) {
            System.out.println(name+"<===>"+age);
            return "<h1>Welcome to access this web page</h1>" +
                    "<p>welcome user "+name+"</p>" +
                    "<p>your age is "+age+"</p>";
        }
    ```

    * **@RequsetParam**：用于绑定请求参数与处理器方法形参之间的关系，写在形参前面
      * 参数属性：required（是否为必传参数）/defaultValue（参数默认值）

* POJO参数：请求参数名与形参对象的属性名相同，定义POJO类型的形参即可接收参数

  ```java
  public String pojoParam(User user) {}
  ```

  * POJO嵌套（POJO对象包含了另一个POJO对象）：请求参数名与形参对象的属性名相同，按照对象层次的结构关系传递参数（对象.属性名）这个对象的名称要与POJO类中的成员变量属性对象名一致。即可接收嵌套的POJO参数

    ```
    ?...&address.city=beijing&address.province=beijing
    ```

* 数组参数：请求参数名与形参数组参数名相同且请求个数为多个，定义一个数组形参，数组名与请求参数名一致，即可接收

  ```java
  public String arrayParam(String[] likes) {}
  ```

* 集合保存普通参数：请求参数名与形参集合对象名相同且请求个数为多个，用@RequestParam绑定参数关系，`不用会报错：NoSuchMethodException: java.util.List.<init>()`

  ```java
  public String listParam(@RequestParam List<String> likes) {}
  ```

* 总结：
  * 对于简单类型：SpringMVC直接将请求参数与形参匹配
  * 对于引用类型：SpringMVC将创建形参的引用类型对象，调用与请求参数匹配的get/set方法，没有用@RequestParam绑定的List集合也是如此，所以报错。
  * 对于List集合，要用@RequestParam绑定与请求参数的关系，使Spring将数据注入到集合中

* **传递JSON数据（重点）**

  * 添加JSON数据转换的相应坐标

    ```xml
        <dependency>
          <groupId>com.fasterxml.jackson.core</groupId>
          <artifactId>jackson-databind</artifactId>
          <version>2.9.0</version>
        </dependency>
    ```

  * 设置发送JSON格式数据（数组：[]，对象：{'': ''}）

    在PostMan中，为请求体中写JSON数据（不管是Get还是Post都可以写请求体）。Body->raw->Text换JSON

  * 开启自动转换JSON数据的支持（**@EnableWebMvc**）

    ```java
    @Configuration
    @ComponentScan("com.wyh.controller")
    @EnableWebMvc
    public class SpringmvcConfig {
    }
    ```

    **@EnableWebMvc**注解功能强大，整合了多个功能，在此包含了转换JSON。

  * 设置接收JSON数据（@RequestBody）

    ```java
    @RequestMapping("/welcome4")
        @ResponseBody
        public String welcome4(@RequestBody List<User> users) {
            System.out.println(users);
            return "<h1>Welcome to access this web page</h1>" +
                    "<p>users is "+users+"</p>";
        }
    ```

    **@RequestBody**：将请求体中的所包含的数据（JSON）传递给请求参数，此注解一个处理器方法只使用一次

    * 传递POJO类型的JSON数据：json数据中的对应名称与形参对象的属性名相同，`@RequestBody User user`即可
    * 传递POJO集合的JSON数据：json数组数据与集合泛型的属性名称相同，`@RequestBody List<User> users`即可

* **@RequestBody**与**@RequsetParam**的区别：

  * @RequestBody用于接收JSON数据（application/json）
  * @RequsetParam用于接收url地址传参和表单传非JSON参数（application/x-www-form-urlencoded）
  * 应用：**后期开发，以发送json格式数据为主（@RequestBody）**

* 日期类型参数传递

  * 根据不同的日期格式设置不同的接收方式，SpringMVC自动将String转Date类型

    ```java
    @RequestMapping("/date")
    @ResponseBody
    public String dateParam(Date date1,
    	@DateTimeFormat(pattern="yyyy-MM-dd") Date date2,
        @DateTimeFormat(pattern="yyyy/MM/dd HH:mm:ss") Date date3) {
        System.out.println(date1);
        System.out.println(date2);
        System.out.println(date3);
        return "<h1>Welcome to access this web page</h1>" +
               "<p>date1 is "+date1+"</p>" +
               "<p>date2 is "+date2+"</p>" +
               "<p>date3 is "+date3+"</p>";
    }
    ```

    **@DateTimeFormat**：设定日期时间型数据格式（pattern的值，时间日期格式字符串）

* 类型转换器

  * Converter接口

    ```java
    public interface Converter<S, T> {
        @Nullable
        T convert(S varl);
    }
    ```

    Spring根据这个接口实现了许许多多的实现类，专门用于类型转换

    年龄（String->Integer）日期（String->Date）

    许多都会默认打开转换，若是无法转换，那么SpringMvcConfig配置类加上**@EnableWebMvc**

#### 2.2 响应

##### 2.2.1 响应页面

```java
@RequestMapping("/welcomePage")
public String toPage() {
    return "welcomePage.jsp";
}
```

SpringMVC会自动认为这个格式下的返回值为一个页面，然后去寻找这个页面打开

经过几小时试错修改，发现了跳转成功但页面资源不显示原因：

```java
@Controller
//@RequestMapping("/user")这个响应页面的路径不能有前缀
public class UserController {
    //@RequestMapping("/users/toPage")不能有前缀
    @RequestMapping("/toPage")//只能不加前缀才行
    public String toPage() {
    System.out.println("success");
    return "Page.jsp";
    }
} 
```

##### 2.2.2 响应文本数据

```java
@RequestMapping("/toText")
@ResponseBody
public String toText() {
    return "welcome text";
}
```

如果不加**@ResponseBody**，那么还会认为是返回个页面，加上之后，Spring会知道这是返回一个字符串，也就是文本数据

##### 2.2.3 响应json数据（POJO，集合)（重点）

```java
@RequestMapping("/toJson")
@ResponseBody
public User toJsonPojo() {
    User user = new User();
    user.setName("wyh");
    user.setAge(20)
    return user;
}
...
@RequestMapping("/toJsonList")
@ResponseBody
public List<User> toJsonList() {
    User user1 = new User();
    user1.setName("wyh");
    user1.setAge(20);
    User user2 = new User();
    user2.setName("wyh");
    user2.setAge(20);
    List<User> users = new ArrayList<>();
    users.add(user1);
    users.add(user2);
    return users;
}
```

如果不加**@ResponseBody**，那么还会认为是返回个页面，加上之后，会知道这是返回一个User的POJO对象，再根据jackson-databind坐标导入的jar包，可以让SpringMVC**自动将POJO类转JSON格式字符串**并响应出去

* **@ResponseBody：设置当前控制器方法的返回值作为响应体**，不加则作为页面

* 响应的转换器：HttpMessageConverter接口



### 3. REST风格

##### 3.1 REST风格简介

* REST（Representational State Transfer）：表现形式状态转换
  * 传统风格资源描述形式
    * http://localhost/user/getById?id=1
    * http://localhost/user/saveUser
  * REST风格描述形式
    * http://localhost/user/1
    * http://localhost/user/
* 优点：隐藏资源的访问行为，无法通过地址得知对资源是哪种操作，书写简化
* 按照REST风格访问资源时使用**行为动作**区分对资源进行了哪种操作
  * http://localhost/users -查询全部用户信息-GET请求（查询）
  * http://localhost/users/1 -查询指定用户信息-GET请求（查询）
  * http://localhost/users -添加用户信息-POST请求 （新增/保存）
  * http://localhost/users -修改用户信息-PUT请求（修改/更新）
  * http://localhost/users/1 -删除用户信息-DELETE请求（删除）
    * 常用的请求方式：**GET**/**POST**/**PUT**/**DELETE**
* 根据REST风格对资源进行访问成为**RESTful**（也就是拿REST风格进行开发）
  * 上述行为为风格，不是规范，但现在都在使用，也和规范差不多了。
  * 模块的名称通常使用复数，加上s，表示此类资源。（如users，books）

##### 3.2 RESTful入门案例

* 设定HTTP的请求动作（**GET**/**POST**/**PUT**/**DELETE**）

  **@RequestMapping(value="/users", method=RequestMethod.POST)**

  method属性为HTTP的请求动作

* 设定请求参数（路径变量）（如/users/**1**的1）

  ```java
  @RequestMapping(value="/users/{id}", method=RequestMethod.DELETE)
  @ResponseBody
  public String delete(@PathVariable Integer id) {
      ...
  }
  ```

  **@PathVariable**：将路径上的变量给方法形参

  value="/users/{id}"，id与形参名（id）一致

* @RequestParam，@RequestBody，@PathVariable的区别与应用：

  * @RequestParam：用于接收url地址传参或非json表单传参
  * @RequestBody：用于接收JSON数据
  * @PathVariable：用于接收路径参数，使用{形参名称}描述路径参数
    * 后期开发，发送请求数超过一个，以JSON格式为主，@RequestBody应用较广
    * 发送非json格式数据，用@RequestParam
    * 采用RESTful开发，当参数数量较少，比如一个，用@PathVariable，常用于传递id值

##### 3.3 REST快速开发

Spring很通人性，知道很多重复步骤duck不必

比如，@RequestMapping都要带上value=“/users”

* 所以直接将它放在控制器类之上，成为前缀（/users）

又比如，每个方法都要带上@ResponseBody

* 所以也将它直接放在控制器类上

此时呢，控制器类上都有@Controller和@ResponseBody

* 所以用**@RestController**代替这两个注解，设置当前控制器类为RESTful风格，等于这两个注解的功能组合

又由于剩下的每个方法之上的@RequestMapping内都有method取值，且都为RequestMethod的值

* 所以Spring直接提供四类请求动作注解代替method属性取值（**@GetMapping，@POSTMapping，@PutMapping，@DeleteMapping**），设置当前控制器方法的请求访问路径与请求动作，内部可以取值，值为路径参数（如@GetMapping(“/{id})）

* 最终呈现：

  ```java
  @RestController
  @RequestMapping("/users")
  public class UserController {
      @GetMapping("/{id}")
      public String getById(@PathVariable Integer id) {..}
      
      @PutMapping
      public String update(@RequestBody User user) {..}
      
      @GetMapping()
      public String getAll() {..}
      
      @PostMapping
      public String save(@RequestBody User user) {..}
      
      @DeleteMapping("/{id}")
      public String delete(@PathVariable Integer id) {..}
  }
  ```

##### 3.4 案例：基于RESTful的页面数据交互

* 先制作后台接口，以及数据测试

  ```java
  @RestController
  @RequestMapping("/books")
  public class BookController {
      @PostMapping
      public String save(@RequestBody Book book) {
          System.out.println("insert into tb_books (name,type,description) values (" + book.getName() + "," + book.getType() + "," + book.getDescription() + ")");
          return "{'model': 'book save success'}";
      }
  
      @GetMapping
      public List<Book> selectAll() {
          System.out.println("select * form tb_books");
          Book book1 = new Book();
          Book book2 = new Book();
          book1.setId(1);
          book1.setName("《Java核心技术卷Ⅰ》");
          book1.setType("Java/编程");
          book1.setDescription("帮助巩固Java基础");
          book2.setId(2);
          book2.setName("《Java核心技术卷Ⅱ》");
          book2.setType("Java/编程");
          book2.setDescription("帮助深入Java学习");
          List<Book> books = new ArrayList<>();
          books.add(book1);
          books.add(book2);
          return books;
      }
  }
  ```

  测试交由Postman

* 由于拦截路径设置的全部都被SpringMVC拦截，所以要重新放行静态资源。在config包下新建一个support包，新建SpringMvcSupport类，设置为配置类，继承**WebMvcConfigurationSupport**，设置访问路径（"/pages/**"）与对应静态资源的目录（"/pages/"）的关系，设置为交由Tomcat服务器处理。SpringMvcConfig要加载这个配置类。

  ```java
  @Configuration
  public class SpringMvcSupport extends WebMvcConfigurationSupport {
      @Override
      protected void addResourceHandlers(ResourceHandlerRegistry registry) {
          registry.addResourceHandler("/pages/**").addResourceLocations("/pages/");
          registry.addResourceHandler("/js/**").addResourceLocations("/js/");
          registry.addResourceHandler("/css/**").addResourceLocations("/css/");
          registry.addResourceHandler("/plugins/**").addResourceLocations("/plugins/");
      }
  }
  ...
  @Configuration
  @ComponentScan({"com.wyh.controller","com.wyh.config.support"})
  @EnableWebMvc//开启JSON转换
  public class SpringMvcConfig {
  }
  ```

* 前端页面进行ajax异步请求，访问后台controller内的方法

  ```javascript
  				//添加
                  saveBook () {
                      axios.post("http://localhost:8080/RESTful/books",this.formData).then((res)=>{
  
                      });
                  },
  
                  //主页列表查询
                  getAll() {
                      axios.get("http://localhost:8080/RESTful/books").then((res)=>{
                          this.dataList = res.data;
                      });
                  },
  ```



### 4. SSM整合（重点）

#### 4.1 SSM整合（基于Spring/SpringMVC）

* SSM整合流程
  1. 创建工程
  2. SSM整合
     * Spring
       * SpringConfig
     * SpringMVC
       * ServletConfig
       * SpringMvcConfig
     * MyBatis（Spring整合MyBatis）
       * MybatisConfig
       * JdbcConfig
       * jdbc.properties
  3. 功能模块
     * 数据表/domain内的POJO类
     * dao（接口与自动代理）（数据交互）
     * service（接口与实现类）（业务逻辑）
       * 业务接口测试（Spring整合JUnit）
     * controller（请求与响应）（表现层）（RESTful）
       * 表现层接口测试（Postman）

#### 4.2 表现层数据封装

开发前后端之间的协议，对于数据的处理，企业有自己的规定协议

* 前端接收数据 —创建结果模型类

* 封装数据到"data"属性中

* 封装操作结果到"code"属性中

* 封装特殊消息到"message"属性中

* 后端设置统一数据返回结果类

  ```java
  public class Result{
      private Object data;
      private Integer code;
      private String msg;
  }
  ```

  * Result类中的字段并不固定，根据需要自定义，提供若干个构造方法，方便操作

* 设置统一数据返回结果编码

  ```java
  public class Code{
      public static final Integer SAVE_OK = 20011;
      public static final Integer DELETE_OK = 20021;
      public static final Integer UPDATE_OK = 20031;
      public static final Integer SELECT_OK = 20041;
  
      public static final Integer SAVE_ERR = 20010;
      public static final Integer DELETE_ERR = 20020;
      public static final Integer UPDATE_ERR = 20030;
      public static final Integer SELECT_ERR = 20040;
  }
  ```

#### 4.3 异常处理器

开发过程中不可避免会出现异常

* 各种异常：

  * 框架内部：因不合规使用
  * 数据层：外部服务器故障（访问超时）
  * 业务层：业务逻辑书写错误
  * 表现层：数据收集、校验等规则错误
  * 工具类：工具类的书写不严谨不健壮

* **所有异常均抛到表现层集中处理**

* 如果要每个方法单独书写的话，那你还是杀了我吧，所以**AOP思想**解决

* SpringMVC早就知道你写不完也不可能自己写，所以人家提供了快捷处理方式（SpringMVC，你是我的神！）

* **异常处理器**：集中，统一的处理项目中出现的异常

  * ```java
    @RestControllerAdvice
    public class ProjectExceptionAdvice{
        @ExceptionHandler(Exception.class)//异常处理器注解
        public Result doException(Exception e) {
            return new Result(6,null);
        }
    }
    ```

    **@RestControllerAdvice**：为REST开发的控制器类做增强，拥有@ResponseBody和@Component对应的功能

    **@ExceptionHandler**：设定指定异常的处理方案，**功能等同于控制器方法**（**可以响应数据到前端**），出现异常后，终止原式控制器执行，转入当前方法执行。（**可以根据处理的异常不同，制作多个方法处理对应的异常**）

#### 4.4 项目异常处理方案

* 项目异常分类：
  * 业务异常：
    * 不规范用户操作行为产生的异常（流氓会Java）
    * 规范的用户操作产生的异常
  * 系统异常
    * 项目运行中可预计且无法避免的异常
  * 其他异常
    * 编程时你压根想不到的异常
  
* 处理方案：
  * 业务异常：发送消息给用户，提醒规范操作（让他别闹）
  * 系统异常：
    * 发送固定消息传递给用户，安抚用户
    * 发送特定消息给运维人员，提醒维护
    * 记录日志
  * 其他异常：
    * 发送固定消息传递给用户，安抚用户
    * 发送特定消息给编程人员，提醒维护，并纳入可预期范围
    * 记录日志
  
  1. 自定义系统异常
  
     ```java
     public class SystemException extends RuntimeException{
         private Integer code;
     
         public Integer getCode() {
             return code;
         }
     
         public void setCode(Integer code) {
             this.code = code;
         }
     
         public SystemException(Integer code, String message) {
             super(message);
             this.code = code;
         }
     
         public SystemException(Integer code,String message, Throwable cause) {
             super(message, cause);
             this.code = code;
         }
     }
     ```
  
  2. 自定义业务异常
  
     ```java
     public class BusinessException extends RuntimeException{
         private Integer code;
     
         public Integer getCode() {
             return code;
         }
     
         public void setCode(Integer code) {
             this.code = code;
         }
     
         public BusinessException(Integer code, String message) {
             super(message);
             this.code = code;
         }
     
         public BusinessException(Integer code, String message, Throwable cause) {
             super(message, cause);
             this.code = code;
         }
     }
     ```
  
  3. 自定义异常编码在Code类中
  
     ```java
         public static final Integer SYSTEM_ERR = 50001;
         public static final Integer BUSINESS_ERR = 50002;
         public static final Integer UNKNOWN_ERR = 59999;
     ```
  
  4. 触发自定义异常
  
     ```java
         @Override
         public Book selectById(Integer id) {
             //模拟出现异常
             if (id.equals(1)) {
                 throw new BusinessException(Code.BUSINESS_ERR,"操作错误");
             }
             //将可能出现的异常进行包装，转换为自定义异常
             try{
                 int i = 1/0;
             }catch (Exception e) {
                 throw new SystemException(Code.SYSTEM_ERR,"服务器异常",e);
             }
             return bookDao.selectById(id);
         }
     ```
  
  5. 拦截并处理异常
  
     ```java
     @RestControllerAdvice//为REST风格的控制器做增强功能的注解
     public class ProjectExceptionAdvice{
         @ExceptionHandler(SystemException.class)//异常处理器
         public Result doSystemException(SystemException e) {
             //记录日志
             //发送消息给运维
             //发送邮件给开发人员。e对象也发给开发人员
             return new Result(e.getCode(),e.getMessage());
         }
     
         @ExceptionHandler(BusinessException.class)//异常处理器
         public Result doBusinessException(BusinessException e) {
             return new Result(e.getCode(),e.getMessage());
         }
     
         @ExceptionHandler(Exception.class)//处理其他未知异常
         public Result doException(Exception e) {
             //记录日志
             //发送消息给运维
             //发送邮件给开发人员。e对象也发给开发人员
             return new Result(Code.UNKNOWN_ERR,"系统繁忙，请稍后再试");
         }
     }
     ```

#### 4.5 案例：SSM整合标准开发

连接上页面之后，页面的异步请求与相应后续操作（JavaWeb有讲，Vue、AXIOS、Element-ui）

```javascript
axios.get("/books").then(resp=>{});
axios.get("/books/"+row.id).then(resp=>{});
axios.post("/books",this.formData).then(resp=>{});
axios.put("/books",this.formData).then(resp=>{});
axios.delete("/books/"+row.id).then(resp=>{});
```



### 5. 拦截器

#### 5.1 拦截器概念

* 拦截器（Interceptor）：在SpringMVC容器内的，一种动态拦截方法调用的机制
* 作用：
  * 在指定方法的调用前后执行预先设定的代码
  * **阻止原始方法的执行**（是否有权限）
* 过滤器：属于web容器层的三大组件之一，过滤请求是对静态资源还是动态资源的访问
* 拦截器与过滤器的区别：
  * 归属不同：Filter属于Servlet技术，Interceptor属于SpringMVC技术
  * 拦截内容不同：Filter在Web服务器对所有访问进行拦截，Interceptor在SpringMVC容器仅针对对SpringMVC的访问进行增强

#### 5.2 入门案例

1. 制作拦截器功能类

   声明拦截器的bean，并实现HandlerInterceptor接口。拦截器一般是给controller用的

   ```java
   @Component
   public class ProjectInterceptor implements HandlerInterceptor {
       @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           System.out.println("preHandle...");
           return true;
       }
   
       @Override
       public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
           System.out.println("postHandle...");
       }
   
       @Override
       public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
           System.out.println("afterCompletion...");
       }
   }
   ```

   ```java
   @Override
       public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
           System.out.println("preHandle...");
           return true;
       }
       //这个方法返回 false -》 即为不通过，不放行，只执行控制器方法之前的操作，也就是阻止了了控制器方法的执行
   	//true 为放行
   ```

2. 配置拦截器的执行位置

   定义配置类，继承WebMvcConfigurationSupport，实现addInterceptor方法，添加拦截的访问路径，路径可以设置多个

   ```java
   @Configuration
   public class SpringMvcSupport extends WebMvcConfigurationSupport {
       @Autowired
       private ProjectInterceptor projectInterceptor;
   
       @Override
       protected void addResourceHandlers(ResourceHandlerRegistry registry) {
           registry.addResourceHandler("/pages/**").addResourceLocations("/pages/");
           registry.addResourceHandler("/js/**").addResourceLocations("/js/");
           registry.addResourceHandler("/css/**").addResourceLocations("/css/");
           registry.addResourceHandler("/plugins/**").addResourceLocations("/plugins/");
           registry.addResourceHandler("/element-ui/**").addResourceLocations("/element-ui/");
       }
   
       @Override
       protected void addInterceptors(InterceptorRegistry registry) {
           registry.addInterceptor(projectInterceptor).addPathPatterns("/books","/books/*");
       }
   }
   ```

* 省去创建SpringMvcSupport配置类的简化开发：

  ```java
  @Configuration
  @ComponentScan({"com.wyh.controller"/*,"com.wyh.config.support"*/})
  @EnableWebMvc
  public class SpringMvcConfig implements WebMvcConfigurer {
      @Autowired
      private ProjectInterceptor projectInterceptor;
  
      @Override
      public void addInterceptors(InterceptorRegistry registry) {
          registry.addInterceptor(projectInterceptor).addPathPatterns("/books","/books/*");
      }
  
      @Override
      public void addResourceHandlers(ResourceHandlerRegistry registry) {
          registry.addResourceHandler("/pages/**").addResourceLocations("/pages/");
          registry.addResourceHandler("/js/**").addResourceLocations("/js/");
          registry.addResourceHandler("/css/**").addResourceLocations("/css/");
          registry.addResourceHandler("/plugins/**").addResourceLocations("/plugins/");
          registry.addResourceHandler("/element-ui/**").addResourceLocations("/element-ui/");
      }
  }
  ```

  让SpringMvcConfig配置类实现Spring的接口WebMvcConfigurer，重写相应的方法即可。侵入性较强，和Spring强绑定了。

* 执行流程：

  ```
  请求 => preHandle => (看return的返回值)
  									=> return true  => controller   => postHandle => afterCompletion =>...
  									=> return false => (直接跳过conroller postHandle afterCompletion) =>...
  ```

#### 5.3 拦截器参数

* 前置拦截处理：

  ```java
  public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {}
  ```

  * 参数：
    * request：请求对象
    * response：响应对象
    * handler：被调用的控制器方法，本质上是一个方法对象，对反射技术的Method对象进行了再包装。
  * 返回值：返回值若为false，被拦截的处理器（控制器）将不再执行

* 后置拦截处理：

  ```java
  public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {}
  ```

  * 参数：
    * modelAndView：如果处理器执行完成具有返回结果，可以读取到对应的数据与页面信息。并进行调整。（也就是用于页面响应或跳转的）

* 完成后拦截处理：

  ```java
  public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {}
  ```

  * 参数：
    * ex：如果处理器执行过程中出现了异常对象，可以针对异常进行单独处理。（但是我们已经有了异常处理器，这个就用处不大了）

#### 5.4 拦截器工作流程分析

* 多拦截器执行顺序：当配置多个拦截器时，形成拦截器链（了解即可）
* 拦截器链的运行顺序参照烂机器的添加顺序为准（在addInterceptor方法中的添加顺序）（进栈原理，先进后出）
* 当拦截器中出现对原始处理器的拦截，后面的拦截器均终止运行
* 当拦截器运行中断（return false），仅运行配置在前面的拦截器的afterCompletion操作



## 三.Maven高级

### 1. 分模块开发与设计

#### 1.1 分模块发开意义

* 将原始模块**按照功能拆分成若干个子模块**，方便模块之间的相互调用，接口共享
  * 如，SSM案例的各个包，对应各个功能层，可分为，ssm_controller，ssm_dao，ssm_domain模块等等

#### 1.2 分模块开发

1. 创建Maven项目。经过我多次尝试，总结出一个规律问题，IDEA创建新项目时，它一个文件就是一个模块，不是项目文件。所以，我们创建项目时，应当先有一个主目录为项目目录，存放之后创建的一个个模块，不然在项目里创建的模块会在第一个创建的模块里面。
2. 书写模块代码。针对模块功能进行设计，不是创建完工程再一个一个拆分
3. **写完一个模块**，我们就要通过maven指令（**Install**），**安装这个模块到本地仓库**（localRepository）。团队内部开发需要发布模块功能到团队内部可共享的仓库中去（**私服**）。不安装找不到，Maven是从仓库里面找这个模块的包的。

### 2. 依赖管理

* 依赖指项目所需要的各样的jar包。一个项目可有多个依赖

#### 2.1 依赖传递

* 依赖具有传递性
  * 直接依赖：在当前模块直接在pom文件写了坐标建立的依赖关系
  * 间接依赖：被依赖的资源也依赖了其他的资源，当前项目间接依赖其他资源（直接依赖里面的其他依赖资源）

#### 2.2 依赖冲突问题

* 路径优先：依赖中出现了相同的资源，层级越深，优先级越低，层级越浅，优先级越高（直接大于间接）（就近原则）
* 声明优先：资源在相同层级被依赖，配置顺序靠前的覆盖配置顺序靠后的（先写的就是老大）
* 特殊优先：配置了相同资源的不同版本，后配置的覆盖先配置的（后写的才是老大）

#### 2.3 可选依赖

* 对外隐藏当前所依赖的资源（不透明）。隐藏后对应的资源将不具有依赖传递性。其他模块将不知道你用没用，且用不到
* 在项目的某个资源依赖坐标（dependency）中加上`<optional>true</optional>`即可对外隐藏这个资源(false为可见，true不可见)
* 场景是，这个**模块被别人用**，有些资源不想被别人知道

#### 2.4 排除依赖

* 主动断开依赖的资源，被排除的资源无需指定版本（不需要）
* 也就是依赖了一个资源，这个资源里依赖了一个你不需要的资源，那么手动排除他，在这个资源的依赖坐标（dependency）中加上`<exclusions><exclusion></exclusion></exclusions>`，加上对应的不需要的资源工组名与项目名即可，无需指定版本，
* 场景是，**使用别人资源，且不想要里面依赖的某种资源**

### 3. 聚合与继承

####  3.1 聚合

* 聚合：将多个模块组织成一个整体，同时进行项目构建的过程

* 聚合工程：通常是一个不具有业务功能的空工程，有且仅有一个pom文件

* 作用：使用聚合工程将多个工程编组，通过对聚合工程进行构建，实现对所包含的模块进行同步构建
  * 当工程中某个模块发生更新时，必须保障工程中与已更新模块关联的其他模块同步更新。
  * 使用聚合工程解决批量模块的同步构建问题
  
* 聚合工程开发

  * 创建空maven模块，设置打包类型为pom

    ```xml
    <groupId>com.wyh</groupId>
    <artifactId>SSM_Totalpom</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>
    ```

    普通为jar web为war

  * 设置当前聚合工程所包含的子模块名称

    ```xml
    <modules>
            <module>../SSM</module>
            <module>../SSM_domain</module>
            <module>../SSM_dao</module>
    </modules>
    ```

    聚合工程中所包含的模块在进行构建时，会根据模块间的依赖关系设置构建顺序，与书写顺序无关

    参与聚合的工程，无法向上感知是否参与聚合，只能向下配置哪些模块参与本工程的聚合

#### 3.2 继承

* 继承：描述两个工程间的关系，与Java的继承类似，子工程可以继承父工程的配置信息，常见于依赖关系的继承

* 作用：简化配置，减少版本冲突

* 继承与聚合一起出现

* 开发

  * 在父工程中配置所有子工程都需要的配置信息，共用坐标全部放在这

  * 在父工程中配置可选依赖关系

    ```xml
    <dependencyManagement>
    	<dependecies>
        	<dependency>
            	<gruopId></gruopId>
                <artifactId></artifactId>
                <version></version>
            </dependency>
        </dependecies>
    </dependencyManagement>
    ```

  * 在子工程配置中配置继承的父工程

    ```xml
    <parent>
    	<groupId>com.wyh</groupId>
        <artifactId>SSM_Totalpom</artifactId>
        <version>1.0-SNAPSHOT</version>
        <relativePath>../SSM_Totalpom/pom.xml</relativePath>
    </parent>
    ```

    `**<relativePath>**`标签指明配置的父工程的pom文件是哪个，可写可不写

  * 子工程可配置父工程提供的可选依赖

    ```xml
    <dependecies>
        	<dependency>
            	<gruopId></gruopId>
                <artifactId></artifactId>
            </dependency>
    </dependecies>
    ```

    只需要提供群组id和项目id，无需提供版本。版本由父工程统一提供，避免了版本冲突。子工程还可以自己配置自己单独需要的资源依赖。

* 聚合与继承的作用：

  * 聚合用于快速构建项目
  * 继承用于快速配置

* 聚合与继承的相同点：

  * 都是写在一个空模块的pom文件中，打包方式都为pom，所以一般，这两玩意儿一起出现，在一个pom文件中

* 聚合与继承的不同点：

  * 聚合是在当前模块中配置关系，聚合可以感知有哪些模块参与了
  * 继承是在子模块中配置关系，父模块不知道有哪些子模块

### 4. 属性管理

* 属性配置与使用

  * 定义属性：

    ```xml
    	<properties>
            <maven.compiler.source>8</maven.compiler.source>
            <maven.compiler.target>8</maven.compiler.target>
            <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!--        自定义属性-->
            <spring-version>5.2.10.RELEASE</spring-version>
        </properties>
    ```

  * 引用属性：

    ```xml
    		<dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-webmvc</artifactId>
                <version>${spring-version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-jdbc</artifactId>
                <version>${spring-version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework</groupId>
                <artifactId>spring-test</artifactId>
                <version>${spring-version}</version>
            </dependency>
    ```

    ${自定义属性名}即可

* 资源文件（xxx.properties）引用属性

  * 首先在pom中定义属性，与上步骤一样

    ```xml
    	<properties>
            <maven.compiler.source>8</maven.compiler.source>
            <maven.compiler.target>8</maven.compiler.target>
            <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <!--        自定义属性-->
            <spring-version>5.2.10.RELEASE</spring-version>
            <jdbc.url>jdbc:mysql://localhost:3306/xxx</jdbc.url>
        </properties>
    ```

  * 配置文件用${jdbc.url}占位引用属性

  *  开启资源文件目录加载属性的过滤器

    ```xml
    <build>
    	<resources>
        	<directory>${project.basedir}/src/main/resources</directory>
            <filtering>true</filtering>
        </resources>
    </build>
    ```

    `<directory>`指资源文件所处的目录是哪儿，且只可以写一个

    `<filtering>`指开启允许资源文件解析Maven的pom文件中的属性

    ${project.basedir}是指当前这个pom文件所处的目录，子模块继承之后也可以让他们自己的resources文件下的资源文件能读取Maven提供的属性

* （了解）配置Maven打包时，忽略web.xml的检查

  ```xml
  <plugin>
  	<groupId>org.apache.maven,plugins</groupId>
      <artifactId>maven-war-plugin</artifactId>
      <version>3.2.3</version>
      <configuration>
      	<fileOnMissingWebXml>false</fileOnMissingWebXml>
      </configuration>
  </plugin>
  ```

* maven的内置属性：${project.basedir}

* 版本管理：
  * 工程版本：
    * SNAPSHOT（快照版本）
      * 项目开发过程中临时输出的版本
      * 会不断更新
    * RELEASE（发布版本）
      * 项目开发到阶段里程碑，向外部发布的较稳定的版本，后续开发不会影响这个版本
      * alpha版
      * beta版
      * 纯数字版

### 5. 多环境配置与应用

#### 5.1 多环境开发

* maven提供配置多环境开发的设定，帮助开发者使用过程中快速切换环境

* 定义多环境

  ```xml
  <!--    定义多环境-->
      <profiles>
  <!--        定义具体生产环境-->
          <profile>
  <!--            定义环境唯一id-->
              <id>env_dev</id>
  <!--            定义环境中专用自定义属性-->
              <properties>
                  <jdbc.url>jdbc:mysql:///db1?useServerPrepStmts=true</jdbc.url>
              </properties>
  <!--            设置默认启动-->
              <activation>
                  <activeByDefault>true</activeByDefault>
              </activation>
          </profile>
          <!--        定义具体生产环境-->
          <profile>
              <!--            定义环境唯一id-->
              <id>env_test</id>
              <!--            定义环境中专用自定义属性-->
              <properties>
                  <jdbc.url>jdbc:mysql://127.1.1.1:3306/db1?useServerPrepStmts=true</jdbc.url>
              </properties>
          </profile>
          <!--        定义具体生产环境-->
          <profile>
              <!--            定义环境唯一id-->
              <id>env_pro</id>
              <!--            定义环境中专用自定义属性-->
              <properties>
                  <jdbc.url>jdbc:mysql://127.2.2.2:3306/db1?useServerPrepStmts=true</jdbc.url>
              </properties>
          </profile>
      </profiles>
  ```
  
* 使用多环境（构建过程）：`mvn 指令 -p 环境定义的唯一id`（如：mvn install -p pro_env）

#### 5.2 跳过测试

* 应用场景：

  * 更新功能中并没有开发完毕又想打包上线
  * 快速打包
  * ...

* 指令版：mvn 指令 -D skipTests（全部跳过测试）

* IDEA按钮版：Maven窗口上有

* 配置文件版（细粒度控制）：

  ```xml
      <build>
          .....
          <plugins>
              <plugin>
                  <artifactId>maven-surefire-plugin</artifactId>
                  <version>2.22.2</version>
                  <configuration>
                      <skipTests>false</skipTests><!--true为跳过全部测试-->
  <!--                    跳过某个文件，不测试-->
                      <excludes>**/User*Test.java</excludes>
  <!--                    指定测试某个文件-->
                      <includes>**/User*TestCase.java</includes>
                  </configuration>
              </plugin>
          </plugins>
      </build>
  ```

### 6. 私服

#### 6.1 私服简介

* 私服是一台独立的服务器。用于解决团队之间内部的资源共享与资源同步问题
* Nexus：
  * Sonatype公司的一款maven私服产品（基于GAV坐标的）
  * 启动服务器（命令行启动）：`nexus.exe /run nexus`
  * 访问服务器（默认端口8081）：localhost:8081
  * 修改基础配置信息：安装路径下的etc目录中的nexus-default.properties文件，保存了nexus的基础配置信息，如默认端口
  * 修改服务器运行配置信息：在安装路径下的bin目录中的nexus.vmoptions文件保存了nexus服务器启动时对应的配置信息，如默认占用内存

#### 6.2 私服仓库分类

* 宿主仓库：hosted
  * 功能：保存自主研发的+第三方资源（带版权要钱的）
  * 关联操作：上传
* 代理仓库：proxy
  * 功能：代理连接中央仓库。以后就不是直接连接中央仓库下载开源资源，所有的需求都找这个仓库要，没有的话，他找中央仓库要
  * 关联操作：下载
* 仓库组：group
  * 功能：为各个仓库编组简化下载操作
  * 关联操作：下载
* 自己创建宿主仓库（maven2-hosted）

#### 6.3 资源上传与下载

* 配置私服位置（Maven-3.8.1的setting.xml）

  ```xml
      <server>
        <id>wyh-snapshot</id>
        <username>admin</username>
        <password>020920</password>
      </server>
      <server>
        <id>wyh-release</id>
        <username>admin</username>
        <password>020920</password>
      </server>
    </servers>
  ```

  ```xml
           <mirror>
              <id>maven-public</id>
              <mirrorOf>*</mirrorOf>
              <url>http://localhost:8081/repository/maven-public/</url>
          </mirror>
  ```

* 配置上传的私服位置（在工程的pom文件中）

  ```xml
  <!--    配置当前工程保存在私服里的的位置-->
      <distributionManagement>
  <!--        release仓库-->
          <repository>
              <id>wyh-release</id>
              <url>http://localhost:8081/repository/wyh-release/</url>
          </repository>
  <!--        snapshot仓库-->
          <snapshotRepository>
              <id>wyh-snapshot</id>
              <url>http://localhost:8081/repository/wyh-snapshot/</url>
          </snapshotRepository>
      </distributionManagement>
  ```

* 发布上传的指令：mvn deploy（或者直接IDEA点deploy）

* 配置私服访问的中央仓库位置（在Nexus服务器中）：齿轮 -> Repositories -> maven-central -> Remote storage的路径改为阿里云路径 -> save



## 四.SpringBoot

### 1. SpringBoot简介

#### 1.1 入门案例

* SpringBoot由pivotal团队提供的全新框架，目的是为了简化Spring的开发（简化简化的简化😂）
* 入门程序：
  1. 创建新模块，在选择模块骨架时，选择**Spring Initializr**（Spring初始化），并配置模块的相关信息，SDK版本与Java版本一致
  2. 选择当前模块需要使用的技术集（如：Spring Web）
  3. 开发控制类（RESTful开发的）
  4. 运行它给你自动生成的Application类
     * 即直接完成了简单后台服务器开发，没有任何坐标导入，Tomcat服务器他都给你自己适配了，牛逼
* SpringBoot两个核心基础文件：
  * pom.xml（有继承）
  * Application类
* SpringBoot简化：
  * 将**pom文件中的坐标**由手工添加变为**勾选添加**
  * 将**web3.0配置类**由手工制作变为**无需制作**
  * 将**Spring/SpringMVC配置类**由手工制作变为**无需制作**
  * 控制器做法未变，那是必然的，这事交给你
    * 注意：基于IDEA开发SpringBoot程序需要联网才能够加载到程序框架结构。不用IDEA则可以去Spring官网创建工程，然后导入

* SpringBoot的快速启动：
  * 对SpringBoot项目打包（执行Maven的构建指令 package）
  * 在打好的jar包文件下，敲cmd调出命令行窗口，输入：`java -jar + TAB键自动补齐该jar包`
    * jar包支持命令行启动的话，需要有对应的插件：org.springframework.boot:**spring-boot-maven-plugin**，要一并打包好

#### 1.2 SpringBoot概述

* SpringBoot用来简化Spring应用的初始搭建以及开发过程

* Spring程序缺点：

  * 配置繁琐
  * 依赖设置复杂

* SpringBoot优点：

  * 自动配置
  * 起步依赖（简化了依赖配置）
  * 辅助功能（内置服务器等）

* 起步依赖（核心功能）：

  * **starter**：
    * SpringBoot常见项目名称，定义了当前项目使用的所有项目坐标，以达到**减少依赖配置**的目的
  * parent：
    * 所有SpringBoot项目要继承的项目，定义了若干个坐标版本号（依赖管理，SpringBoot给你统一规定了版本），以达到**减少依赖冲突**的目的
    * spring-boot-starter-parent（2.5.0）与（2.4.6）共计57处不同
  * 实际开发：
    * 使用任意坐标时，仅书写GAV的G和A，无需指定版本，版本由SpringBoot提供
    * 如果发生坐标错误，再指定版本（要小心版本冲突）

* 辅助功能：快速启动内自带了一些辅助插件，方便快速启动

* 启动方式：

  ```java
  @SpringBootApplication
  public class SpringBoot1Application {//引导类
      public static void main(String[] args) {
          SpringApplication.run(SpringBoot1Application.class, args);
      }
  }
  ```

* SpringBoot在创建项目时，采用的是jar的打包方式

* SpringBoot的**引导类是项目的入口，运行main方法**就是启动项目

* 引导类就是一个配置类，里面整合了所有的Spring配置类所需注解，它会**自动扫描并加载当前引导类所在包及其子包内的所有bean**

* 使用Maven依赖管理变更起步依赖项

  ```xml
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-web</artifactId>
  <!--排除依赖     更换技术首先需要排除之前那项技术-->
              <exclusions>
                  <exclusion>
                      <groupId>org.springframework.boot</groupId>
                      <artifactId>spring-boot-starter-tomcat</artifactId>
                  </exclusion>
              </exclusions>
          </dependency>
  <!--然后自己再导入需要的技术的GA，无需版本-->
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-jetty</artifactId>
          </dependency>
  ```

  Jetty比Tomcat更轻量级，可扩展性相较于Tomcat更强，谷歌应用引擎（GAE）已经全面切换为了Jetty。小型项目可用Jetty，大型还是Tomcat

### 2. 基础配置

#### 2.1 配置文件格式

* SpringBoot提供了多种属性配置方式

  * application.properties文件：`server.port=80`

  * application.yml文件：

    ```yml
    server:
    	port: 81
    ```

  * application.yaml文件：

    ```yaml
    server:
    	port: 82
    ```

  * 以后开发SpringBoot主要写**yml格式文件**

  * 三种文件的生效顺序：properties > yml > yaml

* yml文件内部IDEA自动提示失效的话：项目结构 -> facet -> 哪个Spring项目模块 -> 单击Configuration filels -> 点击上方自定义SpringBoot -> 点击文件点击加号添加文件到工程中即可

* 敲关键字有自动提示

#### 2.2 yaml

* YAML（YAML Ain't Markup Language），一种数据序列化格式

* 优点：

  * 容易阅读
  * 容易与脚本语言交互
  * 以数据为核心，重数据轻格式

* YAML文件扩展名

  * .yml（主流）
  * .yaml
  * 都是对的

* 语法规则：

  * 大小写敏感
  * 属性层级关系使用多行描述，每行结尾用冒号结束
  * 使用缩进表示层级关系，同层级左侧对齐，只允许使用空格，不允许Tab键
  * 属性值前面添加空格（属性名: 属性值），数据前面要**加空格**与冒号隔开
  * #代表注释

* YAML数组格式：(用于一个属性配多个值)

  ```yaml
  likes:
     - music
     - Game
     - programming
  ```

* yaml读取数据：

  * 使用@Value读取单个数据，属性名引用方式：${一级属性名.二级属性名....}

    ```yaml
    #yaml中数据
    lesson: SpringBoot
    
    server:
      port: 80
    
    enterprise:
      name: wyh
      age: 20
      tel: 187******
      likes:
       - Java 
       - C/C++
    ```

    ```java
    @RestController
    @RequestMapping("/books")
    public class BookController {
        @Value("${lesson}")
        private String lesson;
        @Value("${server.port}")
        private int port;
        @Value("${enterprise.likes[1]}")
        private String like;
    }
    ```

  * 封装数据到Environment对象中（框架内部使用几率高）

    ```java
    @RestController
    @RequestMapping("/books")
    public class BookController {
        @Autowired
        private Environment environment;
        .....
        //调用
        //environment.getProperty("lesson")
        //environment.getProperty("enterprise.likes[0]")
    }
    ```

  * 自定义对象封装指定数据（常用）

    ```java
    @Component
    @ConfigurationProperties(prefix = "enterprise")//可以添加下一层数据 enterprise.likes
    public class Enterprise {
        private String name;
        private int age;
        private String[] likes;
    }
    ```

    ```java
    @RestController
    @RequestMapping("/books")
    public class BookController {
    	@Autowired
        private Enterprise enterprise
    }
    ```

  * 自定义对象封装数据警告解决方案

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-configuration-processor</artifactId>
        <optional>true</optional>
    </dependency>
    ```

#### 2.3 多环境启动

* yml文件中：

```yml
#设置启动哪个环境
spring:
  profiles:
    active: test
---
#开发
spring:
  profiles: dev
server:
  port: 81
---
#生产
server:
  port: 82
#以下这个方式为最新的配置方式，但是感觉有点冗杂，还是老旧的好，少
spring:
  config:
    activate:
      on-profile: pro
---
#测试
spring:
  profiles: test
server:
  port: 83
```

* properties文件格式：（了解）

  * 主启动配置文件：application.properties

    ```properties
    spring.profiles.active=pro
    ```

  * 环境分类配置文件：application-pro.properties

    ```properties
    server.port=80
    ```

  * 环境分类配置文件：application-dev.properties

    ```properties
    server.port=81
    ```

  * 环境分类配置文件：application-test.properties

    ```properties
    server.port=82
    ```

* 多环境启动命令格式

  package前要先clean

  文档编码格式为UTF-8

  * 带参数启动SpringBoot（即可切换想要的环境）

    ```
    java -jar SpringBoot_1.jar --spring.profiles.active=test
    ```

  * 命令行临时修改端口（可配置文件中没有的端口号）（有加载优先顺序，命令行参数大于配置文件中的）

    ```
    java -jar SpringBoot_1.jar --spring.profiles.active=test --server.port=8080
    或
    java -jar SpringBoot_1.jar --server.port=8080
    ```

* 多环境开发控制

  * 有Maven又有SpringBoot的多环境，**Maven为主**，SpringBoot为辅

  * maven中

    ```xml
        <!--    定义多环境-->
        <profiles>
            <!--        定义具体生产环境-->
            <profile>
                <!--            定义环境唯一id-->
                <id>pro</id>
                <!--            定义环境中专用自定义属性-->
                <properties>
                    <profile.active>pro</profile.active>
                </properties>
                <!--            设置默认启动-->
                <activation>
                    <activeByDefault>true</activeByDefault>
                </activation>
            </profile>
            <!--        定义具体生产环境-->
            <profile>
                <!--            定义环境唯一id-->
                <id>test</id>
                <!--            定义环境中专用自定义属性-->
                <properties>
                    <profile.active>test</profile.active>
                </properties>
            </profile>
            <!--        定义具体生产环境-->
            <profile>
                <!--            定义环境唯一id-->
                <id>dev</id>
                <!--            定义环境中专用自定义属性-->
                <properties>
                    <profile.active>dev</profile.active>
                </properties>
            </profile>
        </profiles>
    ```

  * SpringBoot的application.yml文件读取maven的属性

    ```yml
    #设置启动哪个环境
    spring:
      profiles:
        active: ${profile.active}
    ```

  * 添加maven插件解析yml文件的占位符${profile.active}

    ```xml
    			<plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-resources-plugin</artifactId>
                    <version>3.3.0</version>
                    <configuration>
                        <encoding>UTF-8</encoding>
                        <useDefaultDelimiters>true</useDefaultDelimiters>
                    </configuration>
                </plugin>
    ```

#### 2.4 配置文件分类

* SpringBoot中的四级配置文件
  * 一级（最高）：file : config/application.yml（这是在打包后的文件夹中创建一个config目录，里面放application.yml，内置属性）
  * 二级：file ：application.yml（这是在打包后的文件夹中放application.yml，内置属性）
  * 三级：classpath：config/application.yml（这是在IDEA项目的resource文件夹中创建一个config目录，里面放application.yml，内置属性）
  * 四级（最低）：classpath：application.yml（这是在IDEA项目的resource文件夹里面放application.yml，内置属性）
* 作用：
  * 一级与二级留做系统打包后的设置通用属性
  * 三级和四级用于系统开发阶段设置通用属性

### 3. 整合第三方技术

#### 3.1 整合Junit

* SpringBoot自动给你配置整合好了Junit

  ```java
  @SpringBootTest//开启SpringBoot的测试
  class SpringBoot2ApplicationTests {
      @Autowired//注入要测试的功能对象
      private BookService bookService;
  
      @Test//然后测试
      void testSelectAll() {
          bookService.selectAll();
      }
  }
  ```

* @SpringBootTest：设置Junit加载的SpringBoot启动类（也就是自动创建的那个引导类）

  * 相关属性：classes （包名与层级一致则可以省略这个属性）

  * 作用：设置指定的SpringBoot的启动类（当测试类所在的包层级与包名与main目录中的启动类包名和层级不同会报错，那么指定即可解决报错）

    ```java
    @SpringBootTest(classes = SpringBoot2Application.class)
    ```

    这个测试注解和相应的测试类SpringBoot会给你自动创建，不要可以自己创

#### 3.2 整合MyBatis

* Spring整合MyBatis时：

  * 配置SpringConfig配置类，导入JdbcConfig和MyBatisConfig配置类
  * 配置JdbcConfig配置类，定义数据源（加载jdbc.properties文件内的属性）
  * 配置MyBatisConfig配置类，定义两个bean
    * SqlSessionFactoryBean和MapperScannerConfigurer（映射配置类）

* SpringBoot直接简化：

  * 创建模块时，选择SQL栏中的：**MyBatis framework** 和 **MySQL Driver**

  * 设置数据源参数：

    ```yml
    spring:
      datasource:
        driver-class-name: com.mysql.cj.jdbc.Driver
        url: jdbc:mysql://localhost:3306/db1?useServerPrepStmts=true
        username: root
        password: 020920
        type: com.alibaba.druid.pool.DruidDataSource
    ```

    SpringBoot整合时，**内置了默认的数据源**，但里面没有相应的参数设置，比如连接哪个库，所以这是我们唯一需要做的，替换了Jdbc.properties文件

    我们可以更改替换掉SpringBoot的默认数据源为Druid，要导入坐标，并在yml文件中的type属性加上对应的DruidDataSource全路径名

  * 定义数据层接口与映射的配置

    ```java
    @Mapper
    public interface BookDao {
        @Select("select * from tb_books")
        List<Book> selectAll();
    }
    ```

    由于没有了MyBatisConfig配置类，无法告知需要创建代理对象的mapper接口是哪个

    所以加上**@Mapper**注解。SpringBoot扫描到这个注解，就会创建mapper的代理对象，由这个对象去执行对应的的SQL语句                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       

#### 3.3 基于SpringBoot的SSM整合

* 基于SpringBoot整合SSM
  * 不需要整合Spring和SpringMVC，勾选Web即可
  * 主要是整合MyBatis
  * pom.xml：
    * 配置起步依赖
    * 必要的资源坐标（Druid）
  * application.yml：
    * 数据源
    * 端口、多开发环境
  * 无需再设置任何配置类
  * dao层接口注解多加了一个**@Mapper**，告知要创建mapper代理对象的dao层接口
  
* 实现细节：

  * 页面资源（html、css等）放在resources目录的**static目录**下

  * 若想直接在浏览器地址栏输入localhost就访问到books.html页面，那么在static页面下创建页面index.html，跳转到页面即可

    ```html
    <script>
      document.location.href = "pages/books.html"
    </script>
    ```



## 五.MybatisPlus

### 1.MyBatisPlus简介

#### 1.1 入门案例

* MyBatisPlus（简称MP）是基于MyBatis框架基础上开发的增强型**工具**，旨在简化开发，提高开发效率

* 开发方式：

  * 基于MyBatis使用MP
  * 基于Spring使用MP
  * 基于SpringBoot使用MP（当前课程首推）

* 使用步骤：

  * 创建SpringBoot模块，选择MySQL Driver 和 MP（IDEA默认没有的话那就手动添加）

  * 添加MP起步依赖

    ```xml
    		<dependency>
                <groupId>com.baomidou</groupId>
                <artifactId>mybatis-plus-boot-starter</artifactId>
                <version>3.5.2</version>
            </dependency>
    ```

  * 设置JDBC参数（application.yml）

  * 数据库中的**表名（user）要与实体类名（User）相对应，属性名和字段名相对应，不同就会报错**

  * 定义数据接口，继承**BaseMapper<User>**，是一个泛型，指定实体类数据类型，再加上**@Mapper**注解使SpringBoot创建mapper代理对象

  * 测试是否能访问数据库操作

#### 1.2 MyBatisPlus概述

* 国内技术
* 特性：
  * 无侵入：只做增强不做改变
  * 强更大的CRUD操作：内置通用mapper，少量配置即可完成单表CRUD操作
  * 支持Lambda：编写查询条件无须担心字段写错
  * 支持主键自动生成
  * 内置分页插件



### 2.标准数据层开发

#### 2.1 标准数据层高CRUD功能

* MP接口：

  * int inert(T t)
  * int deleteById(Serializable id)
  * int updateById(T t)
  * T selectById(Serializable id)
  * List<T> selelctList()
  * IPage<T> selectPage(IPage<t> page)
  * IPage<T> selectPage(Wrapper<T> queryWrapper)

* lombok：一个Java类库，提供了一组注解，简化了POJO的开发

  * 导入坐标

    ```xml
    <dependency>
    	<groupId>org.prorjectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.12</version>
        <scope>provided</scope>
    </dependency>
    ```

  * 常用注解：@Date

    ```java
    @Date
    public class User{
    	private Long id;
    	....
    }
    ```

    为当前实体类在编译期设置对应的get/set方法，无参/有参构造方法，toString方法，hashCode方法，equals方法等

#### 2.2 分页功能

*  IPage<T> selectPage(IPage<t> page)

* IPage<T> selectPage(Wrapper<T> queryWrapper)

* 设置分页拦截器作为Spring管理的bean

  ```java
  public class MpConfig {
      @Bean
      public MybatisPlusInterceptor pageInterceptor() {
          MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
          interceptor.addInnerInterceptor(new PaginationInnerInterceptor());
          return interceptor;
      }
  }
  ```

* 执行分页查询

  * ```java
    当前页码：page.getCurrent()
    当前显示数：page.getSize()
    当前一共多少页：page.getPages()
    当前一共多少条数据：page.getTotal()
    当前数据：page.getRecords()
    ```

* 可以开启日志功能

  ```yml
  mybatis-plus:
    configuration:
      log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
  ```



### 3.DQL控制

#### 3.1 条件查询方式

* MP将书写复杂的SQL查询条件进行封装，使用编程的形式完成查询条件的组合

* Wrapper用来封装条件查询的条件

* 设置条件查询条件

  * 方式一：QueryWrapper

    ```java
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper.lt("id",4);//常规格式
    wrapper.lambda().lt(User::getId,4);//Lambda格式
    wrapper.lt("id",4).gt("id",2);//链式格式
    System.out.println(userDao.selectList(wrapper));
    ```

  * 方式二：LambdaQueryWrapper

    ```java
    LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<>();
    lqw.lt(User::getId,4);
    lqw.gt(User::getId,2);
    lqw.gt(User::getId,2).lt(User::getId,4);
    lqw.lt(User::getId,2).or().gt(User::getId,3);
    System.out.println(userDao.selectList(lqw));
    ```

  * 组合关系：

    * 直接链式为AND

      ```java
      wrapper.lt("id",4).gt("id",2);
      ```

    * 加or()为OR

      ```java
      lqw.lt(User::getId,2).or().gt(User::getId,3);
      ```

* null值处理

  一般情况下，用户不会在条件查询时将条件区间写完，会有一个null值，SQL在拼接时，拼接上null会报错，处理方式就是，如果为null，那么不拼接在SQL中

  * if语句控制条件追加

    ```java
    LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<>();
    if(null != userQuery.getAge()) {
        lqw.gt(User::getAge,userQuery.getAge());
    }
    if(null != userQuery.getAge2()) {
        lqw.lt(User::getAge,userQuery.getAge2());
    }
    System.out.println(userDao.selectList(lqw));
    ```

  * 条件参数控制

    ```java
    LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<>();
    lqw.gt(null != userQuery.getAge(),User::getAge,userQuery.getAge());
    lqw.lt(null != userQuery.getAge2(),User::getAge,userQuery.getAge2());//这两行可以链式变编程，太长了一般不推荐
    System.out.println(userDao.selectList(lqw));
    ```

#### 3.2 查询投影

* 也就是查询的字段控制，限制你能看到的字段数据内容，只能看到你设置的字段数据

* 查询结果包含模型类中的部分属性

  ```
  LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<>();
  lqw.select(User::getId,User::getName,User::getAge);//只有Lambda方式才可以使用，::为方法引用，指定能查询的属性
  System.out.println(userDao.selectList(lqw));
  ```

* 查询结果包含模型类中未定义的属性（比如一共多少行：count（*））

  ```
  QueryWrapper<User> wrapper = new QueryWrapper<>();
  wrapper.select("gender","count(*) as nums");//逗号后指的是要查询多少列，并将列数取个别名为nums，逗号前指要查询的列名为gender
  wrapper.groupBy("gender");//以什么列（属性）来分组
  System.out.println(userDao.selectList(wrapper));
  ```

#### 3.3 查询条件设定

* 范围匹配（>, <, =, between）

  * 等匹配（等于，eq）：登录时，或者查询一个时

    ```java
    lqw.eq(User::getName,"wyh").eq(User::getPassword,"jerry");
    System.out.println(userDao.selectOne(lqw));
    ```

  * 范围匹配：lt（小于，没有等号），le（小于，有等号），gt（大于，无等号），ge（大于，有等号），between（大于等于~小于等于）

    购物设定价格区间，户籍设定年龄区间

    ```java
    lqw.between(User::getAge,10,20);//前后顺序不可变，前为最小值，后为最大值
    ```

* 模糊匹配（like）：查信息，搜索新闻（非全文检索版）

  ```java
  lqw.like(User::getName,"J");
  lqw.likeRight(User::getName,"J");//J%
  lqw.likeLeft(User::getName,"J");%J
  ```

* 空判断（null）

* 包含性匹配（in）

* 分组（group）：统计报表（分组查询聚合函数）

  ```java
  QueryWrapper<User> wrapper = new QueryWrapper<>();
  wrapper.select("count(*) as nums,gender");//逗号前指的是要查询多少列，并将列数取个别名为nums，逗号后指要查询的列名为gender
  wrapper.groupBy("gender");//以什么列（属性）来分组
  System.out.println(userDao.selectList(wrapper));
  ```

* 排序（order）

* 更多条件查询在官网（https://mybatis.plus/guide/wrapper.html#abstractwrapper）

#### 3.4 字段映射与表名映射

* 实体类中属性名与表中字段名不一致，那肯定是后端开发人员解决这个问题，数据库的表早就做好了，没法改

  ```mysql
  create table user {
  	id bigint(20) NOT NULL AUTO_INCREMENT,
  	name varchar(32),
  	pwd varchar(32),
  	age int(3),
  	tel varchar(32),
  	PRIMARY KEY('id')
  }
  ```

  ```java
  //如表中为pwd，实体类中为password
  public class User {
  	private Long id;
      private String name;
      @TableField(value="pwd")//设置当前属性映射表中的pwd字段
      private String password;
      private Integer age;
      private String tel;
  }
  ```

* 实体类中存在数据库表中没有的字段属性

  ```java
  //如实体类中的online，表中没有这样的字段
  public class User {
  	private Long id;
      private String name;
      @TableField(value="pwd")//设置当前属性映射表中的pwd字段
      private String password;
      private Integer age;
      private String tel;
      @TableField(exist=false)//设置当前属性只有在这个实体类中使用，数据表中没有，不要去找了
      private Integer online;
  }
  ```

* 采用默认查询时，开放了许多私密字段的数据显示，比如查询时能看得见密码的值

  ```java
  public class User {
  	private Long id;
      private String name;
      @TableField(value="pwd",select=false)//设置当前属性映射表中的pwd字段,并设置当前属性不参与查询，与select()配置不冲突
      private String password;
      private Integer age;
      private String tel;
      @TableField(exist=false)//设置当前属性只有在这个实体类中使用，数据表中没有，不要去找了
      private Integer online;
  }
  ```

* 表名与实体类名不一致，如表名为tb_user，实体类名为User

  ```java
  @TableName("tb_user")//设置当前实体类映射的表为tb_user
  public class User {
  	private Long id;
      private String name;
      @TableField(value="pwd",select=false)//设置当前属性映射表中的pwd字段,并设置当前属性不参与查询，与select()配置不冲突
      private String password;
      private Integer age;
      private String tel;
      @TableField(exist=false)//设置当前属性只有在这个实体类中使用，数据表中没有，不要去找了
      private Integer online;
  }
  ```

  

### 4.DML控制

* 添加，修改，删除

* id生成策略控制

  * 日志：自增

  * 购物订单：特殊规则

  * 外卖单：关联地区日期等信息

  * 关系表：可以省略id

    * 注解：@TableId

      * 属性：IdType

        * 取值（策略）：

          * **AUTO**（0）：使用数据库id自增策略控制id生成

          * NONE（1）：不设置id生成策略

          * **INPUT**（2）：用户手工输入id

          * **ASSIGN_ID**（3）（默认的）：雪花算法生成id，可兼容数值型与字符串型，如果自己手动输入了，那么用你输入的

          * ASSIGN_UUID（4）：以UUID生成算法作为id生成策略

            * 雪花算法：生成一个64位二进制数（long值）

              ```asciiarmor
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

  * @TableName("tb_user")等同于在yml文件中配置MP全局属性

    ```yml
    	global-config:
    		banner: false
    		db-config:
    		  id-type: assign_id
    		   prefix: tb_      #为实体类名添加前缀，将实体类名User变为user加上这个前缀映射库中的表tb_user
    ```

* 多记录操作

  * 按主键删除多条记录

    ```java
    List<Long> ids = Arrays.asList(new Long[]{2,3});
    userDao.deleteBatchIds(ids);
    ```

  * 按主键查询多条记录

    ```java
    List<Long> ids = Arrays.asList(new Long[]{2,3});
    List<User> userList = userDao.selectBatchIds(ids);
    ```

* 逻辑删除

  * 删除操作业务问题：业务数据从数据库中丢弃了无法再找到

  * 逻辑删除：为数据**设置是否可用状态字段**，**删除时设置状态字段为不可用状态**，数据仍保留在数据库中

    * 表中添加逻辑字段（deleted）

    * 实体类中添加对应的字段，并设定当前字段为逻辑删除标记字段

      ```java
      public class User {
      	...
      	@TableLogic(value="0",delval="1")
          private Integer deleted;
      }
      ```

      当字段过多嫌麻烦，可以在yml文件中设置成全局属性

      ```yml
      global-config:
      		banner: false
      		db-config:
      		  id-type: assign_id
      		   prefix: tb_
      		   logic-delete--field: deleted #设置表中逻辑删除的字段
      		   logic-not-delete-value: 0    #设置未删除时表中的默认值
      		   logic-delete-value: 1        #设置删除时的值
      ```

    * 逻辑删除执行的SQL语句为update：update user set deleted=1 where id=？and deleted=0；

    * 被逻辑删除后的数据需要自己写SQL来查询

* 乐观锁

  * 锁——用来解决并发问题

  * 业务并发现象带来的问题：秒杀时，同时多人抢一个商品，可能会有人抢到虚空商品，所以乐观锁可以解决2000人左右的并发问题

    * 数据库表中添加乐观锁对应的字段（version）

    * 实体类中添加对应的字段，并设置当前属性字段为乐观锁字段

      ```java
      public class User {
      	...
      	@Version
      	private Integer version;
      }
      ```

    * 类似于开启分页功能，要在MpConfig配置类中开启乐观锁拦截器实现锁机制对应的动态SQL语句的拼装

      ```java
      public class MpConfig {
          @Bean//分页功能拦截器
          public MybatisPlusInterceptor pageInterceptor() {
              MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();//开启MP的拦截器对象
              interceptor.addInnerInterceptor(new PaginationInnerInterceptor());//添加分页功能拦截器
              mpInterceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());//添加乐观锁拦截器
              return interceptor;
          }
      }
      ```

    * 使用了乐观锁机制在修改前必须要先获取到对应的数据的version字段的值才可以正常进行

      ```java
      	//先查询数据，获取到version数据
      	User user = userDao.selectById(1L);
      	//执行数据修改操作
      	user.setName/setPassword/...
      	userDao.updateById(user);
      ```

      修改时执行的SQL：update user set name=？，age=？，tel=？，version=？+1，where id=？and version=？

      也就是第一个在操作这个数据时，修改了版本，下一个人用刚开始未修改的版本去访问并修改就无法进行了



### 5.快速开发

* 代码生成器
  * 模板：由MP提供/自定义
  * 数据库相关配置：读取数据库获取信息
  * 开发者自定义配置：手工配置
* 这是一个工具，其实自我认为那些业务自己多写写也好，老是用模板代码能力终究会下降，所以多写，这里不详细说明，需要时可以再深入了解





​																																																			——**至此，SSM基础框架完结。©iWyh2**

