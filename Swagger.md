# Swagger

## 概念



## 操作步骤

1. 引入依赖

```xml
 <!-- swagger文档 -->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.9.2</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.9.2</version>
        </dependency>
```

2. 创建Swagger2的配置类

```java
@EnableSwagger2
@Configuration
public class Swagger2Config {

    @Value("${enable.swagger2}")
    private boolean enableSwagger;

    /**
     *
     * @return 返回swagger文档类型的数据
     */
    @Bean
    public Docket swagger() {
        return new Docket(DocumentationType.SWAGGER_2)//文档的版本号
                .enable(enableSwagger)//
                .apiInfo(apiInfo())//集成必要的api信息
                .select()//供查询使用
                .apis(RequestHandlerSelectors.basePackage("com.yxg.controller"))//接口所在包的完整目录
                .paths(PathSelectors.any())//将所有接口的请求路径暴露给swagger
                .build();
    }

    /**
     *
     * @return 返回swagger-ui界面的基本信息
     */
    private ApiInfo apiInfo() {
        return new ApiInfoBuilder()
                .title("我的文档")//界面的大标题
                .version("v1.0")//所有接口的版本
                .description("我的测试文档")//界面的描述信息
                .build();
    }
}
```

3. 启动项目，浏览器访问(ip地址:端口号/swagger-ui.html)

![image-20220114114459443](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201141145840.png)



![image-20220114114743780](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201141147093.png)

![image-20220114114831120](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201141148324.png)

