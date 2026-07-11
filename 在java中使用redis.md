## 在java中操作redis
1. redis的java客户端
   * jedis
   * lettuce
   * spring data redis
   * 
spring data redis是spring的一部分，对redis底层开发包进行了高度封装。
在spring项目中，可以使用spring data redis来简化操作。

2. spring data redis使用方式(这个坐标的redis只适用于springboot)
   操作步骤：
      * 导入spring data redis的maven坐标
        ```xml
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>
        ```
      * 配置redis数据源
        ```yml
        spring:
            redis:
                host: localhost
                port: 6379
                password: 123456
                database: 10 #指定数据库，redis有16个数据库，这个值的范围是0-15，不配置默认为0
        ```
      * 编写配置类，创建RedisTemplate对象
        ```java
        @Configuration
        @Slf4j
        public class RedisConfiguration {
            @Bean
            public RedisTemplate redisTemplate(RedisConnectionFactory redisConnectionFactory) {
                RedisTemplate redisTemplate = new RedisTemplate<>();
                //设置redis的连接工厂对象
                redisTemplate.setConnectionFactory(redisConnectionFactory);
                //设置redis key的序列化器
                redisTemplate.setKeySerializer(new StringRedisSerializer());
                return redisTemplate;
            }
        }
        ```
      * 通过RedisTemplate对象操作redis
        1. 在java中操作redis中string类型的数据(常用命令)
           ```java
           //设置指定key的值
           redisTemplate.opsForValue().set("city","北京");
           //获取指定key的值
           redisTemplate.opsForValue().get("city","北京");
           //设置指定key的值,并将过期时间设置为3秒
           redisTemplate.opsForValue().set("code", "1234", 3, TimeUnit.MINUTES);
           //只有在指定key不存在时才设置key的值
           redisTemplate.opsForValue().setIfAbsent("lock", "2");
           ```
        2. 在java中操作redis中hash类型的数据(常用命令)
           ```java
           //将哈希表中key为100里的field字段name的value值设为tom
           redisTemplate.opsForHash().put("100", "name", "tom");
           //获取存储在key为100里的字段为name的value值
           redisTemplate.opsForHash().get("100", "name");
           //获取所有指定key里的所有field字段
           Set keys = hashOperations.keys("100");
           //获取所有value
           List values = hashOperations.values("100");
           ```
        3. 在java中操作redis中list类型的数据(常用命令)
           ```java
           //将一个或多个值插入到指定key中,从左到右插入
           redisTemplate.opsForList().leftPushAll("key", "a", "b", "c");
           //将一个值插入到指定key中
           redisTemplate.opsForList().leftPush("key", "d");
           //获取指定key中列表指定范围内的值(-1表示获取到列表最后一个值)
           redisTemplate.opsForList().range("key", 0, -1);
           //移除并获取指定key中列表最后一个值
           redisTemplate.opsForList().rightPop("key");
           //获取指定key中列表的长度
           redisTemplate.opsForList().size("key");
           ```
        4. 在java中操作redis中集合set类型的数据(常用命令)
           ```java
           //向指定key中插入一个或多个值
           redisTemplate.opsForSet().add("set1", "a", "b", "c", "d", "e");
           redisTemplate.opsForSet().add("set2", "a", "b", "x", "y", "z");
           //获取指定key中所有值
           Set members = redisTemplate.opsForSet().members("set1");
           //获取指定key中值的数量
           Long size = redisTemplate.opsForSet().size("set1");
           //获取两个指定key对应集合的交集
           Set intersect = redisTemplate.opsForSet().intersect("set1", "set2");
           //获取两个指定key对应集合的并集
           Set union = redisTemplate.opsForSet().union("set1", "set2");
           //删除指定key中集合一个或多个值
           redisTemplate.opsForSet().remove("set1", "a", "b");
           ```
        5. 在java中操作redis中有序集合(sorted set)类型的数据(常用命令)
           ```java
           //向指定key中的集合添加成员(数字为分数)
           redisTemplate.opsForZSet().add("zset", "a", 10);
           //取指定key中集合在索引区间内的成员
           Set zset1 = redisTemplate.opsForZSet().range("zset", 0, -1);
           //对指定key中的指定成员的分数增加指定值
           redisTemplate.opsForZSet().incremenScore("zset", "a", 2);
           //删除一个或多个指定成员
           redisTemplate.opsForZSet().remove("zset", "a");
           ```
        6. 通用命令操作
           ```java
           //获取指定key中的内容
           Set keys = redisTemplate.key("key");
           //指定key是否存在
           Boolean name = redisTemplate.hasKey("name");
           //删除指定的key内容
           redisTemplate.delete("mylist");
           ```