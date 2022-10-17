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



### 1.1.1 四种SpringBoot工程创建的方式

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



SpringBoot开发对比Spring开发：

|        类/配置文件         |  Spring  |  SpringBoot  |
| :------------------------: | :------: | :----------: |
|    **pom文件中的坐标**     | 手工添加 | **勾选添加** |
|      **web3.0配置类**      | 手工添加 |      无      |
| **Spring/SpringMVC配置类** | 手工添加 |      无      |
|         **控制器**         | 手工添加 |   手工添加   |



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





# 2. SpringBoot基础配置

# 3. 基于SpringBoot实现SSM整合





# [应用篇]

# [原理篇]