# 注解

## 参数注解

1. `@RequestParam`
   
   - 作用：从URL查询参数（?key=value）中获取值
   
   - 适用场景：GET请求的简单参数绑定
   
   - 特性：
     
     1. 参数名可省略（默认使用方法参数名）：`@RequestParam Long id`
     
     2. 支持默认值：`@RequestParam(defaultValue = "1") int page`
     
     3. 必传控制（默认true）：`@RequestParam(required = false) String name`
        
        ```java
        @GetMapping("/user")
        public String getUser(@RequestParam("id") Long userId) {
        return "User ID: " + userId;
        }
        ```

2. **`@PathVariable`**
   
   - 作用：从URL路径模板中获取值（如RESTful风格）
   
   - 示例：
     
     ```java
     @GetMapping("/user/{id}")
     public String getUser(@PathVariable("id") Long userId) {
         return "User ID: " + userId;
     }
     ```
   
   - 特性：
     
     1. 路径变量名可省略（需与方法参数名一致）：`@PathVariable Long id`
     2. 支持正则表达式：`@GetMapping("/user/{id:\\d+}")`

3. `@RequestBody`
   
   - 作用：
     
     
     
     
     
     

## Lombok

> Lombok是一个Java的编译时注解处理工具，通过简单的注解帮助自动生成繁杂的重复代码，Lombok 不是在运行期用反射（那样性能差），而是在**编译期**直接修改Java的抽象语法树。

### 基础注解

1. **`@Getter` / `@Setter`**
   
   - 作用：生成 getter/setter 方法，可用在类或字段上
   
   - 示例：
     
     ```java
     public class User {
         @Getter
         @Setter
         private String name;
     }
     ```

2. **`@ToString`**
   
   - 作用：生成 toString 方法，在类上使用
   
   - 示例：
     
     ```java
     @ToString(exclude = "password") // 设置排除字段
     public class User {
        private String name;
     }
     ```

3. **`@EqualsAndHashCode`**
   
   - 作用：生成equals和hashCode方法，类上使用
   
   - 示例：
     
     ```java
     @EqualsAndHashCode(callSuper = true)  // 可调用父类
     public class User {
      private String name;
     }
     ```
   
   ### 组合注解

4. **`@Data` 组合注解**
   
   - 作用：组合注解，@Data = @Getter + @Setter + @ToString + @EqualsAndHashCode + @RequiredArgsConstructor
   
   - 示例：
     
     ```java
     @Data  // 一个顶五个
     public class User {
        private Long id;
        private String name;
     }
     ```
     
     ### 构造方法注解

5. **`@NoArgsConstructor`**
   
   - 作用：生成无参构造方法，在类上使用
   - 示例：在最下面，统一展示

6. **`@AllArgsConstructor`**
   
   - 作用：生成全参构造方法，所有字段
   - 示例：在最下面，统一展示

7. **`@RequiredArgsConstructor`**
   
   - 作用：生成必需参数构造方法，`final`或`@NonNull`字段
   
   - 示例：在最下面，统一展示
     
     ```java
     @NoArgsConstructor
     @AllArgsConstructor
     @RequiredArgsConstructor
     public class User {
     private final Long id;        // 会被 @RequiredArgsConstructor 包含
     @NonNull private String name;  // 会被 @RequiredArgsConstructor 包含
     private Integer age;           // 只被 @AllArgsConstructor 包含
     }
     ```
   
   ### 日志注解

8. **`@Slf4j`**
   
   - 作用：生成日志对象
   
   - 示例：
     
     ```java
     @RestController
     @Slf4j  // 自动生成: private static final org.slf4j.Logger log = ...;
     public class UserController {
     
      @GetMapping("/test")
      public String test() {
          log.info("请求进来了");  // 直接使用 log 对象
          log.debug("调试信息");
          log.error("错误信息");
          return "ok";
      }
     }
     ```
   
   ### 建造者模式

9. **`@Builder`**
   
   - 作用：生成建造者模式代码，类上使用
   
   - 示例：
     
     ```java
     @Data
     @Builder
     @NoArgsConstructor
     @AllArgsConstructor
     public class CreateUserRequest {
         private String username;
         private String email;
         private Integer age;
         private String phone;
     }
     
     // 使用 Builder 模式创建对象
     CreateUserRequest request = CreateUserRequest.builder()
         .username("张三")
         .email("zhangsan@example.com")
         .age(25)
         .phone("13800138000")
         .build();
     ```
   
   - 常用配置：
     
     ```java
     @Builder(builderMethodName = "hiddenBuilder")  // 修改 builder 方法名
     @Builder(toBuilder = true)                     // 生成 toBuilder() 方法，可从现有对象创建 builder
     ```

## Swagger

### 接口层注解（描述Conttoller和方法）

1. **`@Api`**
   
   - 作用位置：类（Controller）
   - 核心作用：标记一个类为 Swagger 资源，相当于给整个 Controller 做个“名片”

2. **`@ApiOperation`**
   
   - 作用位置：方法 (Handler)
   - 核心作用：描述一个具体的接口操作，包括它的用途、注意事项等

3. **`@ApiResponses`和`@ApiResponse`**
   
   - 作用位置：方法 (Handler)
   - 核心作用：描述接口的各种响应结果，特别是错误情况，让调用者知道会返回什么

### 参数层注解（描述请求参数）

1. **`@ApiImplicitParams`和`@ApiImplicitParam`**
   
   - 作用位置：方法 (Handler)
   - 核心作用：描述一组请求参数。特别注意：这个注解主要用于描述无法通过 @ApiParam 直接标注的参数（比如一个 GET 方法中有多个 @RequestParam，或者参数是隐含的）

2. **`@ApiParam`**
   
   - 作用位置：方法参数上
   - 核心作用：直接标注在方法的参数上，比 @ApiImplicitParam 更直观，和代码结合得更紧密

### 代码演示

```java
// controller层演示

@RestController
@RequestMapping("/users")
@Api(tags = "用户管理接口")  // 给这个类分组
public class UserController {

    @GetMapping("/{id}")
    // 描述一个具体的接口操作
    // value: 接口的简要描述
    // notes: 详细的备注信息
    @ApiOperation(value = "根据ID查询用户", notes = "通过用户ID获取用户的详细信息")
    // 描述接口的各种响应结果
    // @ApiResponse(code = Http状态码, message = "描述信息", response = 返回数据类型)
    /* @ApiResponses({
    *      @ApiResponse(code = 200, message = "成功"),
    *      @ApiResponse(code = 404, message = "资源不存在")
    *   })
    */
    @ApiResponses({
        @ApiResponse(code = 200, message = "查询成功", response = User.class),
        @ApiResponse(code = 404, message = "用户不存在")
    })
    public User getUserById(
            // name: 参数名。
            // value: 参数说明。
            // required: 是否必填
            @ApiParam(value = "用户ID", required = true) 
            @PathVariable Long id) {
        // 业务逻辑...
        return new User();
    }

    @GetMapping("/list")
    @ApiOperation("分页查询用户列表")
    @ApiImplicitParams({
        @ApiImplicitParam(name = "page", value = "当前页码", defaultValue = "1", dataType = "int", paramType = "query"),
        @ApiImplicitParam(name = "size", value = "每页条数", defaultValue = "10", dataType = "int", paramType = "query")
    })
    public List<User> getUserList(
            @RequestParam(required = false) Integer page,
            @RequestParam(required = false) Integer size) {
        // 业务逻辑...
        return null;
    }
}
```

## bean相关注解

> 此类注解的功能是将类注册到IOC容器中，已经围绕此功能的其它一些能力
> 
> 这里仅包含部分注解，其余注解等遇到再记录

### 声明Bean的注解

#### @Component 注解

> 通用的注解，所有类都可以用，将普通类注册为Spring Bean

```java
@Component
public class MyComponent {
    // ...
}
```

#### @Service 注解

> 标识业务逻辑层组件，内部包含@Component注解

```java
@Service 
public class OrderService {
    // ...
}
```

#### @RestController 注解

> `@Controller` + `@ResponseBody`，用于REST接口

```java
@RestController 
public class ApiController {
    // ...
}
```

#### @Configuration 注解

> 标识配置类，内部可定义`@Bean`方法，其作用是在一个类中注册多个bean，虽然在@Component中也可以，但是推荐@Configuration，该注解包含@Component注解

```java
@Configuration 
public class AppConfig {
    // ...
}
```

### 配置Bean的注解

#### @Bean 注解

> 在配置类中声明一个Bean

```java
@Configuration 
public class MyConfig {
    // 该方法返回的对象会交注册到IOC容器中
    @Bean 
    public DataSource dataSource() { 
        return new HikariDataSource(); 
    }
}
```

#### @Lazy 注解

> 正常情况下，注册到IOC的类会在spring启动时实例化，但是使用@Lazy注解会延迟初始化，在首次使用时才创建。

```java
@Configuration
@Lazy(false)    // 不延迟初始化
public class MyConfig {
    // 该方法返回的对象会交注册到IOC容器中
    @Bean 
    @Lazy    // 延迟初始化
    public DataSource dataSource() { 
        return new HikariDataSource(); 
    }
}
```

#### @Primary 注解

> 存在多个同类型Bean时使用该注解修饰的bean优先注入。

```java
@Configuration
public class MyConfig {
    // 该方法返回的对象会交注册到IOC容器中
    @Bean 
    @Primary    // 优先注入
    public DataSource dataSource() { 
        return new HikariDataSource(); 
    }
}
```

### 依赖注入注解

#### @Autowired 注解

> 按类型自动注入依赖

```java
@Autowired 
private UserService userService;
```

### 条件化注册注解

#### @Conditional 注解

> 满足条件才注册Bean

```java

@Configuration 
public class MyConfig {
    // 该方法返回的对象会交注册到IOC容器中
    @Bean 
    @Conditional(MyCondition.class)     // 当依赖中有MyCondition才注册Bean
    public DataSource dataSource() { 
        return new HikariDataSource(); 
    }
}
```

#### @ConditionalOnClass 注解

> classpath存在指定类时注册

```java
@Configuration 
public class MyConfig {
    // 该方法返回的对象会交注册到IOC容器中
    @Bean 
    @ConditionalOnClass(MyCondition.class)     // 当classpath存在MyCondition才注册Bean
    public DataSource dataSource() { 
        return new HikariDataSource(); 
    }
}
```

### 生命周期回调注解

#### @PostConstruct 注解

> Bean初始化完成后执行

```java
@Componet 
public class MyComponet {
    // 当该类注册到IOC容器中时，执行该方法
    @PostConstruct
    public void init() {
        System.out.println("初始化");
    }
}
```

#### @PreDestroy 注解

> Bean销毁前执行

```java
@Componet 
public class MyComponet {
    // Bean销毁前执行执行该方法
    @PreDestroy
    public void init() {
        System.out.println("清理资源");
    }
}
```







# ThreadLocal

* springboot中每一个线程都贯穿所有层(controller,service,mapper)，每一个线程为一个用户工作一段时间后就会为另一个用户工作一段时间，可以把线程理解为一个请求执行完的全过程。
* ThreadLocal并不是一个Thread，而是Thread的局部变量。
  ThreadLocal为每一个线程提供单独一份存储空间，具有线程隔离的效果，只有在线程内才能获取到对应的值，线程外则不能访问。

==注意：线程切换时，该线程所携带的局部变量可以被其它人访问到 ？？==

## ThreadLocal常用方法

* `public void set(T value)`设置当前线程的局部变量的值
* `public T get()`返回当前线程所对应的线程局部变量的值
* `public void remove()`移除当前的线程局部变量

# spring配置类

# springboot的classpath

1. Spring Boot 的 Classpath 组成
   Spring Boot 的 Classpath 主要包括以下位置（按优先级从高到低排序）：
- 项目根目录下的 /config 文件夹（外部化配置，最高优先级）

- 项目根目录（即 JAR/WAR 所在目录）

- Classpath 中的 /config 包（如 src/main/resources/config/）

- Classpath 根目录（如 src/main/resources/）

- 依赖库中的 META-INF/resources/（如静态资源）

注：Spring Boot 会按这个顺序查找文件（如 application.properties），先找到的配置会覆盖后面的。



# springboot使用

> 这里简单介绍springboot在单体项目中的使用

## Spring IOC & Spring DI

> Spring IOC：控制反转，对象的创建的控制权由程序自身转移到外部（容器），这种思想就是控制反转。
> 
> Spring DI：依赖注入，容器为应用程序提供运行时所依赖的资源，称之为依赖注入。
> 
> Bean对象：IOC容器创建、管理的对象。

### IOC注解

> 要把某个对象交由IOC容器管理，需要在对应的类上加上如下注解：

| 注解         | 说明            | 位置            |
| ---------- | ------------- | ------------- |
| @Component | 声明bean对象的基础注解 | 在要交给容器管理的类上添加 |
|            |               |               |
|            |               |               |
|            |               |               |


