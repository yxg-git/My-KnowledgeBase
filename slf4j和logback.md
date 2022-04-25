# slf4j和logback

[文章链接1](https://cloud.tencent.com/developer/article/1698344)

[文章链接2](https://juejin.cn/post/6844903790349385735)

## 概念



## 配置及其使用



1. 引入slf4j和logbook的相关依赖

```xml
<!--slf4j-->
<dependency>
    <groupId>org.slf4j</groupId>
    <artifactId>slf4j-api</artifactId>
</dependency>
<!--logback-->
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-core</artifactId>
</dependency>
<dependency>
    <groupId>ch.qos.logback</groupId>
    <artifactId>logback-classic</artifactId>
</dependency>
```



2. resource目录下创建一个logback.xml文件

```xml
<?xml version='1.0' encoding='UTF-8'?>
<!--日志配置-->
<configuration>
    <!--直接定义属性-->
    <property name="logFile" value="logs/logback"/>
    <property name="maxFileSize" value="30MB"/>

    <!--控制台日志-->
    <appender name="STDOUT" class="ch.qos.logback.core.ConsoleAppender">
        <encoder>
            <pattern>%d [%thread] %-5level %logger{50} -[%file:%line]- %msg%n</pattern>
            <charset>UTF-8</charset>
        </encoder>
    </appender>

	<!--滚动文件日志-->
    <appender name="fileLog" class="ch.qos.logback.core.rolling.RollingFileAppender">
        <file>${logFile}.log</file>
        <encoder>
        	<!--日志输出格式-->
            <pattern>%d [%thread] %-5level -[%file:%line]- %msg%n</pattern>
            <charset>UTF-8</charset>
        </encoder>
        <rollingPolicy class="ch.qos.logback.core.rolling.SizeAndTimeBasedRollingPolicy">
            <fileNamePattern>${logFile}.%d{yyyy-MM-dd}.%i.log</fileNamePattern>
            <maxFileSize>${maxFileSize}</maxFileSize>
        </rollingPolicy>
    </appender>
    <!--创建一个具体的日志输出-->
    <logger name="com.yxg" level="info" additivity="true">
    	<!--可以有多个appender-ref，即将日志记录到不同的位置-->
        <appender-ref ref="STDOUT"/>
        <appender-ref ref="fileLog"/>
    </logger>

    <!--基础的日志输出-->
    <root level="info">
    </root>
</configuration>
```



3. 在application.yml文件中配置项目要使用的日志配置文件路径

```yaml
logging:
  config: classpath:logback.xml
```



4. 在接口中添加日志

```java
@RestController
@Slf4j//需要配合lomback插件使用
public class UserController {

    //private final Logger logger = LoggerFactory.getLogger(UserController.class);

    @Autowired
    private UserSercice userSercice;

    @GetMapping("/user/{id}")
    public User findById(@PathVariable("id") int id) {
        log.info("logTest,id:{}",id);
        return userSercice.findById(id);
    }
}
```



5. 启动项目，浏览器访问

- 控制台打印日志

![image-20220114143922084](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201141439189.png)



- 在项目中生成一个日志文件

![image-20220114144112852](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201141441075.png)