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
* name属性表示 选择具体的 要绑定的 是 哪个对象 的名称
* ref属性表示name选择的这个对象要参照的具体类型是什么，也就是另一个Bean的id属性

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

像一个类中传递数据的方式：普通方法（set），构造方法

依赖注入描述了容器建立bean与bean之间的依赖关系的过程

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

* 配置中在bean标签中使用property标签中的ref属性注入引用类型的对象类型，也就是另一个Bean的id，name属性表示 选择具体的 要绑定的 是 哪个对象 的名称

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





## 二.SpringMVC

## 三.Maven高级

## 四.SpringBoot

## 五.MybatisPlus