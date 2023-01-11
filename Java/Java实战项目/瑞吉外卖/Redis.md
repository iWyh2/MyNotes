# Redis

Redis是一个**基于内存**的**Key-Value结构**数据库

* 基于内存，读写性能高
* 适合存储热点数据（热点商品，资讯，新闻）
* 企业应用广泛



## Redis入门

### Redis简介

Redis是一个开源的 基于内存的 数据结构存储系统

可用作：数据库 缓存 消息中间件

由C语言开发

成为结构化的NoSQL（Not Only SQL）数据库（非关系型数据库）

[官网](https://redis.io)



应用场景：

* 缓存
* 任务队列
* 消息队列
* 分布式锁





### Redis下载和安装

[Windows下载](https://github.com/microsoftarchive/redis/releases)

[Linux下载](https://download.redis.io/releases/)



Linux安装：

将tar包放在Linux中合适的位置解压

然后需要将解压后的文件编译 需要gcc环境：yum install gcc-c++

然后在解压后的目录中执行：make

然后进入编译后的src目录中 执行：make install



Windows安装：直接解压使用

* Redis配置文件：redis.windows.conf
* Redis客户端：redis-cli.exe
* Redis服务端：redis-server.exe





### Redis服务启动和停止

Linux中启动redis服务，使用redis-server，默认端口号为6379

Ctrl C停止服务

Linux使用redis-server启动服务会霸屏，所以需要修改配置（redis.conf）

vim redis.conf，找到daemonize，改为yes就是支持后台运行

然后启动redis-server时需要显示指定加载这个配置文件：src/redis-server ./redis.conf



redis可以设置密码访问：

修改redis.conf 在里面添加：requirepass 123456

在客户端访问时，就需要输入auth 123456才可以正常访问



可以远程访问，需要修改配置（redis.conf），将"bind 127.0.0.1"注释掉即可





## 数据类型

Redis存储的是key-value结构的数据

key为字符串类型 value有五种数据类型

* 字符串 String
* 哈希 hash
* 列表 List
* 无序集合 Set
* 有序集合 Sorted Set（ZSet）



普通字符串为常用的

hash适合存储对象

list按照插入顺序排序，可以有重复元素，一般做任务队列

set无序集合，没有重复元素

sorted set有序集合，没有重复元素





## 常用命令

**字符串string操作命令：**

* set key value：设置指定key的值
* get key：获取指定key的值
* setex key seconds value：设置指定key的值，并将key的过期时间设置为seconds秒
* setnx key value：只有在key不存在时设置key的值



**哈希hash操作命令：**

redis的hash是一个string类型的field和value的映射表

* hset key field value：将哈希表key中的字段field的值设为value
* hget key field：获取存储在哈希表中的指定字段的值
* hdel key field：删除存储在哈希表中的指定字段
* hkeys key：获取哈希表中指定key的所有字段（field）
* hvals key：获取哈希表中指定key的所有值（value）
* hgetall key：获取在哈希表中指定key的所有字段和值



**列表list操作命令：**

列表是简单的字符串列表，有插入顺序

* lpush key value1 [value2] ...：将一个或多个值**插入到列表头部**
* lrange key start stop：获取列表指定范围内的元素（0为开始，-1为最后）
* rpop key：移除并获取列表最后一个元素
* llen key：获取列表的长度
* brpop key1 [key2 ..] timeout：移除并获取列表最后一个元素，如果列表没有元素，那么会阻塞列表知道等待超时timeout秒或发现可弹出元素为止
* lrem key count value 移除列表元素



**集合set操作命令：**

set是字符串的无序集合，集合成员唯一

* sadd key member1 [member2 ..]：向集合添加一个或多个成员
* smembers key：返回集合中的所有成员
* scard key：获取集合的成员数
* sinter key1 [key2..]：返回给定所有集合的交集
* sunion key1 [key2..]：返回给定所有集合的并集
* sdiff key1 [key2..]：返回给定所有集合的差集（注意set前后顺序，差值不一样）
* srem key member1 [member2..]：移除集合中一个或多个成员



**有序集合sorted set操作命令：**

有序集合也是字符串的集合，每个元素都有一个double类型的score（分数），通过分数将集合成员从小到大排序，成员唯一，分数可以不唯一

* zadd key score1 member1 [score2 member2..]：向有序集合添加一个或多个成员，或者更新已存在的成员的分数
* zrange key start stop [WITHSCORE]：通过索引区间返回有序集合中指定区间内的成员（withscores为返回时带上分数一起显示）
* zincrby key increment member：有序集合中对指定成员的分数加上增量increment
* zrem key memer1 [memer2..]：移除集合一个或多个成员



**通用命令：**

* keys pattern：查找所有给定模式（pattern）的key（常用keys *查询所有key）
* exists key：检查给定key是否存在
* type key：返回key所存储的值的类型
* ttl key：返回给定key的剩余生存时间（ttl，time to live），以秒为单位（-1为永久）
* del key：key存在就删除key



Redis中一般默认有16个数据库（可以在redis.conf中修改数据库数量）

且一般默认使用0号数据库

切换使用数据库命令：select index（例：select 2）



[更多命令参考Redis中文网](https://www.redis.net.cn)





## 在Java中操作Redis

### 介绍

Redis的Java客户端有很多，官方推荐三种：

* Jedis
* Lettuce
* Redission



Spring对Redis客户端进行了整合，提供了Spring Data Redis，SpringBoot中的starter为"spring-boot-starter-data-redis"





### Jedis

Jedis的Maven坐标：

```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>2.8.0</version>
</dependency>
```

使用Jedis操作redis的步骤：

1. 获取连接
2. 执行操作
3. 关闭连接

例如：

```java
public class JedisDemo {
    public static void main(String[] args) {
        //获取连接
        Jedis jedis = new Jedis("192.xxx.xx.xx",6379);//连接的Linux的Redis
        jedis.auth("123456");//输入密码
        //执行操作
        jedis.set("username","wang");
        jedis.hset("hash1","name","test1");
        jedis.hset("hash1","age","20");
        System.out.println(jedis.hget("hash1", "name"));
        System.out.println(jedis.hgetAll("hash1"));
        System.out.println(jedis.keys("*"));
        //close
        jedis.close();
    }
}
```





### Spring Data Redis

在SpringBoot项目中可以使用Spring Data Redis简化redis操作



Spring Data Redis的Maven坐标：

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
```

Spring Data Redis提供了一个高度封装的类：RedisTemplate

针对jedis客户端中的大量api进行了归类封装，将统一类型操作封装为了一类operation接口

* ValueOperations：简单的K-V操作（字符串）
* SetOperations：set类型操作
* ZSetOperations：zset类型操作
* HashOperations：map类型操作
* ListOperations：list类型操作



首先配置：

```yml
spring:
  #配置Redis
  redis:
    host: 192.xxx.xx.xx #Linux的Redis
    port: 6379
    password: 123456
    database: 0         #使用0号数据库
    jedis:
      #Redis连接池配置
      pool:
        max-active: 8   #最大连接数
        max-wait: 1ms   #连接池最大阻塞等待时间
        max-idle: 4     #连接池最大空闲连接
        min-idle: 0     #连接池最小空闲连接
```

测试类：

```java
@SpringBootTest
class SpringDataRedisApplicationTests {
    @Autowired
    private RedisTemplate<String,String> redisTemplate;//这里指定泛型为String 是一般通用的 也是一般常用的 可以避免使用RedisConfig

    @Test
    void SpringDataRedisTest() {
        //String类型
        //.opsForValue()获取ValueOperations对象
        redisTemplate.opsForValue().set("car","BMW");
        redisTemplate.opsForValue().set("day","2023/1/10",3, TimeUnit.SECONDS);
        System.out.println(redisTemplate.opsForValue().get("day"));

        //Hash类型
        //.opsForHash()获取HashOperations对象
        redisTemplate.opsForHash().put("zyh","name","zyh");
        redisTemplate.opsForHash().put("zyh","age","19");
        System.out.println(redisTemplate.opsForHash().get("zyh", "name"));
        System.out.println(redisTemplate.opsForHash().get("zyh", "age"));
        System.out.println(redisTemplate.opsForHash().keys("zyh"));//获取所有field
        System.out.println(redisTemplate.opsForHash().values("zyh"));//获取所有value

        //List类型
        //.opsForList()获取ListOperations对象
        redisTemplate.opsForList().leftPush("list1","a");//一次存入一个值
        redisTemplate.opsForList().leftPushAll("list1","a","b","c","d");//一次存入多个值
        System.out.println(redisTemplate.opsForList().size("list1"));//长度
        System.out.println(redisTemplate.opsForList().range("list1", 0, -1));
        System.out.println(redisTemplate.opsForList().rightPop("list1"));
        System.out.println(redisTemplate.opsForList().leftPop("list1"));

        //Set类型
        //.opsForSet()获取SetOperations对象
        redisTemplate.opsForSet().add("set3","a","b","c");
        System.out.println(redisTemplate.opsForSet().members("set3"));
        redisTemplate.opsForSet().remove("set3","a","b");
        System.out.println(redisTemplate.opsForSet().members("set3"));

        //ZSet类型
        //.opsForZSet()获取ZSetOperations对象
        redisTemplate.opsForZSet().add("ZSet1","a",1.0);
        redisTemplate.opsForZSet().add("ZSet1","b",1.1);
        redisTemplate.opsForZSet().add("ZSet1","c",1.2);
        redisTemplate.opsForZSet().add("ZSet1","d",1.3);
        System.out.println(redisTemplate.opsForZSet().range("ZSet1", 0, -1));
        redisTemplate.opsForZSet().incrementScore("ZSet1","a",0.2);
        System.out.println(redisTemplate.opsForZSet().range("ZSet1", 0, -1));
        redisTemplate.opsForZSet().remove("ZSet1","d","c");
        System.out.println(redisTemplate.opsForZSet().range("ZSet1", 0, -1));

        //通用命令
        //keys *
        System.out.println(redisTemplate.keys("*"));
        //exists key
        System.out.println(redisTemplate.hasKey("wyh"));
        //del key
        System.out.println(redisTemplate.delete("car"));
        //type key
        System.out.println(Objects.requireNonNull(redisTemplate.type("ZSet1")).name());
    }
}
```

配置类：

```java
/**
 * @description
 * 这个类是为了配置Spring DataRedis
 * Redis会对key进行序列化
 * 当我们使用RedisTemplate，没有指定泛型时
 * key的序列化会出现异常 导致无法将key-value存入redis
 * 所以此处重新设置了key的序列化器 使得可以正确存入key
 *
 * 但我发现 指定RedisTemplate的泛型 也就是RedisTemplate<String,String>也可以解决序列化异常
 */

//@Configuration
public class RedisConfig extends CachingConfigurerSupport {
    @Bean
    public RedisTemplate<Object,Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) {
        RedisTemplate<Object, Object> redisTemplate = new RedisTemplate<>();
        //默认的key序列化器为：JdkSerializationRedisSerializer
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setHashKeySerializer(new StringRedisSerializer());
        redisTemplate.setConnectionFactory(redisConnectionFactory);
        return redisTemplate;
    }
}
```

