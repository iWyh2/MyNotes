# SpringBoot

> SpringBoot如今市场占有率极高📊
>
> 是Java开发人员必备技能之一💸
>
> 由Pivotal团队开发
>
> 目的🏷：**简化** Spring应用的 **初始搭建** 和 **开发过程**



# [基础篇]

# 1. 快速上手SpringBoot



## 1.1 SpringBoot入门程序的开发



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

​	继承了spring-boot-starter-parent这个pom文件，内置一个dependencies的pom文件，里面的属性全是版本

​	也就是定义了若干个依赖管理（坐标版本号）

​	继承这个spring-boot-starter-parent模块，就可以避免多个依赖之间出现依赖版本冲突问题，也就是**依赖的版本交给SpringBoot管理了**

​	除了parent继承之外，还可以用引入依赖的形式实现这样的效果（阿里云版本就是这样实现的）

​	**目的：减少依赖冲突**



2. **starter**（快速启动）

​	**内置了当前技术需要使用的所有的依赖**（Maven依赖传递）

​	**目的：减少依赖配置**

* 以上两个技术也就是让我们最终在实际开发中，只需要GAV的G和A，V(版本)由SpringBoot提供，未收录的技术需要自己手动添加GAV（**解决配置问题**）
* 坐标错误需要指定V



3. **引导类**（XxxApplication）

​	由SpringBoot自动帮你创建的一个类

​	引导类是boot工程的执行入口，运行引导类的main方法就可以**启动boot项目**

​	运行之后，会**初始化Spring容器（ApplicationContext），会扫描引导类所在的包去加载bean**



4. **内嵌Tomcat**（辅助功能）

​	内置tomcat服务器的依赖

​	将tomcat容器抽象为一个对象放在Spring容器中运行

​	不想用默认的tomcat，**可以用Maven的排除依赖排除，然后重新设置依赖，或者称之为 某技术的starter**

​	比如，可以将tomcat更换为**jetty**服务器（Maven私服的Nexus就是用的这个服务器）

​	SpringBoot内置三款服务器：1) tomcat  2) jetty 3) undertow，了解即可（tomcat yyds😏）





# 2. SpringBoot基础配置



## [知识补充] 复制工程

> 原则：
>
> * 保留工程基础结构
> * 抹掉原始工程痕迹



1. 在工作空间（也就是放Java文档的地方）中复制一个工程，并修改名称为备份
2. 删除与IDEA相关配置文件。仅保留src文件和pom.xml文件
3. 修改pom.xml文件中的artifactId与这个备份工程名称一致，之后复制也要修改这个
4. 备份文件就保留在工作空间中，供之后使用





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





## 2.2 配置文件分类



## 2.3 yaml文件



## 2.4 yaml数据读取





# 3. 基于SpringBoot实现SSM整合





# [应用篇]

# [原理篇]