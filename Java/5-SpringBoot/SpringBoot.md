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

> MyBatis框架：
>
> * 核心配置：数据库连接相关信息
> * 映射配置：SQL映射（XML或者注解开发）
>



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
* Dao开发：MP技术 -继承BaseMapper，设置@Mapper
  * 测试：数据层测试类
* Service开发：MP技术 -继承IService
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





## [知识补充] 前后端数据协议

用于后端与前端进行**数据格式的统一**

将**表现层返回的结果统一为模型类**

根据需求自行设定，没有固定格式

例如：

```java
@Data
public class Data {
	private Boolean flag;
	private Object content;
    
    public Data() {
    }

    public Data(Boolean flag) {
        this.flag = flag;
    }

    public Data(Boolean flag, Object content) {
        this.flag = flag;
        this.content = content;
    }
}
```



前后端分离结构设计中，页面归属前端服务器

单体工程中**页面资源放置在resources目录下的static目录中**（在做页面之前先执行一下maven的clean，防止bug）





## [知识补充] 表现层统一异常处理

将整个项目的异常抛到表现层（controller层）进行集中处理

将所有异常分为三类：

* 系统异常
* 业务异常
* 其他未知异常

根据需求自定义系统异常和业务异常类

```java
@RestControllerAdvice//为REST风格的控制器做增强功能的注解
public class ProjectExceptionAdvice{
    @ExceptionHandler(SystemException.class)//系统异常处理器
    public Result doSystemException(SystemException e) {
        //记录日志
        //发送消息给运维
        //发送邮件给开发人员。e对象也发给开发人员
        return new Result(e.getCode(),e.getMessage());
    }

    @ExceptionHandler(BusinessException.class)//业务异常处理器
    public Result doBusinessException(BusinessException e) {
        return new Result(e.getCode(),e.getMessage());
    }

    @ExceptionHandler(Exception.class)//其他未知异常处理器
    public Result doException(Exception e) {
        //记录日志
        //发送消息给运维
        //发送邮件给开发人员。e对象也发给开发人员
        return new Result(Code.UNKNOWN_ERR,"系统繁忙，请稍后再试");
    }
}
```





# [实用篇]

## [运维实用篇]

## 1. 打包与运行⚙

> 为什么要打包？
>
> ：通俗一点来说，你在你电脑上写完了项目，总不能把你电脑捐给公司当服务器运行项目吧？所以要**将项目打包到公司服务器电脑运行**



### 项目打包-Windows环境

1. 将SpringBoot项目**进行Maven的打包指令：mvn package**

   在**打包之前最好要clean一下**，保证打包的环境是干净的

   同时，由于我们在项目里创建了测试类进行mapper与service的测试，所以在打包执行package指令的时候，maven还会执行test指令去测试，会造成不必要的数据污染，所以我们**要跳过maven的测试环节**（mvn指令：-D skipTests（全部跳过测试）或者IDEA直接点击mavenhelper的窗口执行跳过测试）

2. 运行打包好的项目：在jar包的目录下，在文件路径栏输入cmd，即可直达该目录下，执行启动指令（**java -jar SpringBootxxxx(打包好的jar包名).jar**）（直接**输入首字母再按TAB键自动补全**）

   **jar包支持命令行启动需要依赖maven打包插件**（spring-boot-maven-plugin）支持，需要确保在打包之前项目拥有此插件（否则打包的项目无法独立运行，因为插件会把项目需要的依赖jar包和需要的工具包都会打包在一起）且没有打包插件，打包完的jar包描述文件（MANIFEST.MF）也不一样，会缺少SpringBoot的jar启动器和项目的引导类位置信息



[知识补充] Windows环境端口被占用时解决方案（一般不会用到）：

```properties
#查询端口
netstat -ano
#查询指定端口
netstat -ano |findstr "端口号"
#根据进程PID查询进程名称
tasklist |findstr "进程PID号"
#根据PID杀死任务
taskkill /F /PID "进程PID号"
#根据进程名称杀死任务
taskkill -f -t -im "进程名称"
```



### 项目打包-Linux环境

跳过了这一章节（P56）之后学了Linux补充（2022/11/2)





## 2. 配置高级🛠

> 项目配置的高级操作



### 临时属性设置

> 一般会有风险性，不常用
>
> 可用于加载自定义配置文件



**命令行带属性参数启动SpringBoot项目**：java -jar SpringBootxxxx(打包好的jar包名).jar **--属性1=值 --属性2=值...**

例如设置端口号：java -jar SpringBootxxxx(打包好的jar包名).jar --server.port=80

多个属性启动时，属性之间用空格分隔



命令行设置的**临时属性的传参入口就是SpringBoot的引导类main方法的args参数**，没有这个参数那么就不可以设置临时属性



【属性加载优先顺序】 [参考官网](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config)（命令行 > 配置文件）





### 配置文件

> 开发时我们会自己写一套配置，但是在项目上线之前，项目经理会修改这些配置
>
> 为了方便修改配置，所以SpringBoot有着配置文件分级



配置文件的四级：

* 一级（最高级）：**项目jar包所处目录下的config目录(自己建的)中**的配置文件
* 二级：**项目jar包所处目录下**的配置文件
* 三级：（IDEA中）**项目放置配置文件的resources目录下的config目录(自己建的)中**的配置文件
* 四级（最低级）：SpringBoot给你自动创建的配置文件的位置（**resources目录**）

作用：

* 一级与二级作为系统打包后设置的通用属性
  * 一级一般是运维经理进行线上整体项目部署方案调控
* 三级与四级用于系统开发阶段设置通用属性
  * 三级一般用于项目经理进行整体项目属性调控



【注】已知级别：properties > yml > yaml

【注】classpath指的是资源文件路径





### 自定义配置文件

> 防止一些老六同事为项目开后门



**可以更改配置文件默认的application名称**

更改之后若是启动项目的话，Springboot会去搜寻叫application的配置文件加载

此时我们可以**用启动参数指定加载我们更改名字之后的秘密配置文件**（也就是设置临时属性**spring.config.name**=xxx，或者**spring.config.location**=classpath:/xxx.yml）

命令行设置或者**IDEA的编辑运行/调试配置中设置程序实参**

可以**同时设置多个，但是最后一个才生效，覆盖思想**（多配置文件用于将配置进行分类，进行独立管理，将可选配置单独制作，便于上线更新维护）



说明：

单服务器项目：使用自定义配置文件的需求很低

多服务器项目：使用自定义配置文件的需求较高，且将所有的配置放在一个目录中统一管理





## 3. 多环境开发💻

> 开发时和上线时的配置环境是不一样的



在一个配置文件application.yml中：

```yml
#公共配置 --配置公共的配置，同时可以设置应用环境
spring:
	profiles:
	  active: pro
--- #分隔环境，区分环境
#设置环境 --配置该环境的独有的配置，不同环境配置加载不同

#生产环境
spring:
	profiles: pro
server:
  port: 80
---
#开发环境
spring:
	profiles: dev
server:
  port: 81
---
#测试环境
spring:
	profiles: test
server:
  port: 82
```





### 自定义多环境文件

> 在一个主配置文件中设置多环境属性会有安全隐患，所以可以将这一份文件一分为四，用独立的配置文件定义环境属性



**application.yml**：

```yml
#公共配置 --配置公共的配置，同时可以设置应用环境
spring:
	profiles:
	  active: pro
```

**application-pro.yml**：

```yml
#生产环境
server:
  port: 80
```

**application-dev.yml**：

```yml
#开发环境
server:
  port: 81
```

**application-test.yml**：

```yml
#测试环境
server:
  port: 82
```



【注】properties类文件和yaml类文件是一样设置的，只是后缀和书写格式不一样，其余的都一样，包括属性名



多环境开发配置文件书写：

* 主配置文件设置公共属性
* 环境配置文件设置冲突属性（独特的属性）

* 可以根据功能对配置文件的配置信息进行拆分，制作为独立的配置文件（也就是控制文件的细粒度）

  * 如：**application-devDB.yml**/**application-devRedis.yml**/**application-devMVC.yml**

  * 在主配置文件中，用**include属性**，在激活指定的环境下，同时对多个环境进行加载并使其生效（多个环境之间用逗号隔开）

    例如：

    ```yml
    spring:
      profiles:
        active: dev #在开发环境下（主环境）
        include: devDB,devRedis,devMVC #加载开发环境下的DB Redis MVC环境
    ```

    

    【注】当主环境dev与其他小环境有相同属性时，主环境的生效；其他小环境有相同属性时，则后加载的生效（覆盖）

    【注】SpringBoot**2.4版本开始使用group属性代替include**

    例如：

    ```yml
    spring:
      profiles:
        active: dev #在开发环境下
         group: 
        	"dev": devDB,devRedis,devMVC
        	"pro": proDB,proRedis,proMVC
           "test": testDB,testRedis,testMVC
    ```





### 多环境开发控制

> 假设SpringBoot和Maven都设置了配置环境，那么SpringBoot以Maven配置的环境为主



如果Maven设置了多环境属性，同时还设置了多环境变量（如：profile.active）

那么在SpringBoot中引用这个变量即可：

```yml
spring:
	profiles:
	  active: @profile.active@ #引用maven中设置的环境变量（用@..@占位符）
```





### [知识补充] Maven多环境设置

在pom文件中配置多环境属性：

```xml
<!--定义多环境-->
<profiles>
	<!--定义具体生产环境-->
	<profile>
		<!--定义环境唯一id-->
		<id>env_dev</id>
		<!--定义环境中专用自定义属性-->
		<properties>
            <!--设置环境变量-->
            <profiles.active>dev</profiles.active>
			<jdbc.url>jdbc:mysql:///db1?useServerPrepStmts=true</jdbc.url>
		</properties>
		<!--设置默认启动-->
        <activation>
			<activeByDefault>true</activeByDefault>
        </activation>
	</profile>
    <!--定义具体生产环境-->
    <profile>
		<!--定义环境唯一id-->
		<id>env_test</id>
        <!--定义环境中专用自定义属性-->
		<properties>
            <!--设置环境变量-->
            <profiles.active>test</profiles.active>
			<jdbc.url>jdbc:mysql://127.1.1.1:3306/db1?useServerPrepStmts=true</jdbc.url>
        </properties>
    </profile>
    <!--定义具体生产环境-->
    <profile>
        <!--定义环境唯一id-->
		<id>env_pro</id>
        <!--定义环境中专用自定义属性-->
		<properties>
            <!--设置环境变量-->
            <profiles.active>pro</profiles.active>
			<jdbc.url>jdbc:mysql://127.2.2.2:3306/db1?useServerPrepStmts=true</jdbc.url>
		</properties>
	</profile>
</profiles>
```





## 4. 日志📝

> 日志作用：
>
> * 编程期间调试代码
> * 经营期间记录信息
>   * 记录日常运营信息（峰值流量）
>   * 记录应用报错信息（错误堆栈）
>   * 记录运维过程数据（扩容、宕机、报警）
>
> 
>
> SpringBoot默认使用**Logback日志框架**



### 日志基础

1. 在某一层的类中定义创建日志对象：（需要优化）

   ```java
   public static final Logger log = LoggerFactory.getLogger(产生日志对象的类名.class);
   
   //如：
   public static final Logger log = LoggerFactory.getLogger(BookController.class);
   ```

2. 在控制台输出日志信息：

   * log.debug("");
   * log.info("");
   * log.warn("");
   * log.error("");



日志级别：TRACE < DEBUG < INFO < WARN < ERROR < FATAL(归入到ERROR)

设置日志输出级别：

```yml
#开启SpringBoot的debug模式，输出springboot的调试信息，检查系统运行状况
debug: true

#设置日志输出级别
logging:
  gruop: #设置分组
   ebank: com.wyh.controller,com.wyh.dao
   iservice: com.wyh

  level:
    root: debug #root表示根节点，也就是整体应用的日志级别
    com.wyh.controller: debug #设置某个包的日志级别
    ebank: debug #(先设置分组) 为对应的组设置日志输出级别
```





### [知识补充] 优化日志对象的创建

> 在每一层的类中定义创建一个日志对象耦合度很高且不优雅美观



使用**Lombok提供的注解**：**@Slf4j** 简化开发，减少日志对象的声明创建操作

将@Slf4j注解添加在需要做日志输出的类上，然后**调用提供的log对象输出日志**即可





### 日志输出格式控制

默认的日志格式：时间+级别+PID+所属线程+所属类/接口名+日志信息

* PID：进程的ID，用于表明当前操作所处的进程，当多服务同时记录日志时，该值可用于协助程序员调试程序
* 所属类/接口名：显示的信息为Springboot重写后的信息，名称过长会被简化



设置日志输出格式：

```yml
logging:
	pattern:
	  console: %d %clr(%p) --- [%16t] %clr(%-40.40c){cyan} : %m %n
```

* %d：日期 | %m：消息 | %n：换行 | %p：日志级别 | %clr()：颜色 | %t：线程名 | %c：类名
* 加上数字代表占多少位





### 日志文件

> 日志总不能一直在控制台的吧？所以要记在文件里面



设置日志文件：

```yml
logging:
	file: #设置日志文件 -server.log  -在当前项目的同级目录下产生
	  name: server.log
	logback:
		rollingpolicy: #设置日志文件的详细配置 -滚动策略 循环记录日志
		  max-file-size: 3KB #设置日志的最大大小
		  file-name-pattern: server.%d{yyyy-MM-dd}.%i.log #设置文件的名字格式（%i为序号）
```





## [开发实用篇]



### 1. 热部署🔧

> 热部署也就是想更改了项目之后立马就生效，相当于不停服更新



#### 手动启动热部署

服务器是在SpringBoot里面的一个类了，所以服务器是受SpringBoot管控的，那么我们需要用到SpringBoot提供的工具



添加开发者工具：（内含热部署工具）

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```

修改了项目里面的配置之后，不关掉服务器的情况下激活热部署更新：ctrl+F9（IDEA的build project）



【注】重启（Restart）：重新加载自定义的开发代码（包含类，页面，配置文件）加载位置在restart类加载器

【注】重载（ReLoad）：重新加载jar包，加载位置在base类加载器

【注】**热部署是执行的重启**，**加载开发者自定义资源**，**不加载jar包**





#### 自动启动热部署

> 手动启动依然麻烦，自动最好



设置自动热部署：

**IDEA设置 -> 构建、执行、部署 -> 编译器 -> 勾选 自动构建项目**

**IDEA设置 -> 高级设置 -> 编译器 -> 勾选 即使项目在运行也自动make**

这样设置好之后，那么**在IDEA失去焦点之后的五秒内可以自动构建**





#### 热部署范围配置

默认**不触发热部署**的目录列表：

* /META-INF/maven
* /META-INF/resources
* /resources
* /static
* /public
* /templates



自定义不参与热部署的文件或者文件夹：

```yml
devtools:
   restart:
     exclude: public/**,static/**
```





#### 关闭热部署

> 热部署只在开发环境有效



关闭热部署：

```yml
devtools:
	restart:
		enabled: false
```



【注】有些时候会有人在其他地方设置，然后开启了这个热部署，所以我们需要考虑**属性加载顺序**（[参考官网](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config)），在高级的一层覆盖掉低级一层的设置，让启动热部署失效





### 2. 配置高级🛠



#### @ConfigurationProperties

> 在YAML文件中，在Java类上添加**@ConfigurationProperties**注解，**指定想封装的部分属性名**（prefix）是什么，好针对性的封装
>
> **针对性读取YAML中想要的对象属性**（**部分属性读取**）



除此之外，@ConfigurationProperties可以**为第三方bean绑定属性**

也就是读取YAML文件中的属性，然后添加到对应的bean中

`@ConfigurationProperties(prefix="xxxx")`：prefix为YAML中对应的属性对象的属性名



【注】**@EnableConfigurationProperties注解**：**将添加了@ConfigurationProperties注解的类加入到Spring容器中**，因此添加了@ConfigurationProperties注解的类上面**无需再添加@Component注解将其添加到Spring容器中**（也就是@EnableConfigurationProperties不能与@Component同时使用）

格式：@EnableConfigurationProperties({类数组})（如：@EnableConfigurationProperties({ServerConfig.class})）

【注】解除使用@ConfigurationProperties注释的警告：添加如下依赖坐标即可

```xml
<dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-configuration-processor</artifactId>
</dependency>
```





#### 宽松绑定/松散绑定

**@ConfigurationProperties**绑定属性**支持属性名宽松绑定**

也就是支持属性名为**驼峰、下划线、中划线、常量大写**

且在@ConfigurationProperties**注解内的prefix**有命名规范：**仅能使用纯小写字母，数字，下划线**



【注】@Value注解不支持松散绑定





#### 常用计量单位绑定

> SpringBoot支持JDK8提供的时间与空间计量单位
>
> 用于在需要绑定属性的属性类内，表示具体大小单位



表示时间范围的类：**Duration**（一般用于表示服务器超时时间）

* **默认单位为秒(s)**，可以在类属性上添加@DurationUnit注解，设置时间的单位（年月日时分秒等）

表示空间大小的类：**DataSize**（一般用于表示存储空间的大小）

* **默认单位为Byte(B)**，可以在类属性上添加@DataSizeUnit注解，设置空间单位（B、KB、GB、TB等）





#### 数据校验

> 开启数据校验有助于系统的安全性
>
> J2EE规范中的**JSR303规范**定义了一组有关数据校验的相关API



对于@ConfigurationProperties注解读取的YAML文件的数据，我们需要校验数据的合法性

1. 首先添加JSR303规范（这是能校验数据的大前提）

   ```xml
   <dependency>
         <groupId>javax.validation</groupId>
         <artifactId>validation-api</artifactId>
   </dependency>
   ```

2. 为需要校验的字段添加校验注解：比如@Max、@Min等，为校验规则

3. 规范只是校验的大前提，并不是实现，没有实现会报错，所以我们还需要添加校验的实现：Hibernate校验框架

   ```xml
   <dependency>
         <groupId>org.hibernate.validator</groupId>
         <artifactId>hibernate-validator</artifactId>
   </dependency>
   ```

4. 最后，完成以上操作依然不可以执行校验，因为还需要我们为需要校验的类**开启校验功能**（也就是**添加开启校验注解：@Validated**）





#### [知识补充] YAML文件的字面值表达式

在YAML文件中：字面值有着特定的表达式

```yml
boolean: TRUE                          #TRUE，true，FALSE，fasle，True，False都可以
float: 3.14							   #6.8523015e+5 支持科学计数法
int: 123							   #支持二进制、八进制、十六进制（0(0-7)为八进制、0x(0-9 a-f)为十六进制）
null: ~								   #null用~表示
string1: HelloWorld                    #字符串可以直接书写
string2: "Hello\nWorld"				   #有特殊字符的字符串要用双引号包裹（官方推荐字符串都用双引号包裹）
date: 2002-09-20					   #日期格式必须是yyyy-MM-dd格式
datetime: 2002-09-20T13:14:52+08:00    #时间和日期之间用 T 连接，+后为时区
```



【注】比如数据库连接是输入的密码，假设为纯数字，且为0开头的纯数字。比如为020920，那么会将这个前几位转为八进制，最终导致连接数据库失败。**最简单的解决办法就是将纯数字密码用双引号包裹起来**





### 3. 测试🔩

> 测试时往往需要设置一些专用的测试属性
>
> 那么我们需要在测试的类中测试这些专用值，且不影响项目的其他环境



#### 加载测试专用属性

**@SpringBootTest注解**中，有一个**properties属性**，可以**设置测试环境专用的属性**（可以覆盖掉配置文件中的一些属性）

* 影响范围小，仅对当前测试类有效

**@SpringBootTest注解**中，有一个**args属性**，可以**设置测试环境专用的命令行传入参数**（可以用于模拟命令行传参）



例如：

```java
@SpringBootTest(properties = {"test.prop=testValue2"})
// or @SpringBootTest(args = {"--test.prop=testValue3"})
class PropertiesAndArgsTest {
    @Value("${test.prop}")
    private String msg;

    @Test
    void testProperties() {
        System.out.println(msg);
    }

}
```



【注】application.yml < properties属性设置的值 < args属性设置的参数





#### 加载测试专用配置

> 加载一些临时的配置在测试中使用



**@Import注解**可以为当前测试**加载专用配置**

也就是假设**在test文件下（不是main）创建了配置类**，内含一些bean，添加@Bean注解将bean归为Spring管控

然后**用@Import注解将这个配置导入**，即可**让这个配置在这个测试类中使用，且不会影响到其他的测试**（配置类中不需要添加@Configuration注解，添加了这个的话，不需要@Import导入也会让配置生效）





#### Web环境模拟测试

> 要对表现层做测试，那么首先需要在测试时模拟出一个web环境



**@SpringBootTest注解**中的**webEnvironment属性**，可以设置启用web环境，默认为NONE（也就是不启动web）

* DEFINED_PORT：以定义的端口启动web环境
* RANDOM_PORT：以随机端口启动web环境



**有了Web环境之后**，我们才可以进行表现层请求测试：

1. 开启虚拟MVC调用：添加**@AutoConfigureMockMvc注解**
2. 注入虚拟MVC调用对象：@Autowired **MockMvc** mvc（上个注解提供的调用对象就是MockMvc）
3. 创建虚拟请求，设置访问路径：用**工具类（MockMvcRequestBuilder）**创建实现类之一的**MockHttpServletRequestBuilder**，然后用具体的get、post、put、delete方法，传入路径即可
4. 执行请求：调用perform方法，传入调用对象

例如：

```java
@SpringBootTest(webEnvironment = SpringBootTest.WebEnvironment.RANDOM_PORT)
@AutoConfigureMockMvc//1.
public class WebTest {
    @Test
    public void testBookController(@Autowired MockMvc mvc//2.) {
        MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");//3.
        try {
            mvc.perform(builder);//4.
        } catch (Exception e) {
            e.printStackTrace();
        }
    }
}
```





如果我们想知道这个请求是否成功，我们需要看他的状态码（比如未找到就是404）或者响应体是否符合预期

我们可以进行**虚拟请求状态匹配**：

1. 定义执行状态匹配器：用**MockMvcResultMatchers工具类**，调用**status**方法，即可获得**StatusResultMatchers**对象（假设为status）
2. 用这个执行状态匹配器，定义预期的执行状态（成功还是失败）：直接使用**StatusResultMatchers**对象执行对应方法即可，获得一个预期执行结果**ResultMatcher**
3. 测试执行请求（perform方法），会返回一个真实执行结果**ResultActions**
4. 用真实执行结果和预期执行结果进行比较，比如比较是否相等（andExpect方法）

例如：

```java
@Test
public void testStatus(@Autowired MockMvc mvc) throws Exception {
    StatusResultMatchers status = MockMvcResultMatchers.status();//1.

    ResultMatcher ok = status.isOk();//2.

    MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
    ResultActions resultActions = mvc.perform(builder);//3.
        
    resultActions.andExpect(ok);//4.
}
```



**虚拟响应体匹配**：

与请求状态匹配大致步骤一样

1. 定义执行结果匹配器：用**MockMvcResultMatchers工具类**，调用**content**方法，即可获得**ContentResultMatchers**对象（假设为content）
2. 用这个结果匹配器，定义预期的执行结果（内容String是什么）：直接使用**ContentResultMatchers**对象执行对应方法即可，获得一个预期执行结果**ResultMatcher**
3. 测试执行请求（perform方法），会返回一个真实执行结果**ResultActions**
4. 用真实执行结果和预期执行结果进行比较，比如比较是否相等（andExpect方法）

例如：

```java
@Test
public void testReposeBody(@Autowired MockMvc mvc) throws Exception {
    ContentResultMatchers content = MockMvcResultMatchers.content();

    ResultMatcher resultMatcher = content.string("Change the World!");//string方法是将响应的内容转为字符串再比较

    MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
    ResultActions resultActions = mvc.perform(builder);

    resultActions.andExpect(resultMatcher);
}
```



**虚拟响应体JSON匹配**：

也就是将响应体匹配的string方法换为json方法，传入json字符串即可

例如：

```java
@Test
public void testJSONResponseBody(@Autowired MockMvc mvc) throws Exception {
    ContentResultMatchers content = MockMvcResultMatchers.content();

    ResultMatcher resultMatcher = content
        .json("{\"name\":\"springBoot\",\"type\":\"springBoot\",\"description\":\"springBoot\",\"id\":1}");

    MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
    ResultActions resultActions = mvc.perform(builder);

    resultActions.andExpect(resultMatcher);
}
```



**虚拟响应头匹配**：

与其他方法异曲同工，换了相应的方法和类而已

```java
@Test
public void testResponseHead(@Autowired MockMvc mvc) throws Exception {
    HeaderResultMatchers header = MockMvcResultMatchers.header();

    ResultMatcher resultMatcher = header.string("Content-Type", "application/json");

    MockHttpServletRequestBuilder builder = MockMvcRequestBuilders.get("/books");
    ResultActions resultActions = mvc.perform(builder);

    resultActions.andExpect(resultMatcher);
}
```





#### 业务层测试回滚

> 在打包时，如果没有跳过maven测试，会进行测试环节，去执行业务层和数据层的测试类，也就会造成数据污染
>
> 我们希望不用跳过测试环节，且不会造成数据污染



所以我们需要在业务层测试类上加上注解**@Transactional**：也就是为测试类开启事务，@SpringBootTest注解和这个事务注解一起，就会让SpringBoot检测到你这是在测试业务层，就会将测试用例的事务**进行回滚**，所以不会再污染数据，也就是执行成功了，但没有提交（所以id会被占用）

* 如果希望测试用例的事务提交，那么添加@Rollback注解，这是设置回滚的注解，**@SpringBootTest加@Transactional相当于默认设置了@Rollback(true)**，**需要提交数据的话，那么只需要设置为@Rollback(false)**即可





#### 测试用例数据设定

采取随机值进行测试，用SpringBoot提供的随机数（在yml中打出**random**即可）替换固定数据测试

```yaml
testCase:
   book:
   	id1: ${random.int}           #随机整数
   	id2: ${random.int(10)}	     #10以内整数
   	type: ${random.int(10,20)}   #10-20以内的随机数
   	uuid: ${random.uuid}         #随机uuid
   	name: ${random.value}        #随机字符串，会用MD5加密，看到的是一堆数字
   	publishTime: ${random.long}  #随机整数（long范围的）
```

【注】（）为分隔符，[]或者@@或者！！都可以





### 4. 数据层解决方案📑

> 现有的数据层解决方案：
>
> Druid+MyBatis(或者MP)+MySQL
>
> 数据源：DruidDataSource
>
> 持久化技术：MyBatis
>
> 数据库：MySQL



#### SQL

> 依赖关系型数据库解决数据存储



SpringBoot提供了三种内嵌的数据源对象：

* **HikariCP**（SpringBoot推荐默认使用这个）

  * 默认的数据源配置就是Hikari的配置

    ```yaml
    spring:
      datasource:
          driver-class-name: com.mysql.cj.jdbc.Driver
          url: jdbc:mysql://localhost:3306/db1?serverTimezone=UTC
          username: root
          password: '020920'
          hikari: #为hikari写专有配置
            maximum-pool-size: 50
    ```

    

* Tomcat提供的数据源

* Commons DBCP



SpringBoot默认内置持久化技术：**JdbcTemplate**

使用的话需要导入依赖：

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>
```

还可以设置JdbcTemplate配置：

```yaml
spring:
	jdbc:
	  template:
	  	query-timeout: -1  #查询超时时间
	  	max-rows: 500      #最大行数
	  	fetch-size: -1     #缓存行数
```



SpringBoot提供三种内嵌数据库：（可以在内存运行，小巧，运行速度块，**可以用于测试**）         

* **H2**

  * 导入H2相关坐标依赖：

    ```xml
    <dependency>
    	<groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jpa</artifactId>
    </dependency>
    <dependency>
    	<groupId>com.h2database</groupId>
        <artifactId>h2</artifactId>
        <scope>runtime</scope>
    </dependency>
    ```

  * 设置web项目，配置H2管理控制台参数

    ```yml
    spring:
    	datasource:
    		driver-class-name: org.h2.Driver   
    		url: jdbc:h2:~/test
    		#driver-class-name和url在浏览器访问H2数据库时可以看到，我们需要先将这个配置到数据源中，不然会访问H2数据库失败
    		#访问用户名默认sa 密码123456
    		username: sa
    		password: '123456'
    	h2:
    		console:
    			path: /h2  #在浏览器访问H2数据库的地址
    			enabled: true  #让H2数据库可以在浏览器访问（仅用于开发阶段，上线则必须设置为false）
    ```

    

* HSQL

* Derby

【注】SpringBoot可以根据url地址自动识别出数据库驱动类，因此可以省略driver-class-name





#### NoSQL

> 不是依赖SQL的数据存储
>
> 目前常见的NoSQL解决方案：
>
> * **Redis**
> * **Mongo**
> * **ES**
>
> 这些技术通常在Linux环境下安装部署
>
> 这里做简单了解



**Redis**：

是一款**key-value存储结构的内存级NoSQL数据库**

* 支持多种数据存储格式
* 支持持久化
* 支持集群

Redis启动：

* 服务端启动：`redis-server.exe redis.windows.conf`
* 客户端启动：`redis-cli.exe`

【注】在Windows环境启动会有bug，需要先用客户端shutdown一下然后exit退出，再启动服务端，再启动客户端









### 5. 整合第三方技术🌐

### 6. 监控🔍







# [原理篇]