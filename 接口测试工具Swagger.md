## 介绍
Swagger只需要按照它的规范去定义接口及接口相关的信息，就可以做到生成接口文档，以及在线接口调试页面。
[Knife4j]()是为Java MVC框架集成Swagger生成Api文档的增强解决方案。
1. maven坐标
```xml
<denpendency>

</denpendency>
```
2. 使用方法
   * 导入knife4j的maven坐标
   * 在配置类中加入knife4j相关配置
     ```java
     //在配置类WebMvcConfiguration
     @Bean
     public Docket docket() {
        ApiInfo apiInfo = new ApiInfoBuilder()
            .title("苍穹外卖项目接口文档")
            .version("2.0")
            .description("苍穹外卖项目接口文档")
            .build();
        Docket docket = new Docket(DocumentationType.SWAGGER_2)
            .apiInfo(apiInfo)
            .select()
            //指定生成接口需要扫描的包
            .apis(RequestHandlerSelectors.basePackage("com.sky.controller"))
            .paths(PathSelectors.any())
            .build();
        return docket;
     }
     ```
   * 设置静态资源映射，否则接口文档页面无法访问
    ```java
    //在配置类WebMvcConfiguration
    //设置静态资源映射
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        log.info("开始设置静态资源映射...");
        registry.addResourceHandler("/doc.html").addResourceLocations("classpath:/META-INF/resources/");
        registry.addResourceHandler("/webjars").addResourceLocations("classpath:/META-INF/resources/webjars/");
    }
    ```
3. 常用注解
   * 通过注解可以控制生成的接口文档，使接口文档拥有更好的可读性，常用注解如下：

    | 注解 | 说明 |
    | ---- | ---- |
    | @Api | 用在类上，例如Controller，表示对类的说明 |
    | @ApiModel | 用在类上，例如：entity、DTO、VO |
    |@ApiModelProperty|用在属性上，描述属性信息|
    |@ApiOperation|用在方法上，例如Controller的方法，说明方法的用途|

