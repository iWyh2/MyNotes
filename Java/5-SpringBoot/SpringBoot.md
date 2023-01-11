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
          password: 'xxxxx'
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
          password: 'xxxxxx'
    ```





**[整合第三方技术的关键点]：**

* **导入starter**
* **设置或更改配置**





# 4. SSM案例📄

> 已经在[SSM基础框架.md](D:\Git\GitLocalRepository\Java\3-SSM\SSM基础框架.md)中做过了，这里是相当于再做了一遍



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



详情请看IDEA-SpringBoot2工程的07项目SSM



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
	private Boolean flag;//状态回馈
	private Object content;//数据内容
    
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



### 项目打包-Linux环境*

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
	  active: @profile.active@ #引用maven的pom文件中设置的环境变量（用@..@占位符）
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
            <!--设置环境变量， 标签名称随意-->
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

**将@Slf4j注解添加在需要做日志输出的类上**，然后**调用提供的log对象输出日志**即可





### 日志输出格式控制

默认的日志格式：**时间+级别+PID+所属线程+所属类/接口名+日志信息**

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
	file: #设置日志文件 
	  name: server.log # 在当前项目的同级目录下产生
	logback:
		rollingpolicy: #设置日志文件的详细配置 -滚动策略 循环记录日志
		  max-file-size: 3KB #设置日志的最大大小
		  file-name-pattern: server.%d{yyyy-MM-dd}.%i.log #设置文件的名字格式（%i为序号）
```





## [开发实用篇]



## 1. 热部署🔧

> 热部署也就是想更改了项目之后立马就生效，相当于"不停服更新"



### 手动启动热部署

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



【注】重启（Restart）：**重新加载自定义的开发代码（包含类，页面，配置文件）**加载位置在**restart类加载器**

【注】重载（ReLoad）：**重新加载jar包**，加载位置在**base类加载器**

【注】**热部署是执行的重启**，**加载开发者自定义资源**，**不加载jar包**





### 自动启动热部署

> 手动启动依然麻烦，自动最好



设置自动热部署：

**IDEA设置 -> 构建、执行、部署 -> 编译器 -> 勾选 自动构建项目**

**IDEA设置 -> 高级设置 -> 编译器 -> 勾选 即使项目在运行也自动make**

这样设置好之后，那么**在IDEA失去焦点之后的五秒内可以自动构建**





### 热部署范围配置

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





### 关闭热部署

> 热部署只在开发环境有效



关闭热部署：

```yml
devtools:
	restart:
		enabled: false
```



【注】有些时候会有人在其他地方设置，然后开启了这个热部署，所以我们需要考虑**属性加载顺序**（[参考官网](https://docs.spring.io/spring-boot/docs/current/reference/html/spring-boot-features.html#boot-features-external-config)），在高级的一层覆盖掉低级一层的设置，让启动热部署失效





## 2. 配置高级🛠



### @ConfigurationProperties

> 在YAML文件中，在Java类上添加**@ConfigurationProperties**注解，**指定想封装的部分属性名**（prefix）是什么，好针对性的封装
>
> **针对性读取YAML中想要的对象属性**（**部分属性读取**）



除此之外，@ConfigurationProperties可以**为第三方bean绑定属性**

也就是**读取YAML文件中的属性，然后添加到对应的bean中**

`@ConfigurationProperties(prefix="xxxx")`：prefix为YAML中对应的属性对象的属性名



【注】**@EnableConfigurationProperties注解**：**将添加了@ConfigurationProperties注解的类加入到Spring容器中**，因此添加了@ConfigurationProperties注解的类上面**无需再添加@Component注解将其添加到Spring容器中**（也就是@EnableConfigurationProperties不与@Component同时使用）也就是哪里需要用到有@ConfigurationProperties注解的类的时候，就可以在那里添加这个@EnableConfigurationProperties注解

格式：@EnableConfigurationProperties({类数组})（如：@EnableConfigurationProperties({ServerConfig.class})）

【注】解除使用@ConfigurationProperties注释的警告：添加如下依赖坐标即可

```xml
<dependency>
     <groupId>org.springframework.boot</groupId>
     <artifactId>spring-boot-configuration-processor</artifactId>
</dependency>
```





### 宽松绑定

也叫松散绑定

**@ConfigurationProperties**绑定属性**支持属性名宽松绑定**

也就是支持属性名为**驼峰、下划线、中划线、常量大写**

且在@ConfigurationProperties**注解内的prefix**有命名规范：**仅能使用纯小写字母，数字，下划线**



【注】@Value注解不支持松散绑定





### 常用计量单位绑定

> SpringBoot支持JDK8提供的时间与空间计量单位
>
> 用于在需要绑定属性的属性类内，表示具体大小单位



表示时间范围的类：**Duration**（一般用于表示服务器超时时间）

* **默认单位为秒(s)**，可以在类属性上**添加@DurationUnit注解**，设置时间的单位（年月日时分秒等）

表示空间大小的类：**DataSize**（一般用于表示存储空间的大小）

* **默认单位为Byte(B)**，可以在类属性上**添加@DataSizeUnit注解**，设置空间单位（B、KB、GB、TB等）





### 数据校验

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

2. 

3. 规范只是校验的大前提，并不是实现，没有实现会报错，所以我们还需要添加校验的实现：**Hibernate校验框架**

   ```xml
   <dependency>
         <groupId>org.hibernate.validator</groupId>
         <artifactId>hibernate-validator</artifactId>
   </dependency>
   ```

4. 最后，完成以上操作依然不可以执行校验，因为还需要我们为需要校验的类**开启校验功能**（也就是**添加开启校验注解：@Validated**）





### [知识补充] YAML文件的字面值表达式

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





## 3. 测试🔩

> 测试时往往需要设置一些专用的测试属性
>
> 那么我们需要在测试的类中测试这些专用值，且不影响项目的其他环境



### 加载测试专用属性

**@SpringBootTest注解**中，有一个**properties属性**，可以**设置测试环境专用的属性**（可以覆盖掉配置文件中的一些属性）

**@SpringBootTest注解**中，有一个**args属性**，可以**设置测试环境专用的命令行传入参数**（可以用于模拟命令行传参）                                                                                                                                                                                                                                                      

* 影响范围小，仅对当前测试类有效



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





### 加载测试专用配置

> 加载一些临时的配置在测试中使用



**@Import注解**可以为当前测试**加载专用配置**

也就是假设**在test文件下（不是main）创建了配置类**，内含一些bean，添加@Bean注解将bean归为Spring管控

然后**用@Import注解将这个配置导入**，即可**让这个配置在这个测试类中使用，且不会影响到其他的测试**（配置类中不需要添加@Configuration注解，添加了这个的话，不需要@Import导入也会让配置生效）





### Web环境模拟测试

> 要对表现层做测试，那么首先需要在测试时模拟出一个web环境



**@SpringBootTest注解**中的**webEnvironment属性**，可以设置启用web环境，默认为NONE（也就是不启动web）

* DEFINED_PORT：以定义的端口启动web环境
* RANDOM_PORT：以随机端口启动web环境



**有了Web环境之后**，我们才可以**进行表现层请求测试**：

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





如果我们**想知道这个请求是否成功**，我们需要看他的状态码（比如未找到就是404）或者响应体是否符合预期

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





### 业务层测试回滚

> 在打包时，如果没有跳过maven测试，会进行测试环节，去执行业务层和数据层的测试类，也就会造成数据污染
>
> 我们希望不用跳过测试环节，且不会造成数据污染



所以我们需要在业务层测试类上加上注解**@Transactional**：也就是为测试类开启事务，@SpringBootTest注解和这个事务注解一起，就会让SpringBoot检测到你这是在测试业务层，就会将测试用例的事务**进行回滚**，所以不会再污染数据，也就是执行成功了，但没有提交（所以id会被占用）

* 如果希望测试用例的事务提交，那么添加@Rollback注解，这是设置回滚的注解，**@SpringBootTest加@Transactional相当于默认设置了@Rollback(true)**，**需要提交数据的话，那么只需要设置为@Rollback(false)**即可





### 测试用例数据设定

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





## 4. 数据层解决方案📑

> 现有的数据层解决方案：
>
> Druid+MyBatis(或者MP)+MySQL
>
> 数据源：DruidDataSource
>
> 持久化技术：MyBatis
>
> 数据库：MySQL



### SQL

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

【注】SpringBoot可以根据url地址自动识别出数据库驱动类，因此**可以省略driver-class-name**





### NoSQL

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





#### Redis

> * 支持多种数据存储格式
> * 支持持久化
> * 支持集群
>



[Redis下载](https://github.com/tporadowski/redis/releases)

Redis启动：

* 服务端启动：`redis-server.exe redis.windows.conf`
* 客户端启动：`redis-cli.exe`

【注】在Windows环境启动会有bug，需要先用客户端shutdown一下然后exit退出，再启动服务端，再启动客户端



Redis基础操作：

set、get、hset、hget，操作key-value

清空数据库：flushall



SpringBoot整合Redis：

1. 在创建时**勾选Redis技术**或者导入坐标依赖（导入starter）

   ```xml
   <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-redis</artifactId>
   </dependency>
   ```

2. 配置Redis

   ```yml
   spring:
     redis:
       host: localhost #这是默认的配置，不配也一样
       port: 6379
   ```

3. 操作Redis：用Redis提供的**StringRedisTemplate**（自动装配），使用ops*方法（比如opsForValue），获取各种数据类型操作接口，然后用各种set、get等



【注】RedisTemplate：**以对象作为key和value**，内部对数据进行了序列化

【注】**StringRedisTemplate**：**以字符串作为key和value，与Redis客户端进行的操作一样**，这样对一些数值的操作才不会出现异常（常用）



切换客户端为Jedis：

1. 导入Jedis依赖：

   ```xml
   <dependency>
      <groupId>redis.clients</groupId>
      <artifactId>jedis</artifactId>
   </dependency>
   ```

2. 配置客户端：

   ```yaml
   spring:
     redis:
       host: localhost #这是默认的配置，不配也一样
       port: 6379
       client-type: jedis  #默认为lettcus
   ```





#### MongoDB

> 开源，高性能，无模式的**文档型数据库**
>
> “最像关系型数据库”的**非关系型数据库**



应用场景：

永久性存储，修改频度极低（比如淘宝用户数据）=> 适合存储在数据库中

永久性存储与临时性存储结合，修改频度较高或极高（比如游戏装备游戏道具数据或者直播打赏粉丝数） => 适合存储在数据库或者MongoDB

**临时存储**，**修改频度飞速**（比如物联网数据） => 适合存储在**MongoDB**



[MongoDB下载](https://www.mongodb.com/try/download)

MongoDB启动：

* 服务端启动：`mongod --dbpath=..\data\db`
* 客户端启动：`mongo`

MongoDB可视化客户端：Robo 3T



MongoDB基础CRUD：（了解）

* 增：

  ```sql
  //添加数据(文档）
  //db.book.save({"name": "springboot"})
  db.book.save({"name":"《Java并发编程思想》",type:"Java"})
  ```

* 删：

  ```sql
  //删除数据
  db.book.remove({name:"springboot"})
  ```

* 改：

  ```sql
  //修改数据 --默认只改遇到的第一条数据
  db.book.update({name:"《Java并发编程思想》"},{$set:{name:"《Java并发编程的艺术》"}})
  ```

* 查：

  ```sql
  //查询数据
  //db.getCollection('book').find({})
  db.book.find()
  ```



SpringBoot整合MongoDB：

1. 创建项目时勾选MongoDB技术，或者导入MongoDB的starter：spring-boot-starter-data-mongodb

2. 配置好客户端，连接哪个数据库

   ```yml
   spring:
     data:
       mongodb:
         uri: mongodb://localhost/wyh #默认本地与默认端口即可 后面是具体连接哪个数据库
   ```

3. 自动装配并使用MongoDB提供的连接接口操作MongoDB数据库：**MongoTemplate**，利用这个类进行CRUD即可





#### ES

> ES（Elasticsearch）
>
> 是一个分布式全文搜索引擎
>
> 特点：**倒排索引**(由字段查询到id) 创建文档 使用文档



[ES下载](https://www/elastic.co/cn/downloads/elasticsearch)

ES启动：**点击运行elasticsearch.bat**文件即可



ES基础操作：（了解）

* 索引：

* 创建索引：`PUT	http://localhost:9200/books`
* 查询索引：`GET	http://localhost:9200/books`
* 删除索引：`DELETE	http://localhost:9200/books`

【注】这样创建的索引是没有规则的，需要我们自己添加**分词器**（[IK分词器下载](https://github.com/medcl/elasticsearch-analysis-ik/releases)）

【注】将下载好的**分词器解压到ES的plugins目录下**，因为这是个插件。然后我们在创建索引的时候需要指定规则（在postman的put请求的body中，以json格式指定）

例如：

```json
{
    "mappings":{
        "properties":{
            "id":{
                "type":"keyword"  #以关键字查询
            },
            "type":{
                "type":"keyword"  #以关键字查询
            },
            "name":{
                "type":"text",
                "analyzer":"ik_max_word",   #添加的分词器
                "copy_to":"findWithnameAnddes"  #将保存的字段数据复制到指定的属性中去
            },
            "description":{
                "type":"text",
                "analyzer":"ik_max_word",
                "copy_to":"findWithnameAnddes"
            },
            "findWithnameAnddes":{   #设计型字段，我们想要以name和description字段进行查询，所以用这个保存内容
                "type":"text",
                "analyzer":"ik_max_word"
            }
        }
    }
}
```



* 文档：

* 创建文档：

  ```json
  POST	http://localhost:9200/books/_doc           #使用系统生成的id
  POST	http://localhost:9200/books/_create/1      #使用指定id
  POST	http://localhost:9200/books/_doc/1		   #使用指定id，不存在创建，存在则更新
  并带上json数据
  
  例如：
  {
      "type":"Java",
      "name":"《Java编程思想》",
      "description":"study java"
  }
  ```

* 查询文档：

  ```json
  GET		http://localhost:9200/books/_doc/1         #查询单个文档
  GET		http://localhost:9200/books/_search        #查询全部文档
  ```

  * 条件查询：`GET	  http://localhost:9200/books/_search?q=xxxx:xxxx`

* 删除文档：`DELETE    http://localhost:9200/books/_doc/1`

* 修改文档：

  * 全部修改：`PUT    http://localhost:9200/books/_doc/1`	并带上json格式数据

  * 部分修改：`POST	http://localhost:9200/books/_update/1`

    ```json
    部分修改的JSON格式：比如指定某一字段修改
    {
        "doc":{
            "name":"《Java编程思想》"
        }
    }
    ```

    

SpringBoot整合ES：

1. 勾选ES技术或导入ES依赖

   ```xml
   <dependency>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-data-elasticsearch</artifactId>
   </dependency>
   ```

2. 配置ES

   ```yml
   spring:
      elasticsearch:
        rest:
          uris: http://localhost:9200
   ```

3. 创建客户端并使用：ElasticsearchRestTemplate（这是低等级的客户端）（一般不用）

   高等级的客户端还需要我们自己去导入

   ```xml
   <dependency>
      <groupId>org.elasticsearch.client</groupId>
      <artifactId>elasticsearch-rest-high-client</artifactId>
   </dependency>
   ```

   创建并**自动注入**：**RestHighLevelClient**，然后执行相应的操作

   例如：

   创建索引
   
   ```java
   @Test
   void testAutowiredHighClient() throws IOException {
       CreateIndexRequest request = new CreateIndexRequest("books");//创建 -- 创建索引请求，指定索引名称
       restHighLevelClient.indices().create(request,RequestOptions.DEFAULT);//调用创建索引操作
   }
   ```
   
   创建**带分词器的索引**
   
   ```java
   @Test
   void testCreateIndexByIK() throws IOException {
       CreateIndexRequest request = new CreateIndexRequest("books");
       String json = "{\n" +
               "    \"mappings\":{\n" +
               "        \"properties\":{\n" +
               "            \"id\":{\n" +
               "                \"type\":\"keyword\"\n" +
               "            },\n" +
               "            \"type\":{\n" +
               "                \"type\":\"keyword\"\n" +
               "            },\n" +
               "            \"name\":{\n" +
               "                \"type\":\"text\",\n" +
               "                \"analyzer\":\"ik_max_word\",\n" +
               "                \"copy_to\":\"findWithnameAnddes\"\n" +
               "            },\n" +
               "            \"description\":{\n" +
               "                \"type\":\"text\",\n" +
               "                \"analyzer\":\"ik_max_word\",\n" +
               "                \"copy_to\":\"findWithnameAnddes\"\n" +
               "            },\n" +
               "            \"findWithnameAnddes\":{\n" +
               "                \"type\":\"text\",\n" +
               "                \"analyzer\":\"ik_max_word\"\n" +
               "            }\n" +
               "        }\n" +
               "    }\n" +
               "}";
      request.source(json, XContentType.JSON);//分词器在json中使用，所以以json传入
      restHighLevelClient.indices().create(request,RequestOptions.DEFAULT);
   }
   ```
   
   创建单个文档
   
   ```java
   @Test
   void testCreateDocument() throws IOException {
       Book book = bookDao.selectById(1);
       IndexRequest request = new IndexRequest("books").id(book.getId().toString());
       request.source(JSON.toJSONString(book), XContentType.JSON);
       restHighLevelClient.index(request,RequestOptions.DEFAULT);
   }
   ```
   
   批量创建文档
   
   ```java
   @Test
   void testCreateAllDocument() throws IOException {
       List<Book> books = bookDao.selectList(null);
       BulkRequest bulkRequest = new BulkRequest();//创建批量处理请求
       for (Book book : books) {
           IndexRequest request = new IndexRequest("books").id(book.getId().toString());
           request.source(JSON.toJSONString(book), XContentType.JSON);
           bulkRequest.add(request);//向批量处理请求里面添加单个文档创建的请求
       }
       restHighLevelClient.bulk(bulkRequest,RequestOptions.DEFAULT);//调用批量处理方法
   }
   ```
   
   按id查询单个文档
   
   ```java
   @Test
   void testGet() throws IOException {
       GetRequest request = new GetRequest("books","1");
       GetResponse response = restHighLevelClient.get(request, RequestOptions.DEFAULT);
       System.out.println(response.getSourceAsString());
   }
   ```
   
   按条件查询
   
   ```java
   @Test
   void testSearch() throws IOException {
       //条件设置
       SearchRequest request = new SearchRequest("books");
       SearchSourceBuilder builder = new SearchSourceBuilder();
       builder.query(QueryBuilders.matchPhraseQuery("name","Java"));
       request.source(builder);
       //传入条件请求 获取响应
       SearchResponse search = restHighLevelClient.search(request, RequestOptions.DEFAULT);
       //打印响应
       SearchHits hits = search.getHits();//是否查询到是Hit中的，也就是是否命中，命中的就是按条件查询到的数据
       for (SearchHit hit : hits) {
           String source = hit.getSourceAsString();//想要的数据在Hit的Source中
           Book book = JSON.parseObject(source, Book.class);
           System.out.println(book);
       }
   }
   ```
   
   



## 5. 整合第三方技术🌐



### 缓存

> 缓存（cache）是一种介于数据永久存储介质与数据应用之间的数据临时存储介质
>
> 使用缓存可以**有效的减少低速数据读取的过程次数（例如磁盘IO），提高系统性能**
>
> 缓存不仅可以用于**提高永久性存储介质的数据读取效率**，还**可以提供临时的数据存储空间**
>
> 也就是，从APP <==> DB，变为了 **APP <==> Cache <==> DB**



SpringBoot提供了默认缓存技术：**Simple**

缓存使用：

* 启用缓存

  * **导入缓存的starter**

    ```xml
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-cache</artifactId>
    </dependency>
    ```

  * **在引导类上使用@EnableCaching注解，开启缓存功能**，或者在某个配置类上使用，然后加载入SpringBoot项目

* 设置进入缓存的数据

* 设置读取缓存的数据

  * 设置当前操作的结果数据进入缓存，**使用@Cacheable注解**，value属性是自定义缓存名，key是想拿缓存数据的"钥匙"索引

    例如：

    ```java
    @Override
    @Cacheable(value = "cacheSpace", key = "#id")//查询时会先走缓存
    public Books selectById(Integer id) {
       return bookDao.selectById(id);
    }
    ```
    
  * **@Cacheable注解**：是**向缓存中存值或者取值**，每次取的必然是之前已经向缓存中存的
  
  * **@CachePut注解**：格式与@Cacheable一致，功能是**只向缓存中存值**，无法取值

【注】特别注意，在一个类中，**@CachePut注解向缓存中存值**之后，需要我们**用一个独立的方法将缓存中的值取出来**，因为**@Cacheable注解的取值机制是将缓存内的值作为方法的返回值返回**。然后我们去**调用这个独立的方法得到缓存内的值**。更要**特别注意的是**：如果这个**取缓存值的独立方法**与**调用它的方法**在**同一个类中**，这**属于是类的内部调用，这个方法没有经过Spring容器的管理，没有注入缓存功能，无法实现取出缓存值**，哪怕这个类上有@Service之类的容器Bean注解也不行，因为这个方法始终是类的内部调用，没有被Spring容器管理。解决方法就是**将取缓存值的独立方法放到另一个Bean之中就可以了**，调用这个Bean的方法，就会经过Spring容器管理，然后给它注入缓存功能



SpringBoot除了提供默认的缓存技术，还可以对其他的缓存技术进行整合（**统一接口**，方便缓存技术的开发与管理，**更改了技术但是代码不变**）

常用如下：

* Simple（默认的）
* Ehcache
* **Redis**
* **memcached**（未整合，代码会变）



#### Ehcache

使用Ehcache技术：

1. 需要导入依赖

   ```xml
   <dependency>
       <groupId>net.sf.ehcache</groupId>
       <artifactId>ehcache</artifactId>
   </dependency>
   ```

2. 创建Ehcache的配置文件，配置好所需配置（ehcache.xml）

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
            xsi:noNamespaceSchemaLocation="http://ehcache.org/ehcache.xsd"
            updateCheck="false">
       <!--
          diskStore：为缓存路径，ehcache分为内存和磁盘两级，此属性定义磁盘的缓存位置。参数解释如下：
          user.home – 用户主目录
          user.dir – 用户当前工作目录
          java.io.tmpdir – 默认临时文件路径
        -->
       <diskStore path="D:\Ehcache"/>
   
       <defaultCache
               eternal="false"
               diskPersistent="false"
               maxElementsInMemory="1000"
               overflowToDisk="false"
               timeToIdleSeconds="60"
               timeToLiveSeconds="60"
               memoryStoreEvictionPolicy="LRU"/>
   
       <cache
               name="smsCode"    <!--程序中某处使用的缓存名，需要在这里设置-->
               eternal="false"
               diskPersistent="false"
               maxElementsInMemory="1000"
               overflowToDisk="false"
               timeToIdleSeconds="60"
               timeToLiveSeconds="60"
               memoryStoreEvictionPolicy="LRU"/>
       <!--
          defaultCache：默认缓存策略，当ehcache找不到定义的缓存时，则使用这个缓存策略。只能定义一个。
        -->
       <!--
         name:缓存名称。
         maxElementsInMemory:缓存最大数目
         maxElementsOnDisk：硬盘最大缓存个数。
         eternal:对象是否永久有效，设置为true则不会被清除，一但设置了，timeout将不起作用。通常设置为false
         overflowToDisk:超过最大缓存数时，是否保存到磁盘
         timeToIdleSeconds:设置对象在失效前的允许闲置时间（单位：秒）。仅当eternal=false对象不是永久有效时使用，可选属性，默认值是0，也就是最大不活动间隔。
         timeToLiveSeconds:设置对象在失效前允许存活时间（单位：秒）。最大时间介于创建时间和失效时间之间。仅当eternal=false对象不是永久有效时使用，默认是0.，也就是对象存活时间无穷大。
         diskPersistent：是否缓存虚拟机重启期数据 Whether the disk store persists between restarts of the Virtual Machine. The default value is false.
         diskSpoolBufferSizeMB：这个参数设置DiskStore（磁盘缓存）的缓存区大小。默认是30MB。每个Cache都应该有自己的一个缓冲区。
         diskExpiryThreadIntervalSeconds：磁盘失效线程运行时间间隔，默认是120秒。
         memoryStoreEvictionPolicy：缓存清除策略，当达到maxElementsInMemory限制时，Ehcache将会根据指定的策略去清理内存。默认策略是LRU（最近最少使用）。你可以设置为FIFO（先进先出）或是LFU（较少使用）。
         clearOnFlush：内存数量最大时是否清除。
         memoryStoreEvictionPolicy:可选策略有：LRU（最近最少使用，默认策略）、FIFO（先进先出）、LFU（最少访问次数）。
         FIFO，first in first out，这个是大家最熟的，先进先出。
         LFU， Less Frequently Used，就是上面例子中使用的策略，直白一点就是讲一直以来最少被使用的。如上面所讲，缓存的元素有一个hit属性，hit值最小的将会被清出缓存。
         LRU，Least Recently Used，最近最少使用的，缓存的元素有一个时间戳，当缓存容量满了，而又需要腾出地方来缓存新的元素的时候，那么现有缓存元素中时间戳离当前时间最远的元素将被清出缓存。
      -->
   </ehcache>
   ```

3. 更改配置，使用Ehcache

   ```yml
   spring:
     cache:
       type: ehcache
       ehcache:
         config: ehcache.xml #指定Ehcache的配置文件是哪个，一般是防止Ehcache的配置文件不叫ehcache.xml
   ```

【注】**Ehcache技术需要@Cacheable注解内设置的缓存空间名在Ehcache的配置文件中提前设置好**





#### [知识补充] 数据淘汰策略

LRU：Least Recently Used 最近最少使用的淘汰，也就是淘汰掉最近不活跃的数据

LFU：Least Frequently Used 最近使用次数最少的淘汰

TTL：将将要过期的淘汰掉

RANDOM：任意选择淘汰掉





#### Redis

使用Redis缓存技术：

1. 导入Redis依赖

2. 配置Redis

   ```yml
   spring:
     redis:
       host: localhost
       port: 6379                  #这里是先配置好Redis服务器，才可以使用Redis的缓存
     cache:
       type: redis                 #指定使用Redis缓存
       redis:                      #配置Redis缓存
         time-to-live: 10s         #最大存活时间
         key-prefix: wyh_          #指定key前缀
         use-key-prefix: false     #是否使用前缀 -- 不使用就只用value没有key
         cache-null-values: false  #是否缓存空值
   ```





#### memcached

[memcached下载](https://www.runoob.com/memcacahed/window-install-memcached.html)

mamcached使用：cmd窗口

* memcached.exe -d install：安装
* memcached.exe -d start：启动服务
* memcached.exe -d stop：停止服务



**SpringBoot并没有对memcached进行整合**，**需要使用硬编码**实现对客户端的初始化管理

memcached客户端选择：

* Memcached Client for Java：最早期客户端，稳定可靠，用户群广
* SpyMemcached：效率相较于上一个高
* **Xmemcached**：在上一个的基础上多了并发处理（这个最好）



memcached使用：

1. 导入Xmemcached坐标：

   ```xml
   <!-- https://mvnrepository.com/artifact/com.googlecode.xmemcached/xmemcached -->
   <dependency>
       <groupId>com.googlecode.xmemcached</groupId>
       <artifactId>xmemcached</artifactId>
       <version>2.4.7</version>
   </dependency>
   ```

2. 配置memcached服务器的一些属性：这是可以直接写在代码里的，但为了安全性和美观性，我们选择将这些配置加在配置文件中，为自定义属性

   ```yml
   #Xmemcached的一些属性配置
   memcached:
     servers: localhost:11211 #服务器地址
     poolSize: 10             #连接池数量
     opTimeout: 3000          #默认超时时间
   ```

3. 创建读取配置信息的属性类，加载刚刚配置好的属性：

   ```java
   @ConfigurationProperties(prefix = "memcached")
   @Data
   public class XmemcachedProperties {
       private String servers;
       private int poolSize;
       private long opTimeout;
   }
   ```

4. 创建memcached客户端配置类：因为SpringBoot没有整合，所以需要我们把Xmemcached加入到Spring容器里面

   ```java
   @Configuration
   @EnableConfigurationProperties({XmemcachedProperties.class})
   public class XmemcachedConfig {
       @Autowired
       XmemcachedProperties xmemcachedProperties;
   
       @Bean
       public MemcachedClient memcachedClient() throws IOException {
           MemcachedClientBuilder memcachedClientBuilder = new XMemcachedClientBuilder(xmemcachedProperties.getServers());
           memcachedClientBuilder.setConnectionPoolSize(xmemcachedProperties.getPoolSize());
           memcachedClientBuilder.setOpTimeout(xmemcachedProperties.getOpTimeout());
           return memcachedClientBuilder.build();
       }
   }
   ```

5. 使用memcached客户端类，使用set存入缓存，get取缓存值

   * set方法的三个参数：第一个参数为作为key的值，第二个参数为过时时间（0为永不过时），第三个参数为key对应的value
   * get方法的参数为key





#### jetcache

> 这是由阿里巴巴创建的一门缓存技术
>
> 是对SpringCache进行了封装，在原有的功能上，实现了：
>
> * 多级缓存
> * 缓存统计
> * 自动刷新
> * 异步调用
> * 数据报表



jetcache设定了**本地缓存和远程缓存**的多级缓存解决方案（本地缓存访问速度快，远程缓存解决容量问题）

jetcache支持的四种：

* 本地缓存：
  * **LinkedHashMap**
  * Caffeine
* 远程缓存
  * **Redis**
  * Tair



使用jetcache：

1. 导入jetcache坐标依赖：

   ```xml
   <dependency>
       <groupId>com.alicp.jetcache</groupId>
       <artifactId>jetcache-starter-redis</artifactId>
       <version>2.6.0</version>
   </dependency>
   ```

2. **配置远程缓存或者本地缓存**的必要配置属性：

   ```yml
   jetcache:
     statIntervalMinutes: 15      #设置缓存统计报告，十五分钟后查看
     areaInCacheName: false       #是否将@CreatCache注解的area属性的值加入到缓存名中
     local:			  #本地缓存
       default: 		  #默认配置 设置为default可以省去指定@CreatCache注解的area的属性
         type: linkedhashmap       #指定本地缓存类型为LinkedHashMap
         keyConvertor: fastjson    #key值的转换器 因为如果key为对象，需要转为String
         limit: 100      #最大缓存数量
     remote:             #远程缓存
       default:          #默认配置 设置为default可以省去指定@CreatCache注解的area的属性
         type: redis     #类型
         host: localhost
         port: 6379
         keyConvertor: fastjson
         valueEncoder: java       #value的编码类型
         valueDecoder: java	   #value的解码类型
         poolConfig:     #必须要有的配置
           maxTotal: 50  #最大连接数
           maxIdle: 20
           minIdle: 5
       sms:              #两种缓存除了设置default，还可以自定义配置名，那么需要指定@CreatCache注解的area的属性值为这个
         type: redis     #类型
         host: localhost
         port: 6379
         poolConfig:     #必须要有的配置
           maxTotal: 50
   ```

3. 开启jetcache缓存功能：在启动类上添加**@EnableCreateCacheAnnotation注解**，提供**注解开启缓存功能**

4. 声明缓存对象，并添加**@CreatCache注解**

   ```java
   @CreateCache(name = "jetcache_",expire = 3600,cacheType = CacheType.LOCAL)//必须填两个参数：自定义缓存名和过时时间，单位默认为秒。可以自己设置单位：用第三个参数timeUnit；cacheType为指定缓存类型为本地还是远程还是都是；area为指定配置属性名为哪个；
   private Cache<String,String> jetcache;
   ```

5. 操作缓存：使用缓存对象，调用put注入缓存，get取缓存值



方法缓存：直接在方法上开启缓存功能，可以通过方法注入或取出缓存值

1. 启用方法缓存：在启动类上使用**@EnableMethodCache注解**，**属性值为指定要开启方法缓存的包是哪个**，或者覆盖到这个包

   ```java
   @EnableCreateCacheAnnotation//开启jetcache缓存
   @EnableMethodCache(basePackages = {"com.wyh"})//开启方法注解
   ```

2. 使用方法注解操作缓存

   ```java
   @Cached(name = "book_",key = "#id",expire = 3600,cacheType = CacheType.REMOTE)//读取缓存，没有就注入
   @CacheRefresh(refresh = 10)//缓存刷新（十秒刷新一次，防止更新了数据但是缓存还没有更新
   public Books selectById(Integer id) {
       return bookDao.selectById(id);
   }
   
   @CacheInvalidate(name = "book_",key = "#id")//缓存删除
   public Boolean delete(Integer id) {
       return bookDao.deleteById(id) > 0;
   }
   
   @CacheUpdate(name = "book_",key = "#books.id",value = "#books")//缓存更新
   public Boolean update(Books books) {
       return bookDao.updateById(books) > 0;
   }
   ```

3. **操作缓存的前提是缓存对象是可序列化的**，所以我们需要在缓存对象上**实现Serializable接口**

   ```java
   @Data
   public class Books implements Serializable {
       private Integer id;
       private String name;
       private String type;
       private String description;
   }
   ```

   同时在jetcache配置上我们还需要设定key转换器和value转换类型（在远程缓存时，redis只能操作字符串）

   ```yml
   keyConvertor: fastjson   #key值的转换器 因为如果key为对象，需要转为String
   valueEncoder: java       #value的编码类型
   valueDecoder: java	     #value的解码类型
   ```





#### j2cache

> 是一个缓存整合框架，自身不提供缓存功能
>
> 提供的是缓存的整合方案，使各种缓存搭配使用



整合ehcache + redis：

1. 加入j2cache和Ehcache的坐标依赖：

   ```xml
   <dependency>
       <groupId>net.oschina.j2cache</groupId>
       <artifactId>j2cache-core</artifactId>
       <version>2.8.4-release</version>
   </dependency>
   
   <!--内涵redis的包 所以不需要再导redis的依赖-->
   <dependency>
       <groupId>net.oschina.j2cache</groupId>
       <artifactId>j2cache-spring-boot2-starter</artifactId>
       <version>2.8.0-release</version>
   </dependency>
   
   <dependency>
       <groupId>net.sf.ehcache</groupId>
       <artifactId>ehcache</artifactId>
   </dependency>
   ```

2. 在application.yml中配置使用j2cache，指定j2cache的配置文件（文件名可以更改）

   ```yml
   j2cache:
     config-location: j2cache.properties
   ```

3. 在j2cache配置文件中配置一级缓存与二级缓存以及一级缓存数据到二级缓存的发送方式

   ```properties
   #j2cache最简单基础版
   
   #一级缓存
   j2cache.L1.provider_class = ehcache
   ehcache.configXml = ehcache.xml
   
   #设置是否启用二级缓存
   j2cache.l2-cache-open = true
   
   #二级缓存
   j2cache.L2.provider_class = net.oschina.j2cache.cache.support.redis.SpringRedisProvider
   #这里叫redis后面的属性才可以叫redis，这里是自定义的
   j2cache.L2.config_section = redis
   redis.hosts = localhost:6379
   
   #一级缓存的数据到达二级缓存  -- 广播模式：消息的发布与订阅
   j2cache.broadcast = net.oschina.j2cache.cache.support.redis.SpringRedisPubSubPolicy
   
   #设置redis的单级服务器
   redis.mode = single
   
   #缓存的前缀名
   redis.namespace = j2cache
   ```

4. 设置使用缓存对象：**CacheChannel**（由Spring注入）

5. 调用缓存操作：set设置缓存/get获取缓存值

   例如：cacheChannel.set("sms",tele,code);/cacheChannel.get("sms",smsCode.getTele());





### 任务

> 定时任务是企业级应用的常见操作
>
> 比如：
>
> * 年度报表
> * 缓存统计报告



流行的定时任务技术：

* Quartz
* **Spring Task**



#### Quartz

SpringBoot整合Quartz：

相关概念：

* 工作（Job）：用于定义具体执行的工作
* 工作明细（JobDetail）：用于描述定时工作相关的信息
* 触发器（Trigger）：用于描述触发工作的规则，通常使用cron表达式定义调度规则
* 调度器（Scheduler）：描述了工作明细与触发器的对应关系



1. 导入Quartz依赖坐标

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-quartz</artifactId>
   </dependency>
   ```

2. 定义具体要执行的任务（Job），无需设置为bean，只需**要继承QuartzJobBean**

   例如：

   ```java
   public class MyQuartz extends QuartzJobBean {
       @Override
       protected void executeInternal(JobExecutionContext jobExecutionContext) throws JobExecutionException { //这个方法类似于TimerTask的run方法 相当于一个多线程
           System.out.println("quartz is run");
       }
   }
   ```

3. 定义工作明细（JobDetail）和触发器（Trigger），并绑定对应关系

   例如：

   ```java
   @Configuration
   public class QuartzConfig {
       @Bean
       public JobDetail jobDetail() {
           return JobBuilder
               .newJob(MyQuartz.class)//绑定具体工作Job是什么，指定类
               .storeDurably()//是否持久化，必须要有，不然无法一直执行
               .build();
       }
   
       @Bean
       public Trigger jobTrigger() {
           ScheduleBuilder<? extends Trigger> scheduleBuilder = CronScheduleBuilder
               .cronSchedule("0/1 * * * * ?");//cron表达式，这里是：不限星期 任意月 任意日 任意时 任意分 0秒开始每1秒进行（倒序的，?为不限）
           return TriggerBuilder.newTrigger()
               .forJob(jobDetail())//绑定工作明细
               .withSchedule(scheduleBuilder)//指定时间周期 （调度器）
               .build();
       }
   }
   ```




#### SpringTask

使用**Spring Task**：

1. 开启定时任务功能：在启动类上加上**@EnableScheduling注解**

2. 设置要定时执行的具体任务方法，并制定周期：

   例如：

   ```java
   @Component
   public class Task {
       @Scheduled(cron = "0/1 * * * * ?")//指定具体周期 为cron表达式
       public void task() { //要定时执行的具体任务 这个任务方法要在Bean里面，归Spring管理
           System.out.println("SpringTask is run");
       }
   }
   ```

3. 还可以配置定时任务的相关配置，可以配也可以不配

   ```yml
   spring:
     task:
     	scheduling:
     		pool:
     		  size: 1  						     #任务调度线程池大小 默认为1
           thread-name-prefix: wyh_task_        #调度线程名称的前缀 默认为scheduling_
           shutdown:
           	await-termination: false		 #线程池关闭时是否等待所有任务完成
           	await-termination-period: 10s    #调度线程关闭前最大等待时间，确保最后一定关闭
   ```

   



### 邮件

> 相关概念：
>
> * SMTP：Simple Mail Transfer Protocol 简单邮件传输协议，用于发送电子邮件的传输协议
> * POP3：Post Office Protocol - Version 3 用于接收电子邮件的标准协议
> * IMAP：Internet Mail Access Protocol 互联网消息协议，是POP3的替代协议
>



#### JavaMail

SpringBoot整合JavaMail：

1. 导入SpringBoot整合Javamail的依赖坐标：

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-mail</artifactId>
   </dependency>
   ```

2. 配置javamail：

   ```yml
   spring:
     mail:
       host: smtp.qq.com            #主机地址 这里使用的是QQ邮箱
       username: xxxxxxxxxx@qq.com  #发件人用户账号
       password: xxxxxxxxxx         #在邮箱中开启IMAP/SMTP和POP3/SMTP服务给的码
   ```

3. 设置**简单邮件**对象**SimpleMailMessage**的发送人、收件人、主题、正文内容，然后用**JavaMailSender发送**

   例如：

   ```java
   @Service
   public class MailServiceImpl implements MailService {
       private final String Sender = "xxxxxxx@qq.com";//发件人
       private final String Recipient = "xxxxxxxxx@qq.com";//收件人
       private String subject = "测试邮件";//主题
       private String context = "测试邮件内容";//正文
   
       @Autowired
       private JavaMailSender javaMailSender;
   
       @Override
       public void sendMail() {
           SimpleMailMessage mailMessage = new SimpleMailMessage();
           mailMessage.setFrom(Sender + "你爹");//替换掉发送人内的邮箱地址，显示设置的字符串
           mailMessage.setTo(Recipient);
           mailMessage.setSubject(subject);
           mailMessage.setText(context);
           javaMailSender.send(mailMessage);
       }
   }
   ```



发送多部件邮件：前面步骤与发送简单邮件一致，只是第三步不一样，变为了**设置MimeMessage**对象

例如：

```java
@Service
public class MailServiceImpl implements MailService {
    private final String Sender = "1111111@qq.com";//发件人
    private final String Recipient = "2222222@qq.com";//收件人
    private String subject = "测试邮件";//主题
    private String context = "<a href="www.4399.com">点击玩玩刺激的东西</a>;//正文

    @Autowired
    private JavaMailSender javaMailSender;

    @Override
    public void sendMail() {
        MimeMessage mailMessage = javaMailSender,creatMimeMessage();
        
        MimeMessageHelper helper =  new MimeMessageHelper(mailMessage,true);//用这个MimeMessageHelper设置发送人、收件人、主题、正文内容;                                                                               true为开启多部件发送的功能，这样才可以发送附件
        helper.setFrom(Sender + "Your Father");
        helper.setTo(Recipient);
        helper.setSubject(subject);
        helper.setText(context,true);//true为开启了HTML解析 那么可以在正文中写入HTML格式的内容，比如超链接或者图片
        
        File f1 = new File("xxxx");
        File f2 = new File("xxxx");
        helper.addAttachment(f1.getName,f1);//添加邮件的附件
        helper.addAttachment("附件名称",f2);
        
        javaMailSender.send(mailMessage);
    }
}
```





### 消息

> 消息发送方：生产者
>
> 消息接收方：消费者
>
> 消息分类：
>
> * 同步消息：发出消息之后必须要有回应，有了回应才会接着有后续动作
> * 异步消息：只管发出消息，不需要有回应，发完消息就可以有后续动作（常用）
>
> MQ（Message Queue）：消息队列，是服务器将接收到的各式消息集中存储的地方，再由后续的独特功能子服务器在这消息队列中去搜寻所需消息



企业级应用中广泛使用的三种**异步消息传递技术**：

* JMS：（Java Message Service）一个**规范**，等同于JDBC规范，**提供了与消息服务相关的API接口**
  * JMS消息模型：
    * peer-2-peer：点对点模型，消息发送到一个队列中，队列保存消息，且**队列的消息只能被一个消费者消费**或超时
    * **publish-subscribe**：（pub-sub）**发布订阅模型**，**消息可以被多个消费者消费**，生产者和消费者完全独立，不需要感知对方的存在
  * JMS消息种类：
    * TextMessage
    * MapMessage
    * **BytesMessage**
    * StreamMessage
    * ObjectMessage
    * Message（只有消息头和属性）
  * JMS实现：**ActiveMQ**，**Redis**，HornetMQ，**RabbitMQ**，**RocketMQ**（没有完全遵循JMS规范）



* AMQP：（Advanced Message Queueing Protocol）高级消息队列协议，是一种**协议**，消息代理规范，**规范了网络交换的数据格式**，**兼容JMS**
  * AMQP优点：跨平台，服务器供应商、生产者、消费者可以各自使用不同的编程语言实现
  * AMQP消息模型：
    * **direct exchange**
    * fanout exchange
    * **topic exchange**
    * headers exchange
    * system exchange
  * AMQP消息种类：**byte[]（字节数组）**
  * AMQP实现：**RabbitMQ**，StormMQ，**RocketMQ**



* MQTT：（Message Queueing Telemetry Transport）消息队列遥测传输，转为小设备设计，是物联网（IOT）生态系统中的主要成分之一



想要理解消息，那我们需要模拟一个业务：**购物订单业务**

* 登录状态检测
* 生成主单
* 生成子单
* 库存检测与变更
* 积分变更
* 支付
* **短信通知（异步，我们模拟这一环节）**
* 购物车维护
* 运单信息初始化
* 商品库存维护
* 会员维护
* 等等

消息案例：**订单短信通知**

* OrderService和OrderController，处理订单，内部对消息进行处理，调用MessageService
* MessageService里用LinkedList模拟消息队列处理消息（之后整合各项MQ技术，就是更换这里的实现）





#### ActiveMQ

[ActiveMQ下载](https://activemq.apache.org/components/classic/download)

ActiveMQ使用：

* 启动服务：安装目录下的bin里的win64（根据自己的操作系统来）**双击运行 activemq.bat**
* 访问服务器：`http://127.0.0.1:8161/`
  * 服务端口61616，管理后台端口8161
  * 默认用户名和密码：admin



SpringBoot整合ActiveMQ：

1. 导入坐标依赖

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-activemq</artifactId>
   </dependency>
   ```

2. 配置ActiveMQ的配置

   ```yml
   #ActiveMQ配置
   spring:
     activemq:
       broker-url: tcp://localhost:61616 #连接的服务器地址 - tcp通信
     jms:                                #遵循的JMS规范，所以设置在JMS中
       template:
         default-destination: wyh        #设置默认的保存位置 名称自定义
       pub-sub-domain: true              #是否开启发布与订阅模型
   ```

3. 生产消息与消费消息

   例如：

   ```java
   @Service
   public class MessageServiceActiveMQImpl implements MessageService {
       private JmsMessagingTemplate jmsMessagingTemplate;//遵循的JMS规范，所以是JmsTemplate
       
       @Autowired
       public MessageServiceActiveMQImpl(JmsMessagingTemplate jmsMessagingTemplate) {
           this.jmsMessagingTemplate = jmsMessagingTemplate;
       }
       
       @Override
       public void SendMessage(String id) {
           System.out.println("消息已纳入消息队列 id: " + id);
           jmsMessagingTemplate.convertAndSend("order.queue.id",id);//转换并存储消息到指定的队列中（没有这个消息队列则创建） - 生产消息
       }
   
       @Override
       public String doMessage() {
           //从指定的队列获取消息，并指定转换消息的类型
           String id = jmsMessagingTemplate.receiveAndConvert("order.queue.id",String.class);
           System.out.println("已完成短信发送业务 id: " + id);//这是相当于手动消费消息
           return id;
       }
   }
   ```

   监听器模拟自动消费消息：

   ```java
   @Component
   public class MessageListener {
   	//监听器在于不用手动的消费消息
       @JmsListener(destination = "order.queue.id")//只要这个地方有消息，他就会立马自动消费掉
       @SendTo("order.other.queue.id")//消息被消费完，将此方法的返回值放入到另外一个消息队列中作为消息，交给他们处理 -> 流程式处理
       public String receive(String id) {
           System.out.println("已完成短信发送业务 id: " + id);
           return id;
       }
   }
   ```

   



#### RabbitMQ

RabbitMQ基于**Erlang语言**编写，需要有Erlang的环境

* [Erlang下载](https://www.erlang.org/downloads)
* 环境变量配置：
  * ERLANG_HOME
  * PATH
* Erlang安装完需要重启下电脑



[RabbitMQ下载](https://rabbitmq.com/install-windows.html)

RabbitMQ使用：

在RabbitMQ安装目录下的sbin目录中，启用cmd命令行输入命令

* 启动服务：`rabbitmq-service.bat start`
* 关闭服务：`rabbitmq-service.bat stop`
* 查看服务状态：`rabbitmqctl status`
* 服务管理可视化（插件形式）
  * 查看已安装的插件列表：`rabbitmq-plugins.bat list`
  * 开启服务管理插件：`rabbitmq-plugins.bat enable rabbitmq_management`
  * 访问服务器：`http://localhost:15672`
    * 服务端口：**5672**（我们使用的是这个），管理后台端口：15672（浏览器访问端口）
    * 用户名和密码：默认guest



SpringBoot整合RabbitMQ：

* Direct模式：（直连）

  1. 导入RabbitMQ的坐标依赖

     ```xml
     <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-amqp</artifactId><!--遵循的是AMQP协议，所以为amqp-->
     </dependency>
     ```

  2. 配置RabbitMQ

     ```yml
     spring:
       #RabbitMQ配置
       rabbitmq:
         host: localhost
         port: 5672
     ```

  3. 在RabbitMQ的Direct配置类中设置Direct模式下的消息队列专用的消息队列对象、交换机、绑定关系：

     ```java
     @Configuration
     public class RabbitMQDirectConfig {
         //定义存储消息的消息队列对象
         @Bean
         public Queue directQueue() {
             /*
             durable：是否持久化 默认true
             exclusive：是否当前连接专用 连接关闭后队列即被删除 默认为false
             autoDelete：是否自动删除 当生产者或者消费者不再使用此队列时自动删除 默认为false
             */
             return new Queue("directQueue");//后面三个默认boolean值，为是否持久化、是否当前连接专用，是否删除
         }
     
         //定义交换机对象 - 一个交换机对象可以绑定多个消息队列对象 - 可复用
         @Bean
         public DirectExchange directExchange() {
             return new DirectExchange("directExchange");
         }
     
         //定义绑定关系
         @Bean
         public Binding directBinding() {
             //绑定消息队列对象到交换机上，取个名字为"direct"
             return BindingBuilder.bind(directQueue()).to(directExchange()).with("direct");
         }
     }
     ```

  4. 模拟生产者生产消息

     ```java
     @Service
     public class MessageServiceRabbitMQDirectImpl implements MessageService {
         private final AmqpTemplate amqpTemplate;
     
         @Autowired
         public MessageServiceRabbitMQDirectImpl(AmqpTemplate amqpTemplate) {
             this.amqpTemplate = amqpTemplate;
         }
     
         @Override
         public void SendMessage(String id) {
             System.out.println("消息已纳入消息队列 id(RabbitMQ - Direct): " + id);
             //指定交换机、绑定关系、消息 - 为生产消息
             amqpTemplate.convertAndSend("directExchange","direct",id);
         }
     }
     ```

  5. 监听器模拟消费消息（当有**多个监听器监听到同一个消息队列时，这些监听器会轮询处理统一消息队列的消息**）

     ```java
     @Component
     public class MessageListener {
         @RabbitListener(queues = {"directQueue"})//指定监听的是哪些消息队列 - 字符串数组
         public void receive(String id) {
             System.out.println("已完成短信发送业务 id(RabbitMQ - Direct): " + id);//模拟消费消息
         }
     }
     ```




* Topic模式：（话题）

  1. 导入的依赖和配置和Direct模式一样

  2. 在RabbitMQ的Topic配置类中设置Topic模式下的消息队列专用的消息队列对象、交换机、绑定关系

     ```java
     @Configuration
     public class RabbitMQTopicConfig {
         //定义存储消息的消息队列对象
         @Bean
         public Queue topicQueue1() {
             return new Queue("topicQueue1");//后面三个默认boolean值，为是否持久化、是否当前连接专用，是否删除
         }
     
         @Bean
         public Queue topicQueue2() {
             return new Queue("topicQueue2");//后面三个默认boolean值，为是否持久化、是否当前连接专用，是否删除
         }
     
         //定义交换机对象
         @Bean
         public TopicExchange topicExchange() {
             return new TopicExchange("topicExchange");
         }
     
         //定义绑定关系
         @Bean
         public Binding topicBinding1() {
             //绑定消息队列对象到交换机上，取个名字为"topic.*.id" - 这是设置了一种绑定规则，满足条件的消息就会放入这个绑定关系中
             return BindingBuilder.bind(topicQueue1()).to(topicExchange()).with("topic.*.id");
         }
     
         @Bean
         public Binding topicBinding2() {
             //绑定消息队列对象到交换机上，取个名字为"topic.orders.*" - 设置了一种绑定规则，满足条件的消息就会放入这个绑定关系中
             return BindingBuilder.bind(topicQueue2()).to(topicExchange()).with("topic.orders.*");
         }
     }
     ```

     [知识补充] Topic模式下的**绑定规则匹配**：

     * *（星号）：**表示一个单词**，必须有一个单词
     * #（井号）：表示**任意数量的单词**

     |       匹配键       | topic.\*.\* | topic.# |
     | :----------------: | :---------: | :-----: |
     |   topic.order.id   |    true     |  true   |
     |   order.topic.id   |    false    |  false  |
     | topic.ssm.order.id |    false    |  true   |
     |    topic.ssm.id    |    true     |  true   |
     |   topic.id.order   |    true     |  true   |
     |      topic.id      |    false    |  true   |
     |    topic.order     |    false    |  true   |

  3. 模拟生产者生产消息

     ```java
     @Service
     public class MessageServiceRabbitMQTopicImpl implements MessageService {
         private final AmqpTemplate amqpTemplate;
     
         @Autowired
         public MessageServiceRabbitMQTopicImpl(AmqpTemplate amqpTemplate) {
             this.amqpTemplate = amqpTemplate;
         }
     
         @Override
         public void SendMessage(String id) {
             System.out.println("消息已纳入消息队列 id(RabbitMQ - Topic): " + id);
             //指定交换机、绑定关系、消息 - 为生产消息
             amqpTemplate.convertAndSend("topicExchange","topic.orders.id",id);//这里写的"topic.orders.id"是为了匹配上定义好的两种绑定匹配规则
         }
     
         @Override
         public String doMessage() {
             return null;
         }
     }
     ```

  4. 监听器模拟消费消息（当有**多个监听器监听到同一个消息队列时，这些监听器会轮询处理统一消息队列的消息**）

     ```java
     @Component
     public class MessageListener {
         @RabbitListener(queues = {"topicQueue1"})//指定监听的是哪个消息队列
         public void receive1(String id) {
             System.out.println("已完成短信发送业务 id(RabbitMQ - Topic1): " + id);//模拟消费消息
         }
     
         @RabbitListener(queues = {"topicQueue2"})//指定监听的是哪个消息队列
         public void receive2(String id) {
             System.out.println("已完成短信发送业务 id(RabbitMQ - Topic2): " + id);//模拟消费消息
         }
     }
     ```



【注】Topic模式比Direct模式更强大，可以自定义消息匹配规则





#### RocketMQ



[RocketMQ下载](https://rocketmq.apache.org/)

* 环境变量配置：

  * ROCKETMQ_HOME

  * Path

  * NAMESRV_ADDR：127.0.0.1:9876

    （也就是配置了一个**命名服务器** **NameServer**，生产者和消费者访问命名服务器，由命名服务器转接**子业务服务器** **Broker**）

    （方便我们启动broker的时候不需要再打开cmd界面设置这个值才能启动）

* 默认服务端口：9876



RocketMQ使用：

* 先启动命名服务器：安装目录下的bin目录下，双击 `mqnamesrv.cmd`启动
* 再启动业务服务器：安装目录下的bin目录下，双击 `mqbroker.cmd`启动



SpringBoot整合RocketMQ：

1. 导入RocketMQ坐标依赖

   ```xml
   <!-- https://mvnrepository.com/artifact/org.apache.rocketmq/rocketmq-spring-boot-starter -->
   <!-- RocketMQ -->
   <dependency>
       <groupId>org.apache.rocketmq</groupId>
       <artifactId>rocketmq-spring-boot-starter</artifactId>
       <version>2.2.2</version>
   </dependency>
   ```

2. 配置RocketMQ - 默认配置

   ```yml
   #RocketMQ配置
   rocketmq:
     name-server: localhost:9876         #连接命名服务器
     producer:
       group: rocketMQ_group             #对生产者分组，使其初始化成功 - 组名无所谓，这是必要的配置，不然会启动报错
   ```

3. 模拟生产者生产消息

   ```java
   @Service
   public class MessageServiceRocketMQImpl implements MessageService {
       private final RocketMQTemplate rocketMQTemplate;
   
       @Autowired
       public MessageServiceRocketMQImpl(RocketMQTemplate rocketMQTemplate) {
           this.rocketMQTemplate = rocketMQTemplate;
       }
   
       @Override
       public void SendMessage(String id) {
           System.out.println("消息已纳入消息队列 id(RocketMQ): " + id);
           //rocketMQTemplate.convertAndSend("order_id",id);//这是一种同步消息，不常用
           rocketMQTemplate.asyncSend("order_id", id, new SendCallback() {//异步消息，多了一个回调方法参数，常用
               @Override
               public void onSuccess(SendResult sendResult) {//消息成功发送到消息队列时的行为 - 也就是发完消息后的回调操作
                   System.out.println("消息发送成功");
               }
   
               @Override
               public void onException(Throwable throwable) {//消息发送到消息队列失败时的行为
                   System.out.println("消息发送失败");
               }
           });
       }
   }
   ```

4. 监听器模拟消费消息

   ```java
   @Component
   /*
   	指定监听的消息队列名
   	和
   	指定消息来自哪个生产者组名（先前在配置文件中配置的生产者组名），让生产者和消费者在同一个组
   */
   @RocketMQMessageListener(topic = "order_id", consumerGroup = "rocketMQ_group")
   public class MessageListener implements RocketMQListener<String> {//实现RocketMQ自带的监听器接口，泛型为消息类型
       @Override
       public void onMessage(String s) {
           System.out.println("已完成短信发送业务 id(RocketMQ): " + s);
       }
   }
   ```

   



#### kafka

> 一种高吞吐量的分布式发布订阅消息系统，提供实时消息功能
>
> 它的主要功能不是用于消息队列的，但是依然可以使用



[kafka下载](https://kafka.apache.org/downloads)

* Windows系统下3.0.0版本存在BUG，建议使用2.X版本



kafka使用：安装目录下的bin目录中的windows目录下，开启cmd命令行

* 先启动zookeeper（相当于一个命名服务器）：`zookeeper-server-start.bat ../../config/zookeeper.properties`
  * 默认端口：2181
* 再启动kafka（这个才是用的服务器）：`kafka-server-start.bat ../../config/server.properties`
  * 默认端口：9092

* 测试：

  * 先拥有topic：（**这里的topic可以理解为一个个消息队列对象**）

  * 创建topic：

    ```asciiarmor
    kafka-topics.bat --create --zookeeper localhost:2181 --replication-factor 1 --partitions 1 --topic wyh
    ```

    * 查看topic：

      ```asciiarmor
      kafka-topics.bat --zookeeper 127.0.0.1:2181 --list
      ```

    * 删除topic：

      ```asciiarmor
      kafka-topics.bat --delete --zookeeper localhost:2181 --topic wyh
      ```

    

  * 生产者功能测试：

    ```asciiarmor
    kafka-console-producer.bat --broker-list localhost:9092 --topic wyh
    ```

  * 消费者功能测试：

    ```asciiarmor
    kafka-console-consumer.bat --bootstrap-server localhost:9092 --topic wyh --from-beginning
    ```



SpringBoot整合kafka：

1. 导入kafka坐标依赖

   ```xml
   <dependency>
       <groupId>org.springframework.kafka</groupId>
       <artifactId>spring-kafka</artifactId>
   </dependency>
   ```

2. 配置kafka

   ```yml
   spring:
     #kafka配置
     kafka:
       bootstrap-servers: localhost:9092 #指定访问的服务器地址
       consumer:
         group-id: order                 #在开启监听功能时的必须配置 配置给consumer 不然会报错 名称随意
   ```

3. 模拟生产者生产消息

   ```java
   @Service
   public class MessageServerKafkaImpl implements MessageService {
       private final KafkaTemplate<String,String> kafkaTemplate;
   
       @Autowired
       public MessageServerKafkaImpl(KafkaTemplate<String, String> kafkaTemplate) {
           this.kafkaTemplate = kafkaTemplate;
       }
   
       @Override
       public void SendMessage(String id) {
           kafkaTemplate.send("wyh",id);//将消息传入到kafka的topic中，第一个参数即为指定已存在的topic
           System.out.println("消息已纳入消息队列(Kafka) id: " + id);
       }
   }
   ```

4. 监听器模拟消费者消费消息

   ```java
   @Component
   public class MessageListener {
       @KafkaListener(topics = {"wyh"})//指定监听的是哪个topic
       public void doMessage(ConsumerRecord<String, String> record) {//kafka使用的接收消息的类型 - ConsumerRecord
           System.out.println("已完成短信发送业务 id(Kafka): " + record.value());//用value方法取出消息队列里的消息
       }
   }
   ```

   



## 6. 监控🔍

> 监控的意义：
>
> * 监控服务状态是否宕机
> * 监控服务运行指标（内存，虚拟机，线程，请求等）
> * 监控日志
> * 管理服务（强制服务下线）
>
> 总之，就是盯着服务器的状态，根据状态做出相应的调整



### 监控的实施方式



**显示监控服务信息的服务器**：用于**主动获取服务信息**，并显示对应的信息

**各个运行的服务**：**启动时要主动上报，告知监控服务器自己需要受到监控**





### 可视化监控平台

> 显示监控服务信息的服务器如果要自己实现太麻烦了，所以有人已经帮我们实现了



#### Spring Boot Admin

> 开源社区项目 - 不是SpringBoot官方的
>
> 用于管理和监控SpringBoot应用程序
>
> 客户端注册到服务端后，通过HTTP请求方式，服务端定期从客户端获取对应的信息，并通过浏览器中的UI界面展示对应信息



Spring Boot Admin使用：

* 配置SpringBootAdmin的**服务端**：

  1. 导入Admin的服务端依赖坐标

     ```xml
     <dependency>
         <groupId>de.codecentric</groupId>
         <artifactId>spring-boot-admin-starter-server</artifactId>
         <version>2.5.4</version> <!--admin的版本由自己控制，SpringBoot没有管控，版本要与SpringBoot一致-->
     </dependency>
     ```

  2. 将服务端设置为web项目，并配置：（这样我们才能在浏览器里通过UI监控）

     ```xml
     <dependency>
         <groupId>org.springframework.boot</groupId>
         <artifactId>spring-boot-starter-web</artifactId>
     </dependency>
     ```

     ```yml
     server:
       port: 8080
     ```

  3. 在服务端的启动类上添加**@EnableAdminServer注解**，**开启SpringBAdmin的功能**，然后启动，前往浏览器输入地址即可查看到监控的UI界面



* 配置SpringBootAdmin的**客户端**：

  1. 导入Admin的客户端依赖坐标

     ```xml
     <dependency>
         <groupId>de.codecentric</groupId>
         <artifactId>spring-boot-admin-starter-client</artifactId>
         <version>2.5.4</version> <!--admin的版本由自己控制，SpringBoot没有管控，版本要与SpringBoot一致-->
     </dependency>
     ```

  2. 配置客户端

     ```yml
     spring:
       boot:
         admin:
           client:
             url: http://localhost:8080   #设置监控的信息交到哪里  - 也就是我们配置好的Admin服务端地址
     server:
       port: 80                           #客户端最好也是一个web项目，不然无法持久化的监控
     
     management:
       endpoint:                          #各个端点配置
         health:							 #health端点
           show-details: always           #设置健康指标都可以看 默认为never-不可以
       endpoints:
         web:                             #开启web请求下的某项监控信息是否显示 - 默认为health端点可看
           exposure:
             include: "*"                 #开启监控所有的endpoint 默认为health的endpoint - *为所有endpoint
         enabled-by-default: true         #开启所有端点的监控信息递送
     ```

  

然后我们就**通过浏览器的服务端UI界面监控各个客户端服务**





### 监控原理



* **Actuator**提供了SpringBoot生产就绪的功能，通过端点的配置与访问，获取端点的信息
* SpringBootAdmin就是加入了Actuator的坐标依赖
* 端点：描述了一组监控信息，SpringBoot提供了多个内置端点，也可以根据需要自定义端点信息



* 访问当前应用的所有端点的信息：/actuator
* 访问某端点的详细信息：/actuator/端点名称
  * 一些常用端点：
    * health：显示应用程序的健康信息 -- 健康
    * loggers：显示和修改应用程序中日志记录器的配置 -- 日志
    * metrics：显示当前应用程序的指标度量信息 -- 性能



端点的配置：

```yml
management:
  endpoint:                          #各个端点配置
    health:							 #health端点 - 具体某个端点
      enabled: true                  #是否启用该端点 - 单一端点
      show-details: always           #设置健康指标都可以看 默认为never-不可以
  endpoints:
    enabled-by-default: true         #开启所有端点的监控信息递送 - 所有端点
```



[知识补充] JMX访问方式：

* SpringBootAdmin为web的访问形式
* 我们可以在cmd界面上输入：jconsole，即可**使用Java自带的监控管理平台**





### 自定义监控指标



我们可以为各端点添加自定义指标



info端点：

* 方式一：可以在项目配置文件中添加info端点配置，可以直接添加自定义信息

  ```yml
  info:
    author: "王钇欢"
  ```

* 方式二：创建一个info配置类，以编程的手段添加自定义信息

  例如：

  ```java
  @Component//要受Spring管理
  public class InfoConfig implements InfoContributor {//需要实现SpringBootAdmin提供的接口 - InfoContributor
      @Override
      public void contribute(Info.Builder builder) {//实现这个方法，并用builder直接添加自定义消息
          Date time = new Date(System.currentTimeMillis());
          SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm");
          String timeStr = simpleDateFormat.format(time);
          builder.withDetail("runTime",timeStr);//withDetail方法为直接添加键值对，为一条信息
  
          Map<String,Object> infoMap = new HashMap<>();
          infoMap.put("buildTime","2022/11/25");
          builder.withDetails(infoMap);//withDetails方法为添加Map集合，为多条信息
      }
  }
  ```

  

health端点：

代表一个应用的状态信息，**一般是很重要的**才会加进去

我们自定义的health指标会加载health端点里面显示：

例如：

```java
@Component
public class HealthConfig extends AbstractHealthIndicator {//为health端点加指标，只需要继承AbstractHealthIndicator
    @Override
    protected void doHealthCheck(Health.Builder builder) throws Exception {
        boolean condition = true;
        if (condition) {
            builder.status(Status.UP);//标记这个的状态为UP
            Date time = new Date(System.currentTimeMillis());
            SimpleDateFormat simpleDateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm");
            String timeStr = simpleDateFormat.format(time);
            builder.withDetail("runTime",timeStr);

            Map<String,Object> infoMap = new HashMap<>();
            infoMap.put("buildTime","2022/11/25");
            builder.withDetails(infoMap);
        } else {
            builder.status(Status.OUT_OF_SERVICE);//标记这个的状态为OUT_OF_SERVICE
            builder.withDetail("服务状态","Exception");
        }
    }
}
```



metrics端点：（代表性能端点）

为Metrics端点添加自定义指标：比如用户付费次数指标（也就是一个统计操作）

例如：

```java
@Service
public class BookServiceImpl implements BookService {
    private Counter counter;
    public BookServiceImpl(MeterRegistry meterRegistry) {//构造器注入 MeterRegistry对象 用于为metrics端点添加指标
        counter = meterRegistry.counter("用户付费操作次数: ");//选择统计指标，并为指标取名
    }
    
    @Override
    public boolean pay() {
        counter.increment();//只要调用了这个业务一次，就会计数一次
        return true;
    }
}
```



**自定义端点**：

除了info，health等端点，我们还可以自己定义一个端点：比如支付端点

```java
@Component
@Endpoint(id = "pay",enableByDefault = true)//定义端点名称 和 是否默认被读取（默认为true）
public class PayEndpoint {
    //一个端点被定义了，还需要有相应的操作方法才可以被监控
    @ReadOperation//对端点的读操作
    public Object pay() {
        //这里应该调用具体的业务操作，获取相应的端点应该具有的信息
        Map<String,String> payMap = new HashMap<>();
        payMap.put("Level 1","100");
        payMap.put("Level 2","200");
        payMap.put("Level 3","300");

        return payMap;//会自动的转为JSON格式数据返回
    }
}
```







# [原理篇]



# 自动配置⚙



## bean的加载方式



一：xml+\<bean/>

**XML加载方式**

在Spring的配置文件中（应用上下文）定义

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--XML方式声明自己开发的bean-->
    <bean id="cat" class="com.wyh.bean.Cat"/>
    <bean class="com.wyh.bean.Dog" scope="singleton"/>

    <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource"/>
</beans>
```





二：xml:context+注解

**XML+注解声明方式**

1. 要想使用注解并被Spring容器扫描到，需要开启一个新的xmlnamespace：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <beans xmlns="http://www.springframework.org/schema/beans"
          xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
          
          xmlns:context="http://www.springframework.org/schema/context"
          
          xsi:schemaLocation="
          http://www.springframework.org/schema/beans
          http://www.springframework.org/schema/beans/spring-beans.xsd
                              
          http://www.springframework.org/schema/context
          http://www.springframework.org/schema/context/spring-context.xsd
   ">
       <!--开启一个全新的xmlnamespace（命名空间）：context 格式：xmlns:context="http://www.springframework.org/schema/context"
         同时xsi:schemaLocation中加入命名空间对应的地址 格式：http://www.springframework.org/schema/context
                                                        http://www.springframework.org/schema/beans/spring-context.xsd
   -->
   
   
       <!--指定加载bean（Component）的位置-->
       <context:component-scan base-package="com.wyh.bean,com.wyh.config"/>
   
   
   </beans>
   ```

2. 然后我们就可以将**自己开发的bean**加上**@Component**及其衍生注解**@Controller**，**@Service**，**@Repository**，将其定义为bean，后加括号设置bean的id

   **第三方的bean**要想加入到Spring容器，就需要**以方法的返回值返回这个第三方bean的对象**来注入到容器中，这个方法一般在称为配置类中，并使用**@Bean**

   ```java
   @Configuration//这个配置类也得是Spring容器中的，所以@Component和@Configuration是一样的，一般做配置的类建议用@Configuration
   public class DBConfig {//一般是一个配置类
       @Bean//将方法的返回值定义为Spring管控的bean
       public DruidDataSource dataSource() {
           return new DruidDataSource();
       }
   }
   ```





三：配置类+扫描+注解

**注解方式声明Spring配置类（去掉XML文件）**

以一个配置类直接代替xml文件，component-scan变为了**@ComponentScan**

```java
@ComponentScan({"com.wyh.bean","com.wyh.config"})//指定扫描bean的位置 -> 也就是component-scan
public class SpringConfig {//这个类就代替了xml文件
}
```

【注】这个Spring的配置类，在**AnnotationConfigApplicationContext**构造时，传入的类也会变为一个Bean，所以SpringConfig也会成为一个Bean，所以不用加@Configuration，也就是这个**配置类如果不被扫描，那么可以省略@Configuration**

例如：

```java
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig.class);
```



扩展-1：**FactoryBean接口**

初始化实现FactoryBean接口的类，**实现对bean加载到容器之前的批处理操作**

例如：

```java
public class BookFactoryBean implements FactoryBean<Book> {
    /*
    	实现FactoryBean接口需要至少实现这两个方法，一个是返回对象，一个是返回对象类型，还有一个是 指定是否为单例（一般默认为单例）
    */
    public Book getObject() throws Exception {
        Book book = new Book();
        //进行对book对象的相关初始化操作 这也就是FactoryBean的作用
        return book;
    }
    
    public Class<?> getObjectType() {
        return Book.class;
    }
}
```

实现了这个FactoryBean接口了之后，我们在用方法返回值将Bean加入到Spring容器时，可以用这个FactoryBean类型获取想要的类型对象

例如：

```java
@Bean
public BookFactoryBean book() {//这个BookFactoryBean本身不会成为一个bean，而是会被调用里面的getObject，将该方法的返回值作为bean放入Spring容器
	return new BookFactoryBean();
}
```



扩展-2：**@ImportResource注解**

**加载配置类的时候同时加载配置文件**（可以做系统迁移）

也就是向一个以注解声明Bean的项目中，加入XML中声明的bean，使用**@ImportResource注解**

例如：

```java
@Configuration
@ComponentScan("com.wyh")
@ImportResource("applicationContext-config.xml")//加载xml中的配置
public class SpringConfig {}
```



扩展-3：**@Configuration**的**proxyBeanMethods**属性

@Configuration和@Component的区别在于多了个proxyBeanMethods属性，**此属性是保障调用获取Bean的方法得到的对象是从Spring容器中获取的，也就是复用的，而不是重新创建的**。

此属性，将类的对象变为了**代理对象**，通过这个代理对象去调用自己里面的获取Bean的方法，就会去Spring容器查找已经被加载的Bean

例如：

```java
@Configuration(proxyBeanMethods=true)//默认值为true，开启代理Bean方法
public class SpringConfig {
    @Bean//让这个对象被加载在Spring容器中，代理对象SpringConfig就会去Spring容器找
    public Book book() {
     return new Book();   
    }
}

//----------------------------------------

public class App {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig.class);
        SpringConfig springConfig = context.getBean("springConfig",SpringConfig.class);
        springConfig.book();
        springConfig.book();//这两次调用得到的Book对象是同一个
    }
}
```

这也就解释了在消息队列的配置类中，我们绑定交换机和消息队列时，例如：

```java
return BindingBuilder.bind(topicQueue1()).to(topicExchange()).with("topic.*.id");
```

是调用的方法来获取消息队列和交换机来执行绑定的，就是因为配置类上的@Configuration默认开启了代理Bean方法





四：**@Import**导入的方式

使用@Import注解导入要注入的bean的字节码文件即可

例如：

```java
@Import({Dog.class, Cat.class})//只能导入一次
public class SpringConfig2 {
}
```

【注】**被导入的bean无需声明为bean**，可以有效地降低耦合度



扩展：可以**使用@Import导入配置类**

```java
@Import({DBConfig.class})
```

这样的话，这个**配置类会成为bean**，同时**配置类里面声明的bean也会被加载**到Spring容器





五：**上下文对象调用register方法**

在上下文对象**容器初始化完成之后，手工注册加载bean**

例如：

```java
public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(SpringConfig2.class);
        //在Spring的应用程序上下文对象初始化完毕后，手工的注册加载bean
        context.registerBean("tom", Cat.class);
        context.registerBean("tom", Cat.class);//覆盖思想，后加载的覆盖之前的
        context.register(Mouse.class);//直接加载类的字节码，bean的id为类的首字母小写
        for (String beanDefinitionName : context.getBeanDefinitionNames()) {
            System.out.println(beanDefinitionName);
        }
    }
```





六：@Import导入**ImportSelector接口**，条件式

也就是对于实现了ImportSelector接口的类，我们**可以编程式的控制需要加载的bean或者条件式的做一些逻辑**

而**导入了实现了ImportSelector接口的类的类，也就是一个元数据**，我们可以获取元数据的一些数据。同时我们可以在实现了ImportSelector接口的类里面进行条件控制

例如：

```java
public class MyImportSelector implements ImportSelector {
    @Override
    public String[] selectImports(AnnotationMetadata metadata) {//我们可以获取元数据的信息 metadata就是元数据
        System.out.println("metadata: "+metadata.getClassName());//获取元数据类名
        //判断元数据是否含有某个注解
        System.out.println("metadata: "+metadata.hasAnnotation("org.springframework.context.annotation.Configuration"));
		//获取元数据的某个注解的属性值
        Map<String, Object> attributes = metadata.getAnnotationAttributes("org.springframework.context.annotation.ComponentScan");
        System.out.println(attributes);
        
        //根据元数据的信息进行条件判断，从而控制bean的加载
        if (metadata.hasAnnotation("org.springframework.context.annotation.Configuration")) {
            return new String[]{"com.wyh.Mouse"};
        }

        return new String[]{"com.wyh.bean.Dog","com.wyh.bean.Cat"};
    }
}
```

```java
@Import({MyImportSelector.class})//这里导入了实现了ImportSelector接口的类，那么SpringConfig3就是一个元数据
@Configuration
@ComponentScan("com.wyh.bean")
public class SpringConfig3 {
}
```





七：@Import导入**ImportBeanDefinitionRegistrar接口**

导入实现了ImportBeanDefinitionRegistrar接口的类，与ImportSelector一样，会成为一个元数据

在实现了ImportBeanDefinitionRegistrar接口的类的里面，我们可以**通过BeanDefinition的注册器注册实名bean，实现对容器中bean的绑定，或者对现有bean的覆盖，进而达到在不修改源代码的条件下，更换实现的效果**

例如：

```java
public class MyRegistrar implements ImportBeanDefinitionRegistrar {
    @Override
    public void registerBeanDefinitions(AnnotationMetadata metadata, BeanDefinitionRegistry registry) {
        //可以拿元数据进行条件判断

        //然后拿registry对象注册bean
        //用BeanDefinitionBuilder生成一个bean同时获取他的BeanDefinition对象
        BeanDefinition beanDefinition = BeanDefinitionBuilder.rootBeanDefinition(Dog.class).getBeanDefinition();
        //然后注册bean，指定beanID名和哪个bean的BeanDefinition对象
        registry.registerBeanDefinition("yellow",beanDefinition);
    }
}
```





八：@Import导入**BeanDefinitionRegistryPostProcessor**接口，**后处理器**式

导入实现了BeanDefinitionRegistryPostProcessor接口的类，在该类里，通过BeanDefinition的注册器注册实名bean，**实现对容器中bean的最终裁定**

例如：

```java
public class MyPostProcessor implements BeanDefinitionRegistryPostProcessor {
    @Override
    public void postProcessBeanDefinitionRegistry(BeanDefinitionRegistry registry) throws BeansException {
        BeanDefinition beanDefinition = BeanDefinitionBuilder.rootBeanDefinition(BookServiceImpl4.class).getBeanDefinition();
        registry.registerBeanDefinition("bookService",beanDefinition);//如果存在重名的bean，这里的后处理器里的定义为最终效果
    }

    @Override
    public void postProcessBeanFactory(ConfigurableListableBeanFactory configurableListableBeanFactory) throws BeansException {

    }
}
```







## bean的加载控制

> 指根据特定情况对bean进行选择性加载以达到适用于项目的目标



也就是可以条件式的编程控制bean的加载，那么bean的加载方式中，（五）~（八）的方式都可以

**编程式的控制**：根据任意条件确认是否加载bean

例如：用ImportSelector接口

```java
public class MyImportSelector implements ImportSelector {
    @Override
    public String[] selectImports(AnnotationMetadata annotationMetadata) {
        Class<?> aClass = null;
        try {
            aClass = Class.forName("com.wyh.bean.Mouse");//意为如果环境中有Mouse这个类，那么就加载一个Cat的bean
            if (aClass != null) {
                return new String[]{"com.wyh.bean.Cat"};
            }
        } catch (ClassNotFoundException e) {
            return new String[0];
        }

        return null;
    }
}
```



编程式代码庞大，所以有优化

**注解式的控制**：在自定义的bean的上方加上**@Conditional**的注解，但由于**原生@Conditional注解复杂**，所以**SpringBoot衍生出了许多@Conditional的衍生注解，可以直接按注解判断条件**

例如：

以方法返回值作bean的控制：

```java
@Import({Mouse.class, Dog.class})
public class SpringConfig {
    @Bean
    //@ConditionalOnClass(name = "com.wyh.bean.Mouse")//环境中有Mouse这个类就加载这个bean
    //@ConditionalOnMissingClass("com.wyh.bean.Mouse")//环境中没有Mouse这个类就加载这个bean
    //@ConditionalOnMissingBean(name = "com.wyh.bean.Dog")//环境中没有Dog这个bean就加载这个bean
    @ConditionalOnBean(name = "Jerry")//环境中有Mouse这个bean就加载这个bean
    @ConditionalOnWebApplication//以web形式加载时加载bean
    //@ConditionalOnNotWebApplication//以非web形式加载时加载bean
    public Cat tom() {//有多个Conditional时得都满足才会加载这个bean
        return new Cat();
    }
}
```

直接自定义bean的控制：

```java
@Component("Tom")
//@ConditionalOnClass(name = "com.wyh.bean.Mouse")//环境中有Mouse这个类就加载这个bean
//@ConditionalOnMissingClass("com.wyh.bean.Mouse")//环境中没有Mouse这个类就加载这个bean
//@ConditionalOnMissingBean(name = "com.wyh.bean.Dog")//环境中没有Dog这个bean就加载这个bean
@ConditionalOnBean(name = "Jerry")//环境中有Mouse这个bean就加载这个bean
//@ConditionalOnWebApplication//以web形式加载时加载bean
@ConditionalOnNotWebApplication//以非web形式加载时加载bean
public class Cat {
}
```

```java
@ComponentScan("com.wyh.bean")//需要扫描自定义bean的包
public class SpringConfig {
}
```

匹配指定环境再加载bean：

```java
public class SpringConfig {
    @Bean
    @ConditionalOnClass(name = "com.mysql.jdbc.Driver")//如果连接了数据库就加载Druid数据源
    public DruidDataSource dataSource() {
        return new DruidDataSource();
    }
}
```







## bean依赖属性配置



我们可以将业务功能bean在运行时需要的资源抽取出来，创建为一个独立的**属性类（也就是XxxxProperties类），专门用于读取配置文件信息**

那么这个业务功能bean就不需要添加@Component或者@ConfigurationProperties之类的注解了，在启动类上直接用@Import导入这个业务bean，解耦合

业务bean的属性可以设定为默认值，当需要时可以设置通过配置文件传递属性，而业务bean通常要避免被强制加载，应该是根据需要导入

例如：

用于读取配置文件信息的属性类

```java
@ConfigurationProperties(prefix = "cartoon")
@Data//为cat和mouse提供get/set方法，不然配置文件内的属性没办法注入到这里面
public class CartoonProperties {
    private Cat cat;
    private Mouse mouse;
}
```

将这个属性类的信息用于业务bean

```java
@EnableConfigurationProperties(CartoonProperties.class)//这个注解将添加了@ConfigurationProperties的类归为bean
public class CartoonCatAndMouse {
    private Cat cat;
    private Mouse mouse;
    private CartoonProperties cartoonProperties;

    public CartoonCatAndMouse(CartoonProperties cartoonProperties) {
        this.cartoonProperties = cartoonProperties;
        cat = new Cat();
        cat.setName(cartoonProperties.getCat() != null && StringUtils.hasText(cartoonProperties.getCat().getName()) ? cartoonProperties.getCat().getName() : "tom");//这里就是有配置文件信息就用配置文件的，没有就用默认的
        cat.setAge(cartoonProperties.getCat() != null && cartoonProperties.getCat().getAge() != null ? cartoonProperties.getCat().getAge() : 20);
        mouse = new Mouse();
        mouse.setName("jerry");
        mouse.setAge(3);
    }

    public void play() {
        System.out.println("this is a cartoon");
        System.out.println(cat.getAge()+"岁的"+cat.getName()+"和"+mouse.getAge()+"岁的"+mouse.getName()+"在追逐嬉戏");
    }
}
```

启动类上只需要导入这个业务bean

```java
@SpringBootApplication
@Import(CartoonCatAndMouse.class)
public class SpringBoot23BeanPropertiesApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext context = SpringApplication.run(SpringBoot23BeanPropertiesApplication.class, args);
        CartoonCatAndMouse cartoonCatAndMouse = context.getBean(CartoonCatAndMouse.class);
        cartoonCatAndMouse.play();
    }
}
```







## 自动配置原理



自动配置思想：

1. 收集Spring开发者的编程习惯，整理开发过程中使用的**常用技术列表** --> （**技术集A**）
2. 收集常用技术（技术集A）的使用参数，整理开发过程中每个技术的**常用设置列表** --> （**设置集B**）
3. 初始化SpringBoot的基础环境，加载用户自定义的bean和导入的其他依赖，形成**初始化环境**
4. 将**技术集A**包含的所有技术都定义出来，在Spring/SpringBoot**启动时默认全部加载**
5. 将**技术集A**中具有使用条件的技术约定出来，设置成按条件加载，由开发者决定是否使用该技术（与初始化环境比对）
6. 将**设置集B**作为**默认配置加载**（约定大于配置），减少开发者的配置工作量
7. 开放**设置集B**的**配置覆盖接口**，由开发者根据自身需要决定是否覆盖默认配置



也就是，先把所有的技术实现出来，然后**默认全部加载，然后去检查条件，满足条件的加载，不满足的不加载**



对于@SpringBootApplication注解：

```java
/* 主要由以下三个注解，各个注解又下分几个注解
 *
 * @SpringBootConfiguration
 *     -> @Configuration
 *          --> @Component
 *     -> @Indexed
 * @EnableAutoConfiguration  :这就是关于自动配置的注解
 *     -> @AutoConfigurationPackage
 *          --> @Import({AutoConfigurationPackages.Registrar.class}) :设置当前配置所在的包为扫描包，后续针对当前包进行扫描
 *     -> @Import({AutoConfigurationImportSelector.class}) :
 * @ComponentScan(
 *     excludeFilters = {
 *      @Filter(type = FilterType.CUSTOM,classes = {TypeExcludeFilter.class}),
 *      @Filter(type = FilterType.CUSTOM,classes = {AutoConfigurationExcludeFilter.class})})
 */
```



例如SpringBoot源码中：

```java
Enumeration<URL> urls = classLoader.getResources("META-INF/spring.factories");
```

就是将spring.factories文件中的所有与autoconfig相关的配置全部加载，一共130项



ApplicationContextAware：

* 对于实现了这个接口的类，可以在该类中调用Spring的上下文对象

【注】对于XxxAware，都是差不多这样的功能



* 先开发若干技术的标准实现
* SpringBoot启动时加载所有的技术实现对应的自动配置类
* 检测每个配置类的加载条件是否满足并进行对应的初始化
* 是先加载所有的外部资源，然后根据外部资源进行条件对比





## 变更自动配置



**添加自定义的自动配置**：

1. 首先你得拥有自动配置类：

   例如：

   ```java
   @EnableConfigurationProperties(CartoonProperties.class)
   @ConditionalOnClass(name = "org.springframework.data.redis.core.RedisOperations")//当加载了Redis的依赖坐标，那么就加载这个自动配置类
   public class CartoonCatAndMouse implements ApplicationContextAware {
       private Cat cat;
       private Mouse mouse;
       private CartoonProperties cartoonProperties;
   
       public CartoonCatAndMouse(CartoonProperties cartoonProperties) {
           this.cartoonProperties = cartoonProperties;
           cat = new Cat();
           cat.setName(cartoonProperties.getCat() != null && StringUtils.hasText(cartoonProperties.getCat().getName()) ? cartoonProperties.getCat().getName() : "tom");
           cat.setAge(cartoonProperties.getCat() != null && cartoonProperties.getCat().getAge() != null ? cartoonProperties.getCat().getAge() : 20);
           mouse = new Mouse();
           mouse.setName("jerry");
           mouse.setAge(3);
       }
   
       public void play() {
           System.out.println("this is a cartoon");
           System.out.println(cat.getAge()+"岁的"+cat.getName()+"和"+mouse.getAge()+"岁的"+mouse.getName()+"在追逐嬉戏");
           System.out.println("then, I get the BeanContext,so, I will display the Bean's id");
           for (String beanDefinitionName : applicationContext.getBeanDefinitionNames()) {
               System.out.println(beanDefinitionName);
           }
   
       }
   
       private ApplicationContext applicationContext;
   
       @Override
       public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
           this.applicationContext = applicationContext;
       }
   }
   ```

   ```java
   @ConfigurationProperties(prefix = "cartoon")
   @Data//为cat和mouse提供get/set方法，不然配置文件内的属性没办法注入到这里面
   public class CartoonProperties {
       private Cat cat;
       private Mouse mouse;
   }
   ```

2. 在**resource**目录下创建**META-INF目录**，再创建**spring.factories**文件

   spring.factories文件的大致内容为：

   ```properties
   # Auto Configure
   org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
     com.wyh.bean.CartoonCatAndMouse  ##指定自动配置类的全路径名
   ```

那么，就算启动类没有导入这个自动配置类，也会自动加载上（**只要符合自动配置类的加载条件**）



**排除自动配置**：

**application.yml文件中**配置排除自动配置

例如：

```yml
spring:
  autoconfigure:
    exclude:
    - org.springframework.boot.autoconfigure.task.TaskExecutionAutoConfiguration
    - org.springframework.boot.autoconfigure.Xxxxx
```

或者直接**在@SpringBootApplication注解上的excludeName属性或exclude属性中**配置排除

例如：

```java
@SpringBootApplication(excludeName = {"org.springframework.boot.autoconfigure.context.LifecycleAutoConfiguration"})
public class SpringBoot23BeanPropertiesApplication {}
```



有一些得在Maven的pom文件中设置排除依赖才可以排除掉自动配置

比如排除内嵌的Tomcat服务器：（排除掉了tomcat记得加上jetty）

```xml
<dependency>
	<groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions><!--排除依赖-->
    	<exclusion><!--具体排除的依赖-->
        	<groupId>org.springframework.boot</groupId>
   		 	<artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```







# 自定义starter💡

> 由案例引出自定义starter



**案例：统计独立IP访问次数**

在导入了对应的starter之后，可以做到：

* 每次访问网站行为均进行统计
* 后台每十秒输出一次监控信息（格式为：IP+访问次数）



案例需求分析：

1. 数据记录位置：Map/Redis
2. 功能触发位置：每次Web请求（拦截器）
   * 步骤一：从简单起手，主动调用，仅统计单一操作访问次数（例如查询）
   * 步骤二：开发拦截器
3. 业务数据（配置项）
   * 输出频度，默认为10秒
   * 数据特征：累计数据/阶段数据，默认为累计数据
   * 输出格式：详细模式/极简模式





**自定义starter**

starter名称定义：

* 技术名-spring-boot-starter（第三方技术）
* spring-boot-starter-技术名（官方制作的技术的命名）

starter模块定义：

* 一个starter的空壳模块，只有pom文件，另一个是META-INF+Autoconfiguration配置类的模块
* 或者一个模块全包含



使用一个模块格式：

1. 创建模块：技术名-spring-boot-starter

2. 勾选web技术：spring-boot-starter-web

3. 定义技术业务功能：

   例如：

   ```java
   public class IpCountService {
       private final Map<String,Integer> ipCountMap = new HashMap<>();//数据存储的地方
       private final HttpServletRequest httpServletRequest;//获取访问请求对象
   
       @Autowired
       public IpCountService(HttpServletRequest httpServletRequest) {
           this.httpServletRequest = httpServletRequest;
       }
   
       /**
        * @author iWyh2
        * @date [2022/12/6 0006 21:42]
        * @description 每次调用给这个方法，就记录IP和累加访问次数
        */
       public void count() {
           //1.获取访问ip
           String ip = httpServletRequest.getRemoteAddr();
           System.out.println("===一次IP统计:"+ip+"===");
           //2.根据ip从map中找值并累加
           if (ipCountMap.containsKey(ip)) {
               ipCountMap.put(ip,ipCountMap.get(ip) + 1);
           } else {
               ipCountMap.put(ip,1);
           }
       }
   }
   ```

4. 定义自动配置类，自动配置类的作用为将业务bean导入Spring容器

   例如：

   ```java
   @Import(IpCountService.class)
   public class IpAutoConfiguration {
   }
   ```

5. 定义自动配置类被自动加载的配置文件，在resource目录下，创建META-INF/spring.factories

   例如：

   ```properties
   # Auto Configure
   org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
     com.wyh.autoconfig.IpAutoConfiguration
   ```

这样，自定义starter初级版就做好了（其实此时自定义starter就已经算是做完了，后面的都只算是完善）

然后我们只需要导入这个自定义starter，注入ipCountService业务对象调用count方法即可

【注】在导入自定义starter之前，需要我们先将自定义starter模块**clean**，再**install**到本地maven仓库





**辅助功能开发**



**定时展示**：

初级版自定义starter只是做到了记录存储访问次数的数据，现在需要将存储的数据展示出来（这与自定义starter毫无关系）

实现：利用定时任务技术做到每十秒展示访问次数数据 - SpringTask

```java
@Scheduled(cron = "0/5 * * * * ?")
public void print() {
    System.out.println("        IP访问监控");
    System.out.println("+-----ip-address-----+--num--+");

    for (Map.Entry<String, Integer> entry : ipCountMap.entrySet()) {
        String key = entry.getKey();
        Integer value = entry.getValue();
        System.out.printf("|%18s  |%5d  |\n", key, value);
    }
    System.out.println("+--------------------+-------+");
}
```

【注】这里所用的printf方法，和C语言的printf一样的用法，格式化输出字符串

还需要开启Spring内置的任务技术：使用@EnableScheduling注解

```java
@Import(IpCountService.class)
@EnableScheduling
public class IpAutoConfiguration {
}
```

开启任务功能的地方必须是整个应用的核心地方，显然这个自动配置类是核心被加载的类



**配置属性**：

对于一个技术，我们应该允许用户在yml配置文件中配置我们技术的属性，做到自定义功能或切换

那么读取配置文件中的配置信息，我们需要一个属性类，并拥有默认的配置属性：

```java
@ConfigurationProperties(prefix = "wyh-tools.ip")//如果应用中的配置文件配了自定义属性，那么就读取并覆盖掉下面的默认值
@Data//提供getter和setter方法，这样才能读取并覆盖
public class IpProperties {
    /*日志的显示周期*/
    private Long cycle = 5L;
    /*是否周期内重置数据*/
    private Boolean cycleReset = false;
    /*日志的输出模式，明细-detail，简单-simple*/
    private String model = LogModel.DETAIL.value;
    public enum LogModel {
        DETAIL("detail"),
        SIMPLE("simple");
        private String value;
        LogModel(String value) {
            this.value = value;
        }
        public String getValue() {
            return value;
        }
    }
}
```

要使其生效，我们需要将这个属性类设置为bean，使用@EnableConfigurationProperties即可：

```java
@Import(IpCountService.class)
@EnableScheduling
@EnableConfigurationProperties(IpProperties.class)
public class IpAutoConfiguration {
}
```

丰富业务展示数据功能：

```java
@Autowired
    private IpProperties ipProperties;//读取应用中配置文件中的配置信息

    @Scheduled(cron = "0/5 * * * * ?")
    public void print() {
        //根据设置的是simple还是detail切换
        if (ipProperties.getModel().equals(IpProperties.LogModel.DETAIL.getValue())) {
            System.out.println("        IP访问监控");
            System.out.println("+-----ip-address-----+--num--+");
            for (Map.Entry<String, Integer> entry : ipCountMap.entrySet()) {
                System.out.printf("|%18s  |%5d  |\n", entry.getKey(), entry.getValue());
            }
            System.out.println("+--------------------+-------+");
        } else if (ipProperties.getModel().equals(IpProperties.LogModel.SIMPLE.getValue())){
            System.out.println("       IP访问监控");
            System.out.println("+-----ip-address-----+");
            for (String  key : ipCountMap.keySet()) {
                System.out.printf("|%18s  |\n", key);
            }
            System.out.println("+--------------------+");
        }

        if (ipProperties.getCycleReset()) {//如果设置的为true，则每次打印完就重置
            ipCountMap.clear();
        }
    }
```

定义好供属性类读取的属性：

```yml
#ip config：自定义starter
wyh-tools:
  ip:
    cycle: 10L
    cycleReset: true
    model: simple
```



**改变定时任务的周期时间属性**：

对于配置文件中配置周期使其生效比较复杂

我们需要将属性类自定义成bean，并自定义名称

```java
@ConfigurationProperties(prefix = "wyh-tools.ip")//@EnableEnableConfigurationProperties会在此处生成bean的id
@Component("ipProperties")//自定义bean的名称
public class IpProperties {}
```

接着我们需要放弃使用@EnableEnableConfigurationProperties来将属性类设置为bean，因为它生成的bean的id名称很麻烦，不符合EL表达式的需要

```java
@Import({IpCountService.class,IpProperties.class})//用Import导入自定义bean，这样bean的id才是我们自定义的（手工控制）
@EnableScheduling
//@EnableConfigurationProperties(IpProperties.class)
public class IpAutoConfiguration {
}
```

最后我们在定时任务的cron表达式上使用EL表达式读取配置文件中的对应配置信息，格式为：**#{bean的id名.属性名称}**

```java
@Scheduled(cron = "0/#{ipProperties.cycle} * * * * ?")
public void print() {}
```



**拦截器**：

ip访问统计逐个在方法里调用肯定不行，所以我们需要用拦截器来拦截请求去统计

创建拦截器

```java
public class IpCountInterceptor implements HandlerInterceptor {
    @Autowired
    private IpCountService ipCountService;

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        ipCountService.count();
        return true;
    }
}
```

让拦截器生效，设置SpringMVC的配置类，加载拦截器

```java
@Configuration(proxyBeanMethods = true)
public class SpringMvcConfig implements WebMvcConfigurer {
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //用方法获取拦截器对象bean，@Configuration保证获取的拦截器是唯一的
        registry.addInterceptor(ipCountInterceptor()).addPathPatterns("/**");//拦截所有的请求路径
    }

    @Bean//将拦截器设置为bean
    public IpCountInterceptor ipCountInterceptor() {
        return new IpCountInterceptor();
    }
}
```



**YAML提示功能开发**：

1. 首先导入一个配置处理器坐标：

   ```xml
   <dependency>
   	<groupId>org.springframework.boot</groupId>
   	<artifactId>spring-boot-configuration-processor</artifactId>
       <optional>true</optional><!--防止向下传递-->
   </dependency>-->
   ```

2. 然后我们clean再install

3. 在target目录下找到META-INF目录，取出里面的**spring-configuration-metadata.json**文件，复制粘贴到自定义starter模块下的META-INF目录中

4. 在spring-configuration-metadata.json文件中做需要的提示配置

   例如这个自定义starter的提示文件：

   ```json
   {
     "groups": [
       {
         "name": "wyh-tools.ip",
         "type": "com.wyh.properties.IpProperties",
         "sourceType": "com.wyh.properties.IpProperties"
       }
     ],
     "properties": [
       {
         "name": "wyh-tools.ip.cycle",
         "type": "java.lang.Long",
         "description": "日志的显示周期",//这里的描述提示，一般会来自你在属性类的属性上写的注释文档内容
         "sourceType": "com.wyh.properties.IpProperties"
       },
       {
         "name": "wyh-tools.ip.cycle-reset",
         "type": "java.lang.Boolean",
         "description": "是否周期内重置数据",
         "sourceType": "com.wyh.properties.IpProperties"
       },
       {
         "name": "wyh-tools.ip.model",
         "type": "java.lang.String",
         "description": "日志的输出模式，明细-detail，简单-simple",
         "sourceType": "com.wyh.properties.IpProperties"
       }
     ],
     "hints": [//具体给哪个属性提示值，且提示的是具体什么值
       {
         "name": "wyh-tools.ip.model",
         "values": [
           {
             "value": "detail",
             "description": "详细模式."
           },
           {
             "value": "simple",
             "description": "简略模式."
           }
         ]
       }
     ]
   }
   ```

   【注】这里的描述提示，一般会来自你在属性类的属性上写的注释文档内容

5. 最后为了防止提示出来的东西有两份，所以在重新clean-install时需要解除掉一开始导入的依赖







# 核心原理📌



## SpringBoot启动流程



1. **初始化各种属性，加载成对象**
   * 读取环境属性（Environment）
   * 系统配置（spring.factories）
   * 参数（Arguments、application.properties/yml）
2. 在**创建容器之前**，通过**监听机制**，应对不同阶段的加载数据，更新数据的需求
3. **创建Spring容器**对象ApplicationContext，**加载各种配置**
4. 在**容器初始化过程中**，追加各种功能（例如统计时间，输出日志等）



以一个空壳SpringBoot程序为例，研究源码：（大致过程）

```java
/*
##SpringBoot应用的开始（引导类执行run方法）
SpringBoot24StartupApplication类 [10行] -> SpringApplication.run(SpringBoot24StartupApplication.class, args);
    =>SpringApplication类 [825行] -> return run(new Class[]{primarySource}, args);
        =>SpringApplication类 [829行] -> return (new SpringApplication(primarySources)).run(args);
            ##加载各种配置信息，初始化各种配置对象
            =>SpringApplication类 [829行] -> new SpringApplication(primarySources)
                =>SpringApplication类 [101行] -> this((ResourceLoader)null, primarySources);
                    =>SpringApplication类 [104行] -> public SpringApplication(ResourceLoader resourceLoader, Class<?>... primarySources) {}
                        ##初始化资源加载器
                        =>SpringApplication类 [117行] -> this.resourceLoader = resourceLoader;
                        ##初始化配置类的类名信息（格式转换，信息未变）
                        =>SpringApplication类 [119行] -> this.primarySources = new LinkedHashSet(Arrays.asList(primarySources));
                        ##确认当前容器加载的类型（是否是web环境）
                        =>SpringApplication类 [120行] -> this.webApplicationType = WebApplicationType.deduceFromClasspath();
                        ##获取系统配置引导信息（spring.factories）
                        =>SpringApplication类 [121行] -> this.bootstrapRegistryInitializers = this.getBootstrapRegistryInitializersFromSpringFactories();
                        ##获取ApplicationContextInitializer.class对应的实例
                        =>SpringApplication类 [122行] -> this.setInitializers(this.getSpringFactoriesInstances(ApplicationContextInitializer.class));
                        ##初始化监听器（获取ApplicationListener.class对应的实例）对初始化过程及运行过程进行干预
                        =>SpringApplication类 [123行] -> this.setListeners(this.getSpringFactoriesInstances(ApplicationListener.class));
                        ##初始化整个应用的引导类类名信息，以备用
                        =>SpringApplication类 [124行] -> this.mainApplicationClass = this.deduceMainApplicationClass();
            ##初始化容器，获取ApplicationContext对象
            =>SpringApplication类 [829行] -> (new SpringApplication(primarySources)).run(args);
                =>SpringApplication类 [154行] -> (public ConfigurableApplicationContext run(String... args) {}
                    ##设置计时器
                    =>SpringApplication类 [155行] -> StopWatch stopWatch = new StopWatch();
                    ##开始计时
                    =>SpringApplication类 [156行] -> stopWatch.start();
                    ##创建系统引导信息对应的上下文对象
                    =>SpringApplication类 [157行] -> DefaultBootstrapContext bootstrapContext = this.createBootstrapContext();
                    ##模拟输入输出信号，避免出现因缺少外设导致的信号传输失败，进而引发错误（模拟显示器，键盘，鼠标...）：java.awt.headless=true 也就是做设备的兼容
                    =>SpringApplication类 [159行] -> this.configureHeadlessProperty();
                    ##获取当前注册的所有监听器
                    =>SpringApplication类 [160行] -> SpringApplicationRunListeners listeners = this.getRunListeners(args);
                    ##监听器执行了对应的操作步骤
                    =>SpringApplication类 [161行] -> listeners.starting(bootstrapContext, this.mainApplicationClass);
                    ##获取传入应用的命令行参数args
                    =>SpringApplication类 [164行] -> ApplicationArguments applicationArguments = new DefaultApplicationArguments(args);
                    ##将前期读取的数据信息加载成了一个环境对象，用来描述信息
                    =>SpringApplication类 [165行] -> ConfigurableEnvironment environment = this.prepareEnvironment(listeners, bootstrapContext, applicationArguments);
                    ##做了一个配置用以备用
                    =>SpringApplication类 [166行] -> this.configureIgnoreBeanInfo(environment);
                    ##初始化打印SpringBoot的banner
                    =>SpringApplication类 [167行] -> Banner printedBanner = this.printBanner(environment);
                    ##创建容器对象，根据前期配置的容器类型进行判定并创建
       *最重要的一步* =>SpringApplication类 [168行] -> context = this.createApplicationContext();
                    ##设置启动模式
                    =>SpringApplication类 [169行] -> context.setApplicationStartup(this.applicationStartup);
                    ##对容器进行设置，参数来源于前期的设定
                    =>SpringApplication类 [170行] -> this.prepareContext(bootstrapContext, context, environment, listeners, applicationArguments, printedBanner);
                    ##刷新容器环境
                    =>SpringApplication类 [171行] -> this.refreshContext(context);
                    ##刷新完毕之后的后处理
                    =>SpringApplication类 [172行] -> this.afterRefresh(context, applicationArguments);
                    ##停止计时
                    =>SpringApplication类 [173行] -> stopWatch.stop();
                    ##判定是否记录启动时间的日志
                    =>SpringApplication类 [174行] -> if (this.logStartupInfo)
                    ##创建日志对应的对象，输出日志信息，包含启动时间信息
                    =>SpringApplication类 [175行] -> new StartupInfoLogger(this.mainApplicationClass).logStarted(this.getApplicationLog(), stopWatch);
                    ##监听器执行了对应的操作步骤
                    =>SpringApplication类 [178行] -> listeners.started(context);
                    ##调用运行器执行操作
                    =>SpringApplication类 [179行] -> this.callRunners(context, applicationArguments);
                    ##监听器执行了对应的操作步骤
                    =>SpringApplication类 [186行] -> listeners.running(context);
*/
```





## 监听器

> 监听器是Spring一个很重要的机制
>
> 它为我们提供了干涉各阶段的数据处理的接口



监听器类型：

* 在应用运行但未进行任何处理时，将发送ApplicationStartingEvent
* 当Environment被使用，且上下文对象创建之前，将发送ApplicationEnvironmentPreparedEvent
* 在开始刷新之前，bean定义被加载发送之后，发送ApplicationPreparedEvent
* 在上下文对象刷新之后且所有的应用和命令行运行器被调用之前发送ApplicationStartedEvent
* 在应用程序和命令行运行器被调用之后，将发出ApplicationReadyEvent，用于通知应用已经准备处理请求
* 启动时发生异常，将发送ApplicationFailedEvent



> ©iWyh2