# Spring Cache

## 介绍

Spring Cache是一个框架，实现基于注解的缓存功能

只需要**添加注解即可实现缓存功能**



Spring Cache提供了一层抽象，**底层可以切换不同的cache实现**

CacheManager是Spring提供的各种缓存技术的抽象接口

也就是**通过CacheManager接口来统一不同的缓存技术**



不同技术需要实现不同的CacheManager：

|    CacheManager     |                描述                |
| :-----------------: | :--------------------------------: |
| EhCacheCacheManager |      使用EhCache作为缓存技术       |
|  GuavaCacheManager  | 使用Google的GuavaCache作为缓存技术 |
|  RedisCacheManager  |       使用Redis作为缓存技术        |

------





## 使用

Spring Cache常用注解：

|      注解      |                             说明                             |
| :------------: | :----------------------------------------------------------: |
| @EnableCaching |                       开启注解缓存功能                       |
|   @Cacheable   | 在方法执行前，Spring先查看缓存中是否有数据，有数据则直接返回缓存数据，没有数据则调用方法将执行方法的返回值存入缓存 |
|   @CachePut    |                   将方法的返回值放到缓存中                   |
|  @CacheEvict   |                 将一条或多条数据从缓存中删除                 |

@CachePut注解：将方法的返回值放到缓存中

属性：

* value：缓存的名称，每个缓存名称下可以有多个key（也就是缓存的分类）
* key：缓存的key（可以使用SpEL表达式动态的生成key）（key="#result.id" / "root.method"）

@CacheEvict：清理指定缓存（相当于精准清理缓存）

属性：

* value：缓存的名称，每个缓存名称下可以有多个key（也就是缓存的分类）
* key：缓存的key（可以使用SpEL表达式动态的生成key）（key="#id" / "#p0" / "root.args[0]"）
* allEntries：将缓存名称下的缓存数据全部删除，默认为false 需要手动设置为true（也就是删除全部缓存）

@Cacheable：先从缓存获取，没有就查数据库并存入缓存

属性：

* value：缓存的名称，每个缓存名称下可以有多个key（也就是缓存的分类）
* key：缓存的key（可以使用SpEL表达式动态的生成key）（key="#id" / "#p0" / "root.args[0]"）
* condition：存入缓存的条件，满足条件才存入缓存
* unless：存入缓存的条件，不满足条件才存入缓存



在SpringBoot项目中，使用缓存技术只需要在项目中**导入相关缓存技术的依赖包**，并**在启动类上使用@EnableCaching开启注解缓存功能**

例如，使用Redis作为缓存技术，只需要项目导入Spring Data Redis即可

使用方式：

1. 导入maven坐标：spring-boot-starter-data-redis spring-boot-starter-cache(此包内有各CacheManager的扩展)

2. 配置application.yml

   ```yml
   spring:
     #redis配置
     redis:
       host: localhost
       port: 6379
       database: 0
     #SpringCache
     cache:
       redis:
         time-to-live: 1800000 #缓存过期时间 1800s 单位为ms
   ```

3. 启动类添加@EnableCaching

4. 再需要缓存的各方法上添加缓存操作注解

