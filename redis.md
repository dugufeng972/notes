## reids应用场景

1. 有效期验证码

2. 保存用户的登录信息

## Redis常见命令

### Redis数据结构介绍

Redis是一个key-value的数据库，key一般是String类型，不过value的类型多种多样：

| value类型       | value举例               | 分类   |
| ------------- | --------------------- | ---- |
| String字符串     | hello                 | 基本类型 |
| Hash          | {name:"jack", age:21} | 基本类型 |
| List          | [A->B->C->D]          | 基本类型 |
| set无序集合       | {A,B,C}               | 基本类型 |
| SortedSet有序集合 | {A:1, B:2, C:3}       | 基本类型 |
| GEO           | {A:(120.3, 30.5)}     | 特殊类型 |
| BitMap        | 01101010101           | 特殊类型 |
| HyperLog      | 01101100001           | 特殊类型 |

### Redis常用命令--通用命令

通用指令是部分数据类型都可以使用的指令，常见的有：

1. keys:查看符合模板的所有key，不建议在生产环境设备上使用(模糊查询很浪费时间)
   
   * 语法：keys patterns
     * 例：查找所有键       
       * keys * 
     * 例：查找以a开头的键
       * keys a*

2. del：删除一个指定的key
   
   * 删除一个键：del age
   * 删除多个键：del name gender

3. exists：判断key是否存在
   
   * 例：exists home
   * 注：存在返回1，不存在返回0

4. expire：给一个已经存在的key设置有效期，有效期到期时该key会被自动删除
   
   * 例：expire home 5
   * 注：时间的单位是秒

5. ttl：查看一个key的剩余有效期
   
   * 例：ttl home

### Redis常用命令--String类型命令

String类型，也就是字符串类型，是redis中最简单的存储类型。其value是字符串，不过根据字符串的格式不同又可以分为3类：

* string：普通字符串
* int：整数类型，可以做自增自减操作
* float：浮点类型，可以做自增自减操作

不管哪种格式，底层都是字节数组形式存储，只不过是编码方式不同。字符串类型的最大空间不能超过512m。

##### 命令：

1. set：添加或者修改已经存在的一个String类型的键值对
2. get：根据key值获取String类型的value
3. mset：批量添加多个String类型的键值对
4. mget：根据多个key获取多个String类型的value
5. incr：让一个指定key的整型的value自增1
6. incrby：让一个指定key的整型的value自增并指定步长，例：incrby num 2
7. incrbyfloat：让一个浮点类型的数字自增并指定步长
8. setnx：添加一个String类型的键值对，前提是这个key不存在，否则不执行
9. setex：添加一个String类型的键值对，并且指定有效期，例：setex name 5 'jack'

### Redis key的结构

redis的key允许有多个单词形成层级结构，多个单词之间用':'隔开，例：
    项目名:业务名:类型:id
如果value是一个java对象，例如user对象，则可以将对象序列化为json字符串后存储：

| key        | value                             |
| ---------- | --------------------------------- |
| ddw:user:1 | {"id":1, "name":"jack", "age":23} |

### Redis常用命令--Hash类型命令

hash类型也称为散列，其value是一个无序字典，类似于java中hashmap结构。
string结构是将对象序列化为json字符串后存储，当需要修改对象某个字段时很不方便。hash结构可以将对象中的每个字段独立存储，可以针对单个字段做crud：

| key        | value | ---   |
| ---------- | ----- | ----- |
| key        | field | value |
| ddw:user:1 | name  | jack  |
| ddw:user:1 | age   | 23    |
| ddw:user:2 | name  | tom   |
| ddw:user:2 | age   | 18    |

##### 常见命令：

1. hset key field value:添加或者修改hash类型key的field的值
2. hget key field：获取一个hash类型key的field的值
3. hmset：批量添加多个hash类型key的field的值
4. hmget：批量获取多个hash类型key的field的值
5. hgetall：获取一个存有hash类型的key中的所有的field和value
6. hkeys：获取一个存有hash类型的key中的所有的field
7. hvals：获取一个存有hash类型的key中的所有的value
8. hincrby：让一个存有hash类型的key的指定字段值value自增并指定步长
9. hsetnx：添加一个hash类型的key的field值，前提是这个field不存在，否则不执行

### Redis常用命令--List类型命令

redis中的List类型与java中的LinkedList类似，可以看作是一个双向链表结构。既可以支持正向检索也支持反向检索。
特征也与LinkList类似：

* 有序
* 元素可以重复
* 插入和删除快
* 查询速度一般

##### 命令：

1. lpush key element... :向列表左侧插入一个或多个元素
2. lpop key：移除并返回列表左侧的第一个元素，没有则返回nil
3. rpush key element... :向列表右侧插入一个或多个元素
4. rpop key：移除并返回列表右侧的第一个元素
5. lrange key start end：返回一段角标范围内的所有元素
6. blpop和brpop：与lpop和rpop类似，只不过在没有元素时会等待指定时间，而不是直接返回nil

### Redis常用命令--Set类型命令

redis的Set类型结构与java中的HashSet类似，可以看作是一个value为null的HashMap。因为也是一个hash表，具备与HashSet类似的特征：

* 无序
* 元素不可重复
* 查找快
* 支持交集、并集、差集等功能

##### 命令：

1. sadd key member... : 向set中添加一个或多个元素
2. srem key member... :移除set中的指定元素
3. scard key：返回set中元素的个数
4. sismember key member：判断一个元素是否存在于set中
5. smembers：获取set中的所有元素
6. sinter key1 key2... : 求key1与key2的交集
7. sdiff key1 key2... : 求key1与key2的差集(属于key1而不属于key2)
8. sunion key1 key2... : 求key1与key2的并集

### Redis常用命令--SortedSet类型命令

> redis的SortedSet是一个可排序的set集合，与java中的treeset有些类似，但底层数据结构却差别很大。SortedSet中的每一个元素都带有一个score属性，可以基于score属性对元素排序，底层的实现是一个跳表(SkipList)加hash表。

> SortedSet具备以下特性:
> 
> * 可排序
> * 元素不重复
> * 查询速度快

##### 命令：

| 命令                                      | 说明                                        |
| --------------------------------------- | ----------------------------------------- |
| zadd key score member [score member...] | 添加一个或多个元素到sortedset，如果已经存在则更新其score值      |
| zrem key member                         | 删除sortedset中的一个指定元素                       |
| zscore key member                       | 获取sortedset中指定元素的score值                   |
| zrank key member                        | 获取sortedset中的指定元素的排名                      |
| zcard key                               | 获取sortedset中的元素个数                         |
| zcount key min max                      | 统计score值在给定范围内的所有元素的个数                    |
| zincrby key incrememt member            | 让sortedset中的指定元素的score自增，步长为指定的increment值 |
| zrange key min max                      | 按照score排序后，获取指定排名范围内的元素                   |
| zrangebyscore key min max               | 按照score排序后，获取指定score范围内的元素                |
| zdiff、zinter、zunion                     | 求差集、交集、并集                                 |

<mark>注意：</mark> 所有排名默认都是升序，如果要降序则在命令的z后面添加rev即可，如：zrange->zrevrange，语法不变仅排序改变

## Redis pipelining

> Redis pipelining是一种通过一次发出多个命令而无需等待对每个单独命令的响应来提高性能的技术。大多数Redis客户机都支持Pipelining

N条命令依次执行

<img title="" src="./pic/redis/屏幕截图 2026-04-19 093957.png" alt="">

N条命令批量执行

<img title="一次发送N条命令" src="./pic/redis/屏幕截图 2026-04-19 093748.png" alt="">



## Redis的Java客户端

1. jedis：学习成本低，简单实用。但是jedis实例是线程不安全的，多线程环境下需要基于连接池使用

2. lettuce(推荐)：lettuce是基于netty实现的，支持同步、异步和响应式编程方式，并且是线程安全的。支持redis的哨兵模式、集群模式和管道模式。

3. spring data redis：实现了一套api可以在jedis和lettuce框架之间无感切换。

4. <mark>引入spring data redis如果没有特殊需要，不需要在配置类中声明，可以直接使用</mark>

### jedis

#### jedis快速入手

[jedis官网地址](https://github.com/redis/jedis)

1. 引入依赖
   
   ```xml
    <dependency>
        <groupId>redis.clients</groupId>
        <artifactId>jedis</artifactId>
        <version>6.0.0</version>
    </dependency>
   ```

2. 建立连接
   
   ```java
   private Jedis jedis;
   @BeforeEach
   void setUp() {
    //建立连接
    jedis = new Jedis("192.168.150.101", 6379);
    //设置密码
    jedis.auth("123456");
    //选择库
    jedis.select(0);
   }
   ```

3. 测试string
   
   ```java
   @Test
   void testString() {
    //插入数据，方法名就是redis命令名称，非常简单
    String result = jedis.set("name", "zhangsan");
    System.out.println("result=" + result);
    //获取数据
    String name = jedis.get("name");
   }
   ```

4. 释放资源
   
   ```java
   @AfterEach
   void tearDown() {
    //释放资源
    if(jedis != null) {
        jedis.close();
        }
   }
   ```

#### jedis连接池

> jedis本身是线程不安全的，并且频繁的创建和销毁连接会有性能损耗，因此推荐使用连接池方式连接。

```java
public class JediConnectionFactory {
 private static final JedisPool jedisPool;
 static {
     JedisPoolConfig jedisPoolConfig  = new JedisPoolConfig();
     //最大连接
     jedisPoolConfig.setMaxTotal(8);
     //最大空闲连接
     jedisPoolConfig.setMaxIdle(8);
     //最小空闲连接
     jedisPoolConfig.setMinIdle(0);
     //设置最长等待时间, ms
     jedisPoolConfig.setMaxWaitMillis(200);
     jedisPool = new JedisPool(jedisPoolConfig, "129.168.150.101", 6379, 1000, "123456");
 }
 //获取Jedis对象
 public static Jedis getJedis() {
     return jedisPool.getResource();
 }
}
```

### Spring Data Redis

SpringData是Spring中数据库操作的模块，包含对各种数据库的集成，其中对Redis的集成模块就叫做SpringDataRedis，SpringData有以下特性：

* 提供了对不同redis客户端的整合(Lettuce和Jedis)
* 提供了RedisTemplate统一API来操作Redis
* 支持Redis的发布订阅模型
* 支持Redis哨兵和Redis集群
* 支持基于Lettuce的响应式编程
* 支持基于JDK、JSON、字符串、Spring对象的数据序列化及反序列化
* 支持基于Redis的JDKCollecion实现

#### SpringDataRedis快速上手

SpringDataRedis中提供了RedisTemplate工具类，其中封装了各种对Redis的操作。并且将不同数据类型的操作api封装到了不同的类型中：

| API                         | 返回值类型           | 说明              |
| --------------------------- | --------------- | --------------- |
| redisTemplate.opsForValue() | ValueOperations | 操作String类型数据    |
| redisTemplate.opsForHash()  | HashOperations  | 操作Hash类型数据      |
| redisTemplate.opsForList()  | ListOperations  | 操作List类型数据      |
| redisTemplate.opsForSet()   | SetOperations   | 操作Set类型数据       |
| redisTemplate.opsForZSet()  | ZSetOperations  | 操作SortedSet类型数据 |
| redisTemplate               |                 | 通用的命令           |

##### 依赖引入

```xml
<!-- Spring Data Redis -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>

<!-- 如果需要使用连接池 -->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-pool2</artifactId>
</dependency>
```

##### 配置文件

```yml
spring:
    redis:
        host: 192.168.150.101
        port: 6379
        password: 123456
        lettuce:
            pool:
                max-active: 8   #最大连接
                max-idle: 8     #最大空闲连接
                min-idle: 0     #最小空闲连接
                max-wait: 100   #连接等待时间
```

##### SpringDataRedis的序列化方式

RedisTemplate可以接收任意Object作为值写入Redis，只不过写入前会把Object序列化，默认是采用JDK序列化。
缺点：

* 可读性差

* 内存占用大
  自定义RedisTemplate的序列化方式，代码如下：
  
  ```java
  @Bean
  public RedisTemplate<String, Object> redisTemplate(RedisConnectionFactory redisConnectionFactory) throws UnKnownHostException {
    //创建Template
    RedisTemplate<String, Object> redisTemplate = new RedisTemplate<>();
    //设置redis的连接工厂对象
    redisTemplate.setConnectionFactory(redisConnectionFactory);
    //设置序列化工具
    GenericJackson2JsonRedisSerializer jsonRedisSerializer = new GenericJackson2JsonRedisSerializer();
    //key和hashkey采用string序列化
    redisTemplate.setKeySerializer(RedisSerializer.string());
    redisTemplate.setHashKeySerializer(RedisSerializer.string());
    //
    redisTemplate.setValueSerializer(jsonRedisSerializer);
    redisTemplate.setHashValueSerializer(jsonRedisSerializer);
    return redisTemplate;
  }
  ```

##### StringRedisTemplate

尽管JSON的序列化方式可以满足需要，但存在一些问题，JSON序列化器会将类的class类型写入json结果中，会带来额外的内存开销。
<mark>为了节省内存空间，一般不会使用JSON序列化器来处理value，而是统一使用String序列化器，这要求只能存储String类型的key和value。当要存储java对象时，要手动完成对象的序列化和反序列化。</mark>
spring默认提供了一个StringRedisTemplate类，它的key和value的序列化方式默认就是String方式。省去了自定义RedisTemplate的过程。

## redis缓存

> 缓存：缓存就是数据交换的缓冲区(称为Cache)，是存储数据的临时地方，一般读写性能较高。

1. 缓存的作用
- 降低后端负载
- 提高读写效率，降低响应时间
2. 缓存的成本
- 数据一致性成本
- 代码维护成本
- 运维成本

### 添加redis缓存

> 思路：服务层在向数据库发起查询时，首先向redis查询，如果redis中有则命中返回结果，未命中向数据库发起请求，得到的结果缓存redis同时返回结果

### 缓存的更新策略

|      | 内存淘汰                                           | 超时剔除                             | 主动更新                  |
| ---- | ---------------------------------------------- | -------------------------------- | --------------------- |
| 说明   | 不用自己维护，利用redis的内存淘汰机制，当内存不足时自动淘汰部分数据，下次查询时更新缓存 | 给缓存数据添加TTL时间，到期后自动删除缓存。下次查询时更新缓存 | 编写业务逻辑，在修改数据库的同时，更新缓存 |
| 一致性  | 差                                              | 一般                               | 好                     |
| 维护成本 | 无                                              | 低                                | 高                     |

#### 选择场景:

- 低一致性需求：使用内存淘汰机制。例如店铺类型的查询缓存
- 高一致性需求：主动更新，并以超时剔除作为兜底方案。例如店铺详情查询的缓存

#### 主动更新策略

1. Cache Aside Pattern   ---- 一般选择这个方法
   由缓存的调用者，在更新数据库的同时更新缓存
2. Read/Write Through Pattern
   缓存与数据库整合为一个服务，由服务来维护一致性。调用者调用该服务，无需关心缓存一致性问题。
3. Write Behind Caching Pattern
   调用者只操作缓存，由其它线程异步的将缓存数据持久化到数据库，保证最终一致。

#### 操作缓存和数据库时需要考虑读写操作 ---- Cache Aside Pattern

读操作：

- 缓存命中则直接返回
- 缓存未命中则查询数据库，并写入缓存，设定超时时间
  写操作：
- 先写数据库，然后再删除缓存
- ==要确保数据库与缓存操作的原子性，保证原子性使用事务==

### 缓存穿透

> 缓存穿透：是指客户端请求的数据在缓存中和数据库中都不存在，这样缓存永远不会生效，这些请求都会打到数据库。

**常见解决方法**：

1. 缓存空对象
   - 优点：实现简单，维护方便
   - 缺点：
     - 额外的内存消耗
     - 可能造成短期的不一致
2. 布隆过滤
   - 优点：内存占用较少，没有多余key
   - 缺点：
     - 实现复杂
     - 存在误判可能

<img src="./pic/redis/屏幕截图 2025-08-21 070446.png">

3. 增强id的复杂度，避免被猜测id规律
4. 做好数据的基础格式校验
5. 加强用户权限校验
6. 做好热点参数的限流

### 缓存雪崩

> 缓存雪崩：是指在同一时间段大量的缓存key同时失效或者redis服务宕机，导致大量请求到达数据库，带来巨大压力。

解决方案：

- 给不同的key的TTL添加随机值，防止key同时失效
- 利用redis集群提高服务的可用性
- 给缓存业务添加降级限流策略
- 给业务添加多级缓存

<img src="./pic/redis/屏幕截图 2025-08-21 074213.png">

### 缓存击穿

> 缓存击穿：也叫热点key问题，就是一个被高并发访问并且缓存重建业务较复杂的key突然失效了，无数的请求访问会瞬间给数据库带来巨大的冲击

<img src="./pic/redis/屏幕截图 2025-08-21 080205.png">

常见的解决方案有两种：

- 互斥锁
- 逻辑过期

<img src="./pic/redis/屏幕截图 2025-08-21 080406.png">

<img src="./pic/redis/屏幕截图 2025-08-21 080703.png">

<img src="./pic/redis/屏幕截图 2025-08-21 125541.png">

### 缓存工具封装 ---- 重要



## 秒杀业务 ---- redis应用

### 全局ID生成器 --- 用redis实现

> 全局ID生成器，是一种在分布式系统下用来生成全局唯一ID的工具

实现策略：

- redis自增
- snowflake算法
- 数据库自增

一般要满足下列特性：

- 唯一性
- 高可用
- 高性能
- 递增性
- 安全性

ID组成实例：
<img src="./pic/redis/屏幕截图 2025-08-22 065517.png">

```java
@Component
public class RedisIdWorker {
    // 初始时间
    private static final Long BEGIN_TIMESTAMP = 1640995200L; // 2022-01-01
    private static final int COUNT_BITS = 32;
    private StringRedisTemplate stringRedisTemplate;

    public RedisIdWorker(StringRedisTemplate stringRedisTemplate) {
        this.stringRedisTemplate = stringRedisTemplate;
    }

    // id前缀
    public long nextId(String keyPrefix) {
        // 生成时间戳
        LocalDateTime now = LocalDateTime.now();
        long nowSencond = now.toEpochSecond(ZoneOffset.UTC);
        // 获取当前时间戳与初始时间的差值
        long timestamp = nowSencond - BEGIN_TIMESTAMP;

        // 生成序列号
        // 获取当前日期，精确到天
        String data = now.format(DateTimeFormatter.ofPattern("yyyy:MM:dd"));
        // 自增长，对"irc:" + keyPrefix + ":" + data对应的value进行自增长
        long count = stringRedisTemplate.opsForValue().increment("irc:" + keyPrefix + ":" + data);

        // 拼接并返回
        return timestamp << COUNT_BITS | count;
    }
}
```

### 秒杀实现 ---- 存在高并发安全问题

实现思路
<img src="./pic/redis/屏幕截图 2025-08-22 095109.png">

代码实现

```java
@Transactional
    public Result secKillVoucher(Long voucherId) {
        // 查询优惠卷
        SeckillVoucher voucher = seckillVoucherService.getById(voucherId);
        // 判断秒杀是否开始
        if (voucher.getBeginTime().isAfter(LocalDateTime.now())) {
            // 尚未开始
            return Result.fail("尚未开始");
        }
        if (voucher.getEndTime().isBefore(LocalDateTime.now())) {
            return Result.fail("秒杀已经结束");
        }
        // 判断是否充足，高并发时这里会出问题，
        if (voucher.getStock() < 1) {
            return Result.fail("库存不足");
        }
        // 扣减库存
        boolean success = seckillVoucherService.update()
                .setSql("stock = stock - 1")
                .eq("voucher_id", voucherId)
                .update();
        // 扣减失败
        if (!success) {
            return Result.fail("库存不足");
        }
        // 创建订单
        VoucherOrder voucherOrder = new VoucherOrder();
        // 订单id
        long orderId = redisIdWorker.nextId("order");
        voucherOrder.setId(orderId);
        // 用户id
        Long userId = UserHolder.getUser().getId();
        voucherOrder.setUserId(userId);
        // 代金券id
        voucherOrder.setVoucherId(voucherId);
        // 保存到数据库
        save(voucherOrder);
        return Result.ok(orderId);
    }
```

> 高并发时在判断库存是否充足会出问题，但库存为1时，一个线程判断充足没来得及进行库存扣减，其它线程这时进行库存判断也能通过，结果导致多个线程对库存为1进行扣减，导致超售。

> ==解决思路：加锁==

### 秒杀问题解决 ---- 加锁

> 悲观锁:认为并发安全问题一定会发生。

> 乐观锁:认为并发安全问题不一定会发生，所以比悲观锁要一步检查是否有其它线程在操作的步骤。

1. 乐观锁解决秒杀高并发问题
   乐观锁的关键是判断之前查询得到的数据是否被修改过，常见的方式有两种：
   
   1. 版本号法
      
      <img src="./pic/redis/屏幕截图 2025-08-22 100528.png">
   
   2. CAS法
      
      <img src="./pic/redis/屏幕截图 2025-08-22 100706.png">

2. 悲观锁 --- 正常加锁

#### 乐观锁解决实现

```java
@Transactional
    public Result secKillVoucher(Long voucherId) {
        // 查询优惠卷
        SeckillVoucher voucher = seckillVoucherService.getById(voucherId);
        // 判断秒杀是否开始
        if (voucher.getBeginTime().isAfter(LocalDateTime.now())) {
            // 尚未开始
            return Result.fail("尚未开始");
        }
        if (voucher.getEndTime().isBefore(LocalDateTime.now())) {
            return Result.fail("秒杀已经结束");
        }
        // 判断是否充足
        if (voucher.getStock() < 1) {
            return Result.fail("库存不足");
        }
        // 扣减库存
        boolean success = seckillVoucherService.update()
                .setSql("stock = stock - 1")
                .eq("voucher_id", voucherId)
                .gt("stock", 0) // 库存必须大于0
                .update();
        // 扣减失败
        if (!success) {
            return Result.fail("库存不足");
        }
        // 创建订单
        VoucherOrder voucherOrder = new VoucherOrder();
        // 订单id
        long orderId = redisIdWorker.nextId("order");
        voucherOrder.setId(orderId);
        // 用户id
        Long userId = UserHolder.getUser().getId();
        voucherOrder.setUserId(userId);
        // 代金券id
        voucherOrder.setVoucherId(voucherId);
        // 保存到数据库
        save(voucherOrder);
        return Result.ok(orderId);
    }
```

#### 一人一单

> 对于优惠卷，每人只能获取一个

> 实现思路：使用用户id查询订单，有则获取，无则获取

```java
public Result secKillVoucher(Long voucherId) {
        // 查询优惠卷
        SeckillVoucher voucher = seckillVoucherService.getById(voucherId);
        // 判断秒杀是否开始
        if (voucher.getBeginTime().isAfter(LocalDateTime.now())) {
            // 尚未开始
            return Result.fail("尚未开始");
        }
        if (voucher.getEndTime().isBefore(LocalDateTime.now())) {
            return Result.fail("秒杀已经结束");
        }
        // 判断是否充足
        if (voucher.getStock() < 1) {
            return Result.fail("库存不足");
        }

        Long userId = UserHolder.getUser().getId();
        // 获取锁
        synchronized (userId.toString().intern()) {
            // 防止事务失效
            IVoucherOrderService proxy = (IVoucherOrderService) AopContext.currentProxy();
            return proxy.getOrderId(voucherId);
        }
    }
    @Transactional
    public Result getOrderId(Long voucherId) {
        // 一人一单
        // 用户id
        Long userId = UserHolder.getUser().getId();
        // 查询订单
        Integer count = query()
                .eq("user_id", userId)
                .eq("voucher_id", voucherId)
                .count();
        if (count > 0) {
            return Result.fail("用户已经购买过了");
        }
        // 扣减库存
        boolean success = seckillVoucherService.update()
                .setSql("stock = stock - 1")
                .eq("voucher_id", voucherId)
                .gt("stock", 0) // 库存必须大于0
                .update();
        // 扣减失败
        if (!success) {
            return Result.fail("库存不足");
        }
        // 创建订单
        VoucherOrder voucherOrder = new VoucherOrder();
        // 订单id
        long orderId = redisIdWorker.nextId("order");
        voucherOrder.setId(orderId);

        voucherOrder.setUserId(userId);
        // 代金券id
        voucherOrder.setVoucherId(voucherId);
        // 保存到数据库
        save(voucherOrder);
        return Result.ok(orderId);
    }
```

## 分布式锁

> 上述一人一单使用`synchronized`实现，对于单体服务是可以的，对于微服务架构或tomcat集群不行，因为服务运行在不同的tomcat上，有不同的jvm，有不止一个锁监视器，所以锁不住。

> 分布式锁：满足分布式系统或集群模式下多进程可见并且互斥的锁

<img src="./pic//redis/屏幕截图 2025-08-22 170239.png">

### 分布式锁的实现

分布式锁的核心是实现多进程之间互斥，而满足这一点的方式有很多，常见有三种：

|     | MySQL           | Redis           | Zookeeper     |
| --- | --------------- | --------------- | ------------- |
| 互斥  | 利用mysql本身的互斥锁机制 | 利用节点唯一性和有序性实现互斥 |               |
| 高可用 | 好               | 好               | 好             |
| 高性能 | 一般              | 好               | 一般            |
| 安全性 | 断开连接，自动释放锁      | 利用锁超时时间，到期释放    | 临时节点，断开连接自动释放 |

### 基于Redis的分布式锁 --- 基于redis的setnx实现

实现分布式锁时需要实现的两个基本方法：

- 获取锁：
  
  - 互斥：确保只能有一个线程获取锁
  
  - 非阻塞：尝试获取锁一次，成功返回true，失败返回false
    
    ```bash
    # 添加锁，nx是互斥，ex是设置超时时间
    set lock thread1 nx ex 10
    ```

- 释放锁：
  
  - 手动释放
  
  - 超时释放：获取锁时添加一个超时时间
    
    ```bash
    # 释放锁，删除即可
    del key
    ```
    
    <img src="./pic/redis/屏幕截图 2025-08-22 174717.png">

#### 基于Redis的分布式锁 --- 简单实现

先声明一个接口：

```java
public interface ILock {
    /**
     * 尝试获取锁
     * 
     * @param timeoutSec 锁持有的超时时间，过期后自动释放
     * @return true代表获取锁成功，false代表获取锁失败
     */
    boolean tryLock(long timeoutSec);

    /**
     * 释放锁
     */
    void unlock();
}
```

实现这个接口：

```java
public class SimpleRedisLock implements ILock {
    // key的名字
    private String name;
    // key的前缀
    private static final String KEY_PREFIX = "lock:";
    private StringRedisTemplate stringRedisTemplate;

    public SimpleRedisLock(String name, StringRedisTemplate stringRedisTemplate) {
        this.name = name;
        this.stringRedisTemplate = stringRedisTemplate;
    }

    @Override
    public boolean tryLock(long timeoutSec) {
        // 获取线程标识
        String threadName = Thread.currentThread().getName();
        // 获取锁，如果设置key成功返回true，反之返回false
        Boolean success = stringRedisTemplate
                .opsForValue()
                .setIfAbsent(KEY_PREFIX + name, threadName, timeoutSec, TimeUnit.SECONDS);
        // 避免包装类自动拆箱有空指针的风险
        return Boolean.TRUE.equals(success);
    }

    @Override
    public void unlock() {  // 不验证锁的持有者，会导致误删其它线程的锁
        // 释放锁
        stringRedisTemplate.delete(KEY_PREFIX + name);
    }
}
```

#### 基于Redis的分布式锁 --- 锁误删问题

> 上面的分布式锁具有超时自动删除能力，如果线程1在执行业务时超过设定的自动删除时长会自动释放锁，这是线程2有机会获取锁并执行任务，线程1执行完业务删除线程2的锁，这时线程3有机会获取锁。。。以此类推导致线程并发执行问题出现。

> 改进锁误删问题实现思路：
> 
> - 获取锁时存入线程标识(标识加上用UUID，不能只用Thread获取线程name，因为不同jvm的线程通过这个方法可能获取到相同的名字)
> - 在释放锁时先获取锁中的线程标识，判断是否与当前线程标识一致
>   - 如果一致释放
>   - 不一致不释放

改进之后的代码：

```java
public class SimpleRedisLock implements ILock {
    // key的名字
    private String name;
    // key的前缀
    private static final String KEY_PREFIX = "lock:";
    // 线程标识前缀
    private static final String THREAD_ID_PREFIX = UUID.randomUUID().toString(true) + "-";
    private StringRedisTemplate stringRedisTemplate;

    public SimpleRedisLock(String name, StringRedisTemplate stringRedisTemplate) {
        this.name = name;
        this.stringRedisTemplate = stringRedisTemplate;
    }

    @Override
    public boolean tryLock(long timeoutSec) {
        // 获取线程标识
        String threadName = THREAD_ID_PREFIX + Thread.currentThread().getName();
        // 获取锁，如果设置key成功返回true，反之返回false
        Boolean success = stringRedisTemplate
                .opsForValue()
                .setIfAbsent(KEY_PREFIX + name, threadName, timeoutSec, TimeUnit.SECONDS);
        // 避免包装类自动拆箱有空指针的风险
        return Boolean.TRUE.equals(success);
    }

    @Override
    public void unlock() {
        // 判断标识是否一致
        String value = stringRedisTemplate.opsForValue().get(KEY_PREFIX + name);
        String threadName = THREAD_ID_PREFIX + Thread.currentThread().getName();
        if (threadName.equals(value)) { // 如果在验证后被阻塞，依旧会导致误删
            // 释放锁
            stringRedisTemplate.delete(KEY_PREFIX + name);
        }

    }
}
```

#### 基于Redis的分布式锁 --- 未进行原子性导致锁误删问题

> 对于上面的改进存在一个问题：就是判断标识是否一致与释放锁不是原子性的，依旧会导致锁误删。例如：在验证标识一致后，线程1被jvm的gc阻塞，导致超时释放锁，线程2获取锁执行任务，这时线程1醒来继续执行就会误删线程2的锁。

```java
if (threadName.equals(value)) { // 如果在验证后被阻塞，依旧会导致误删
    // 释放锁
    stringRedisTemplate.delete(KEY_PREFIX + name);
}
```

> 解决思路：保证上述两步的原子性

#### Redis的Lua脚本

> Redis提供了Lua脚本功能，在一个脚本中编写多条Redis命令，确保多条命令执行时的原子性。

redis提供的调用函数，语法如下:

```bash
redis.call('命令名称', 'key', '其它参数', ...)
```

例如：执行set name jack

```bash
redis.call('set', 'name', 'jack')
```

redis脚本格式如下：

```bash
# 先执行set name jack
redis.call('set', 'name', 'jack')
# 在执行get name
local name = redis.call('get', 'name')
# 返回
return name
```

如果脚本中的key、value不想写死，可以作为参数传递。key类型参数会放入KEYS数组，其它参数会放入ARGV数组，在脚本中可以从KEYS和ARGV数组获取这些参数：

```bash
# 数字1指的是key类型参数个数，Lua中数组下标从1开始
eval "return redis.call('set', KEYS[1], ARGV[1])" 1 name Rose
```

**java中RedisTemplate调用Lua脚本的API如下：**

```java
public <T> T execute(RedisScript<T> script, List<K> keys, Object... args) {
    return scriptExecutor.execute(script, keys, args);
}
```

#### 使用Lua脚本实现redis原子操作 --- 改进分布式锁

lua脚本：

```lua
-- 获取锁中线程标识
local id = redis.call('get', KEYS[1])
-- 比较标识是否一致
if(id == ARGV[1]) then
    return redis.call('del', KEYS[1])
end
return 0
```

java使用lua脚本实现的分布式锁：

```java
public class SimpleRedisLock implements ILock {
    // key的名字
    private String name;
    // key的前缀
    private static final String KEY_PREFIX = "lock:";
    // 线程标识前缀
    private static final String THREAD_ID_PREFIX = UUID.randomUUID().toString(true) + "-";
    private StringRedisTemplate stringRedisTemplate;
    // 引入Lua脚本
    private static final DefaultRedisScript<Long> UNLOCK_SCRIPT;
    static {
        UNLOCK_SCRIPT = new DefaultRedisScript<>();
        UNLOCK_SCRIPT.setLocation(new ClassPathResource("unlock.lua"));
        UNLOCK_SCRIPT.setResultType(Long.class);
    }

    public SimpleRedisLock(String name, StringRedisTemplate stringRedisTemplate) {
        this.name = name;
        this.stringRedisTemplate = stringRedisTemplate;
    }

    @Override
    public boolean tryLock(long timeoutSec) {
        // 获取线程标识
        String threadName = THREAD_ID_PREFIX + Thread.currentThread().getName();
        // 获取锁，如果设置key成功返回true，反之返回false
        Boolean success = stringRedisTemplate
                .opsForValue()
                .setIfAbsent(KEY_PREFIX + name, threadName, timeoutSec, TimeUnit.SECONDS);
        // 避免包装类自动拆箱有空指针的风险
        return Boolean.TRUE.equals(success);
    }

    @Override
    public void unlock() {
        // 通过Lua脚本来实现原子操作
        Long execute = stringRedisTemplate.execute(UNLOCK_SCRIPT, List.of(KEY_PREFIX + name),
                THREAD_ID_PREFIX + Thread.currentThread().getName());
    }
}
```

### 基于Redis的分布式锁优化

基于setnx实现的分布式锁存在下面的问题：

- 不可重入
- 不可重试
- 超时释放
- 主从一致性

#### Redisson

> Redisson是一个在Redis的基础上实现的Java驻内存数据网格。它不仅提供了一系列的分布式的java常用对象，还提供了许多分布式服务，其中就包含了各种分布式锁的实现。

##### Redisson使用

1. 引入依赖
   
   ```xml
   <dependency>
    <groupId>org.redisson</groupId>
    <artifactId>redisson</artifactId>
    <version>3.13.6</version>
   </dependency>
   ```

2. 配置Redisson客户端：
   
   ```java
   @Configuration
   public class RedisConfig {
    @Bean
    public RedissonClient redissonClient() {
        // 配置类
        Config config = new Config();
        // 添加redis地址，这里添加了单点的地址，也可以使用config.useClusterServers()添加集群地址
        config.userSingleServer.setAddress("redis://127.0.0.1:6379").setPassword("1234");
        // 创建客户端
        return Redisson.create(config);
    }
   }
   ```

3. 使用Redisson的分布式锁
   
   ```java
   @Autowired
   private RedissonClient redissonClient;
   
   @Test
    void testRedisson() throws InterruptedException {
        // 获取锁（可重入），指定锁的名称
        RLock lock = redissonClient.getLock("anyLock");
        // 尝试获取锁，参数分别是：获取锁的最大等待时间（期间会重试），锁自动释放时间，时间单位
        boolean isLock = lock.tryLock(1, 10, TimeUnit.SECONDS);
        // 判断释放获取成功
        if(isLock) {
            try {
                // 执行业务
            } finally {
                // 释放锁
                lock.unlock;
            }
        }
    }
   ```

##### Redisson可重入锁原理

> 使用redis的hash结构的value，value中的field字段是获取锁的标识，value中的value是field字段对应锁获取锁的次数

<img src="./pic/redis/屏幕截图 2025-08-23 075045.png">

原子性也是通过lua脚本实现的

**Redisson分布式锁原理 ---- 重要**
<img src="./pic/redis/屏幕截图 2025-08-23 093402.png">

原理：

- 可重入：利用hash结构记录线程id和重入次数
- 可重试：利用信号量和PubSub功能实现等待、唤醒、获取锁失败的重试机制
- 超时续约：利用watchDog，每隔一段时间（releaseTime / 3），重置超时时间

##### Redisson分布式锁主从一致性问题

> 使用Redisson的multiLock可以解决，原理：多个独立的Redis节点，必须在所有节点都获取重入锁，才算获取锁成功。

### 秒杀优化

> 优化思路：就是将比较耗时的“判断库存是否充足”和“扣减库存”

<img src="./pic/redis/屏幕截图 2025-08-23 154458.png">

## 分布式缓存

### Redis持久化 -- RDB演示

> RDB全称redis database backup file（redis数据备份文件），也被叫做redis数据快照。简单来说就是把内存中的所有数据都记录到磁盘中。当redis实例故障重启后，从磁盘读取快照文件，恢复数据。

redis内部有触发rdb的机制，可以在redis.conf文件中找到，格式如下：

```bash
# 900秒内，如果至少有一个1key被修改，则执行bgsave，如果是save "" 则表示禁用rdb
save 900 1
save 300 10
svae 60 10000
```

rdb的其它配置也可以在redis.conf文件中设置：

```bash
# 是否压缩，建议不开启，压缩也会消耗cpu，磁盘的话不值钱
rdbcompression yes
# rdb文件名称
dbfilename dump.rdb
# 文件保存的路径目录
dir ./
```

### Redis持久化 -- RDB的fork原理

> bgsave开始时会fork主进程得到子进程，子进程共享主进程的内存数据。完成fork后读取内存数据并写入RDB文件。
