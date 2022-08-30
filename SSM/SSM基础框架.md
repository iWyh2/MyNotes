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

### 4. IoC入门案例(XML配置版)

#### 4.1 案例思路分析

* 首先，需要管理的是那些一个个耦合度高的Bean，也就是service和dao层对象
* 然后，要将管理的对象放入IoC容器中，那么目前的基础方法为配置xml实现
* 想要获取这个IoC容器，那么需要用Spring提供的接口去获取
* 容器得到后，再用方法去获取这些被管理的Bean对象
* 要使用Spring的技术，那么IDEA肯定要导入坐标

#### 4.2 案例实现

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

5. 



## 二.SpringMVC

## 三.Maven高级

## 四.SpringBoot

## 五.MybatisPlus