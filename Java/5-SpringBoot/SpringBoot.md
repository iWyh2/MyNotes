# SpringBoot

> SpringBoot如今市场占有率极高📊
>
> 是Java开发人员必备技能之一💸
>
> 由Pivotal团队开发
>
> 目的🏷：**简化** Spring应用的 **初始搭建** 和 **开发过程**



# [基础篇]

# 1. 快速上手🚀



## 1.1 SpringBoot入门程序开发



### 四种SpringBoot工程创建的方式

### ① IDEA联网开发版

> 需要联网，保证能下载配置到本地计算机中



开发步骤：

1. **创建新模块，选择`Spring Initializr`**
2. **配置相关信息，注意Java版本要与自己选的版本一致**
3. **选择模块所需技术**
4. **开发...**
5. **运行自动生成的XxxApplication类的main方法**



SpringBoot两个基础配置文件：

* pom.xml
* XxxApplication启动类



### ② 官网创建版

> 避免IDEA无法联网



1. 前往Spring官网，点击SpringBoot一栏
2. 拉到最底下，看到QuickStart your project，点击Spring Initializr（或者直接点击这里[https://start.spring.io/])
3. 内部与IDEA创建时一样的步骤（IDEA就是访问的这个罢了）
4. 下载好模块压缩包
5. 将压缩包内文件压缩或拖动到IDEA项目文件中
6. 点击模块设置，新建模块，导入模块
7. 选择刚刚添加的新文件，然后选择从外部模型导入模块，选择Maven，然后添加即可



### ③ 阿里云创建版

> (https://start.spring.io/)是外国网站，懂的都懂



所以我们可以在IDEA创建boot模块时，默认官网可以更换为阿里云提供的官网：

* 在IDEA的Spring Initializr中的服务器URL更改为[https://start.aliyun.com] 
* 然后就会进入阿里巴巴提供的创建界面
* 之后步骤一样



[注]：阿里巴巴提供的SpringBoot版本很低，根据需求自己更改版本



### ④ 手工创建版

> 当计算机没有联网时，就可以这样创建，前提是，需要的那些坐标你在本地仓库上已经下好了



步骤：

1. 首先创建一个空的Maven模块
2. 然后需要有之前Boot模块的pom.xml文件中的parent标签内的属性 以及所需的快速启动依赖，复制过来即可
3. 然后需要自己创建Application启动类，这个类需要有**@SpringBootApplication**注解，main方法内只有一个SpringApplication.run方法，内放这刚刚创建的启动类的字节码
4. 然后开发...
5. 然后运行启动类





## 1.2 入门程序的工作原理

早期Spring工程缺点：

* 依赖设置繁琐
* 配置繁琐

SpringBoot程序优点：

* 起步依赖（简化依赖配置）
* 自动配置（简化常用工程相关配置）
* 辅助功能（内置服务器等）

SpringBoot开发对比Spring开发：

|        类/配置文件         |  Spring  |  SpringBoot  |
| :------------------------: | :------: | :----------: |
|    **pom文件中的坐标**     | 手工添加 | **勾选添加** |
|      **web3.0配置类**      | 手工添加 |      无      |
| **Spring/SpringMVC配置类** | 手工添加 |      无      |
|         **控制器**         | 手工添加 |   手工添加   |





入门程序解析：

1. **parent**（Maven继承）

​	继承了spring-boot-starter-parent这个pom文件，内置一个xxx.dependencies的pom文件，里面的属性全是版本

​	也就是定义了若干个依赖管理（坐标版本号）

​	继承这个spring-boot-starter-parent模块，就可以避免多个依赖之间出现依赖版本冲突问题，也就是**依赖的版本交给SpringBoot管理了**

​	除了parent继承之外，还可以用**引入依赖**的形式实现这样的效果（阿里云版本就是这样实现的）

​	**目的：减少依赖冲突**



2. **starter**（快速启动）

​	**内置了当前技术需要使用的所有的依赖**（Maven依赖传递）

​	**目的：减少依赖配置**

* 以上两个技术也就是让我们最终在实际开发中，只需要GAV的G和A，V(版本)由SpringBoot提供，未收录的技术需要自己手动添加GAV（**解决配置问题**）
* 坐标错误需要指定V



3. **引导类**（XxxApplication）

​	由**SpringBoot自动帮你创建**的一个类

​	引导类是boot工程的执行入口，运行引导类的main方法就可以**启动boot项目**

​	运行之后，会**初始化Spring容器（ApplicationContext），会扫描引导类所在的包去加载bean**



4. **内嵌Tomcat**（**辅助功能**）

​	内置tomcat服务器的依赖

​	将tomcat容器抽象为一个对象放在Spring容器中运行

​	不想用默认的tomcat，**可以用Maven的排除依赖排除，然后重新设置依赖（或者称之为 某技术的starter）**

​	比如，可以将tomcat更换为**jetty**服务器（Maven私服的Nexus就是用的这个服务器）

​	SpringBoot内置三款服务器：1) tomcat  2) jetty 3) undertow，了解即可（tomcat yyds😏）





# 2. 基础配置🛠



## [知识补充] 复制工程

> 原则：
>
> * 保留工程基础结构
> * 抹掉原始工程痕迹



1. 在工作空间（也就是放IDEA的Java文档的地方）中复制一个Boot工程，并修改名称为备份
2. 删除与IDEA相关配置文件。仅保留src文件和pom.xml文件
3. 修改pom.xml文件中的artifactId与这个备份工程名称一致，之后复制也要修改这个
4. 备份文件就保留在工作空间中，供之后使用



## [知识补充] IDEA的Boot配置文件中属性提示消失解决办法

* setting -> Project Structure -> Facets
* 选中对应的工程，点击boot配置文件一栏
* 点击 ‘+’ 将配置文件添加进boot工程即可





## 2.1 属性配置

> SpringBoot默认配置文件为：**application.properties**（通过**键值对配置对应属性**）



修改配置：（也就是将想要修改的写在properties文件中，然后自己赋值。在IDEA中，我们可以直接书写关键字，然后靠提示寻找需要的配置）

* 修改服务器端口：

  ```properties
  server.port=80
  ```

* 关闭SpringBoot的banner

  ```properties
  spring.main.banner-mode=off
  ```

* 控制日志打印（默认为 - info）

  ```properties
  logging.level.root=error
  ```

【注】对于某些技术的配置的修改或者添加配置，是需要当前这个工程有这项技术依赖，否则无法修改或添加配置。更多配置请参考Spring官网中的SpringBoot的Application Properties





## 2.2 配置文件

> SpringBoot提供了三种配置文件：
>
> 1. properties（传统，默认格式）
> 2. yml
> 3. yaml

现在企业主流是**.yml**文件

假设三种文件共存，加载优先级：properties > yml > yaml，**相同的属性配置会有优先级，不同的会保留**



### [知识补充] YAML文件

> YAML 是一种数据序列化格式，是一种语言
>
> 优点：
>
> * 容易阅读
> * 易与脚本语言交互
> * 以数据为核心，重数据轻格式



YAML文件扩展格式：

1. **yml（主流）**
2. yaml



语法规则：

* 大小写敏感
* 属性层级关系使用多行描述，每行结尾用冒号结束
* 使用**缩进表示层级关系**，**同层级左侧对齐**，**只允许使用空格**，不允许Tab键
* 属性值前面添加空格（属性名: 属性值），数据前面要**加空格**与冒号隔开
* #代表注释

```yaml
简单介绍：
#YAML数组
likes:
  - 唱
  - 跳
  - rap
  - 篮球
或者：
likes: [唱,跳,rap,篮球]
  
#YAML对象
user:
  name: wyh
  age: 20
  
#YAML对象数组
users:
  - name: wyh
    age: 20
  - name: zyh
    age: 19
或者：
users: [{name:wyh,age:19},{name:zyh,age:19}]
```



**YAML文件数据读取**：

* **在boot工程中**（也就是Java代码中）用**@Value注解读取**YAML文件中的属性：**@Value("${一级属性名.二级属性名...}")**，**用于单个属性注入**

  * **YAML中全部属性读取**方法：SpringBoot**将YAML文件封装为了一个Environment类**对象，用**@Autowired将对象自动封装注入**，只需要**调用方法即可读取想要的属性**

    ```java
    @Autowired
    private Environment environment;
    //-------------------------
    environment.getProperty("一级属性名.二级属性名...");
    ```

  * **针对性读取YAML中想要的对象属性**（**部分属性读取**）：(这个一般用于某些框架技术配置)

    1. 将想要的YAML对象属性封装为一个Java类，用于接收数据，类中的属性名要与YAML中的一致，不然无法读取
    2. 在Java类上添加**@ConfigurationProperties**注解，**指定想封装的部分属性名**（prefix）是什么，好针对性的封装
    3. 再在Java类上添加**@Component**注解，让这个类成为bean，交给Spring容器管理，Spring才会将数据封装到这个类中
    4. 然后用这个类创建对象调用里面的属性即可，注意还要**@Autowired**

    ```java
    @Component//将这个类变为bean 交给Spring容器管理 Spring才会将YAML中的数据封装到这个类中
    @ConfigurationProperties(prefix = "enterprise")//prifix可写可不写 针对性的封装
    public class Enterprise {
        private String name;//属性名必须与YAML文件中的一致
        private Integer age;
        private String[] subjects;
    }
    //--------------------------
    @Autowired
    private Enterprise enterprise;
    ```



* **在YAML文件中**，定义一个属性，想**在这个文件的别的地方使用这个属性**，用**${一级属性名.二级属性名...}**读取



【注】1. 当想要YAML文件中数组的某个元素时，**用"[]"取下标**即可（如：likes[0]）

​			2. 在YAML中，字符串只有**用双引号引起来的才是字符串**，且转义字符只有在字符串内才会生效





# 3. 整合第三方技术🌐

> 用SpringBoot，即可整合所有第三方技术，这是需要掌握的核心思想与技术



## 整合JUnit

对于JUnit，SpringBoot自动默认就给你整合了，在Spring的**spring-boot-starter-test**起步依赖中

一般会自动给你创建一个测试类，类上有@SpringBootTest注解修饰

**@SpringBootTest**：

* 测试类注解
* 作用是**定义JUnit的测试类**

用**自动装配的形式**注入测试需要测试的对象



当**测试类和boot工程的引导类不在一个包下时**（test目录的包会和java目录下的同名包会在编译后合并在一起），测试类会报错

意为找不到SpringBoot的配置类，因为**测试所需的那些对象终究是需要Spring容器为我们注入的，找不到Spring容器了肯定会报错**

解决方法：

* **@SpringBootTest**注解的**classes属性**：作用是指定设置加载SpringBoot的启动类

  * **@SpringBootTest(classes = boot工程启动类.class)**

    说白了，当他们**在同一个包下时，就是省略了classes这个属性**





## 整合MyBatis📌

MyBatis框架：

* 核心配置：数据库连接相关信息
* 映射配置：SQL映射（XML或者注解开发）



SpringBoot整合时：

* 首先在创建工程时就应该要**勾选相关技术**：**MyBatis Framework** 和 **MySQL Driver**（导入快速启动依赖）
* 然后需要在SpringBoot的核心配置文件**application.yml文件中配置连接数据库的相关信息**（设置数据源参数）：Driverclass url username password等
* 在数据层的接口上，要**添加注解@Mapper**（让数据库SQL映射被容器识别到，创建自动代理对象）



【注】1. 在YAML文件中，如果你的MySQL**数据库密码为纯数字，那么记得要加上单引号**（比如：password='66666'）

​			2. 如果SpringBoot给你提供的**MySQL版本为8.x，那么会被强制要求设置时区**：修改url，添加**serverTimezone=UTC**即可





## 整合MyBatisPlus

> MP是由国人团队开发的技术



MP没有被收录在Spring的官方项目里，所以没有直接提供技术勾选

但是我们在IDEA选择阿里云的构建SpringBoot的网址，就可以直接勾选MP技术，为MybatisPlus Framework



整合MP：**和整合Mybatis一致**，**唯一不同点在于在定义数据层接口与SQL映射**时，我们不需要再自己写SQL语句了，哪怕是注解也不需要了，只需要这个数据层接口**继承MP提供的BaseMapper**即可，泛型传入数据类型。MP内置许多已经定义好的SQL查询方法



【注】对于没有被Spring官网收录的技术，我们需要自己手动添加相应技术坐标，可以去mvnrepository查询，对于boot工程，我们需要starter即可





## 整合Druid

> 阿里巴巴提供的数据源，为MyBatis使用



Druid也没有被收录，可惜

国人的大部分技术都没有被收录（看来需要我出马了，做出让Spring折服的技术，让他不得不收录😏



整合Druid：

* 既然没有被收录，那就自己导入Druid的starter坐标（在mavenrepository找坐标）

* 在application.yml文件中，指定数据源：

  * 方法一：直接用type指定（没导入starter，但是有技术坐标）

    ```yaml
    spring:
      datasource:
          driver-class-name: com.mysql.cj.jdbc.Driver
          url: jdbc:mysql://localhost:3306/db1?serverTimezone=UTC
          username: root
          password: '020920'
          type: com.alibaba.druid.pool.DruidDataSource
    ```

  * 方法二：导入了starter，那么直接使用

    ```yml
    spring:
      datasource:
        druid:
          driver-class-name: com.mysql.cj.jdbc.Driver
          url: jdbc:mysql://localhost:3306/db1?serverTimezone=UTC
          username: root
          password: '020920'
    ```





**[整合第三方技术的关键点]：**

* **导入starter**
* **设置或更改配置**





# 4. SSM案例📄

> 已经在[SSM基础框架.md]中做过了，这里是相当于再做了一遍



案例分析：

后端：

* 实体类开发：Lombok技术
* Dao开发：MP技术
  * 测试：数据层测试类
* Service开发：MP技术
  * 测试：业务层测试类
* Controller开发：RESTful开发
  * 测试：Postman测试接口
  * 前后端开发协议制定

前端：

* 页面开发：Vue+Element-UI
  * 前后端联调，页面数据处理，页面消息处理

整体功能：

* 列表、新增、修改、删除、分页、查询、条件查询
  * 还有项目中的异常处理





## [知识补充] Lombok

是一个Java类库

提供了一组注解，简化POJO实体类的开发，且被SpringBoot收录了的

* @Getter
* @Setter
* @Constructor
* @AllArgsConstructor
* @NoArgsConstructor
* **@Data**：除了**不提供构造方法**，其他都给你写好了，包括hashCode和equals，一般直接**写这个就够了**





## [知识补充] MP实现业务层快速开发

MP还可以为业务层进行快速开发，提高效率

* 使用**MP提供的业务层通用接口**（**IService\<T>**）和**业务层通用实现类**（**ServiceImpl\<M,T>**）（内置许多业务功能）
* 在通用实现类的基础上做功能的重载或者功能的追加
* 注意重载时不要覆盖原始操作，避免MP提供的原始功能丢失

【注】**泛型M指的是业务层所需的数据层Mapper映射接口**，泛型T指的是操作的具体数据类型









# [应用篇]

# [原理篇]