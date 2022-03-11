# Java开发工程师学习指南

> **一、熟悉阿里巴巴Java编码规范 (后面要组织考试) ，当中不理解的可以自己网络上搜一下搞明白。（用时1Day）**

> **二、基于JDK1.8，用SpringBoot 2.0搭建一个基于Maven的Web项目，集成SpringMVC，可以启动起来，通过浏览器可以发送请求，网页显示返回内容。 （用时1Day）**

1. 创建maven项目
2. 导入SprinBoot的起步依赖

```xml
 <!--springboot工程需要继承的父工程-->
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.6.0-SNAPSHOT</version>
</parent>

<dependencies>
    <dependency>
        <!--web起步依赖-->
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```



3. 定义Service层接口

```java
public interface HelloService {
    String hello();
}

```



4. 定义Service实现类

```java
@Service
public class HelloServiceImpl implements HelloService {
    @Override
    public String hello() {
        return "hello world";
    }
}
```



4. 定义Controller层

```java
@RestController
public class HelloController {

    @Autowired
    private HelloService helloService;

    @RequestMapping("/hello")
    public String hello() {
        return helloService.hello();
    }
}
```



5. 编写引导类

```java
@SpringBootApplication
public class HelloApplication {
    public static void main(String[] args) {
        SpringApplication.run(HelloApplication.class,args);
    }
}

```



6. 启动，浏览器访问

![image-20220113145155604](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201131452798.png)

> **三、在SpringBoot的Web项目中，集成MySql访问（MySql可以在本地安装,5.7版本以上），集成Mybatis。 （用时1Day)**

1. 导入MyBatis起步依赖和MySQL驱动

```xml
<dependency>
   <groupId>mysql</groupId>
   <artifactId>mysql-connector-java</artifactId>
</dependency>
<dependency>
   <groupId>org.mybatis.spring.boot</groupId>
   <artifactId>mybatis-spring-boot-starter</artifactId>
   <version>2.2.0</version>
</dependency>
```



2. 创建`test`数据库和`user`表

![image-20220113154700452](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201131547585.png)



3. 编写配置文件

```yaml
spring:
  application:
    name: hello
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://123.56.220.247:33067/test?useUnicode=true&useSSL=false&characterEncoding=utf8&serverTimezone=Asia/Shanghai
    username: root
    password: root

mybatis:
  mapper-locations: classpath:com.yxg.mapper/*Mapper.xml
```



4. 定义实体类

```java
@Data
@AllArgsConstructor
@NoArgsConstructor
public class User {
    private int id;
    private String name;
    private int age;
}
```



5. 编写dao层接口

```java
@Mapper
public interface UserDao {

    /**
     * 根据用户名查找
     * @param id
     * @return
     */
    User findById(int id);
}
```



6. 编写mapper文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.yxg.mapper.UserMapper">
    <select id="findById" parameterType="int" resultType="com.yxg.pojo.User">
        select * from t_user where id = #{id}
    </select>
</mapper>
```



7. 编写service层接口以及实现类

```java
/**service接口**/
public interface UserSercice {
    User findById(int id);
}

/**service实现类**/
@Service
public class UserServiceImpl implements UserSercice {
    @Autowired
    private UserMapper userMapper;

    @Override
    public User findById(int id) {
        return userMapper.findById(id);
    }
}
```



8. 编写controller层

```java
@RestController
public class UserController {

    @Autowired
    private UserSercice userSercice;

    @RequestMapping("/user/{id}")
    public User findById(@PathVariable("id") int id) {
        User user = userSercice.findById(id);
        System.out.println(user);
        return user;
    }
}
```



9. 浏览器访问

![image-20220113161653415](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201131616511.png)

> **四、在SpringBoot的Web项目中，集成Swagger，通过Swagger可以实现对一个表的CURD操作 。 （用时1Day）**

1. 引入Swagger文档和Swagger-UI的依赖

```xml
<!-- swagger文档 -->
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
    <version>2.9.2</version>
</dependency>
<!-- swagger-ui -->
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

![image-20220114114459443](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201141152492.png)



![image-20220114114743780](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201141152328.png)

![image-20220114114831120](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201141152566.png)



> **五、熟悉Git的使用。了解概念并操作。比如仓库创建，分支的创建、提交、删除，提交、推送、回滚等。（用时1Day）**

> **六、把上面的项目提交到gitee或者github上的个人仓库，创建不同分支，在另一个分支上尝试集成logback，在crud代码中使用slf4j写日志，通过配置logback把日志能输出到控制台，也可以记录到本次磁盘。（用时1Day）**

1. 通过Sourcetree将项目创建为git项目，同时在GitHub中创建Test仓库

![image-20220114120534566](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201141205801.png)



2. 将项目提交到仓库并且推送到GitHub中

![image-20220114121559561](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201141215850.png)



![image-20220114121535492](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201141216079.png)

3. 使用idea创建`dev`分支

![image-20220114140628479](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201141406831.png)



4. 引入slf4j和logbook的相关依赖

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



5. resource目录下创建一个logback.xml文件

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



6. 在application.yml文件中配置项目要使用的日志配置文件路径

```yaml
logging:
  config: classpath:logback.xml
```



7. 在接口中添加日志

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



8. 启动项目，浏览器访问

- 控制台打印日志

![image-20220114143922084](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201141447801.png)



- 在项目中生成一个日志文件

![image-20220114144112852](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201141447728.png)

> **七、本地安装redis，程序集成redis，熟悉redis数据结构，在SpringBoot项目中，自己设计场景，熟悉其中的使用。（用时 3Day）**

1. 引入redis的起步依赖

```xml
<!--redis-->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<!--spring2.0集成redis所需common-pool2-->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-pool2</artifactId>
    <version>2.10.0</version>
</dependency>
```



2. 配置redis的相关属性

```yaml
spring:
  redis:
    database: 0
    host: 123.56.220.247
    port: 9736
    password: wsyxg19980720
    jedis:
      pool:
        #最大连接数
        max-active: 8
        #最大阻塞等待时间
        max-wait: -1
        #最大空闲
        max-idle: 8
        #最小空闲
        min-idle: 0
        #超时时间
        timeout: 10000
```



3. 新建redis的配置类

```java
@Configuration
public class RedisConfig {

    /**
     *  默认情况下的模板只能支持 RedisTemplate<String,String>，
     *  只能存入字符串，很多时候，需要自定义 RedisTemplate ，设置序列化器
     */
    @Bean
    public RedisTemplate<String, Serializable> redisTemplate(LettuceConnectionFactory connectionFactory) {
        RedisTemplate<String, Serializable> redisTemplate = new RedisTemplate<>();
        redisTemplate.setKeySerializer(new StringRedisSerializer());
        redisTemplate.setValueSerializer(new GenericJackson2JsonRedisSerializer());
        redisTemplate.setConnectionFactory(connectionFactory);
        return redisTemplate;
    }
}
```



3. 编写测试方法测试存取数据

```java
@SpringBootTest
@RunWith(SpringRunner.class)
@Component
public class RedisTest {

    @Autowired
    private RedisTemplate redisTemplate;

    /**
     * 存取数据
     */
    @Test
    public void testSet(){
        redisTemplate.opsForValue().set("test","testValue");
    }

    /**
     * 获取数据
     */
    @Test
    public void testGet() {
        Object value = redisTemplate.opsForValue().get("test");
        System.out.println(value);
    }
}
```



- 已存入redis数据库中

![image-20220116153157862](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201161531995.png)

- 获取数据

![image-20220116151205485](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201161512658.png)





> **八、本地安装RabbitMQ，熟悉RabbitMQ的相关概念，在SpringBoot项目中，自己设计场景，熟悉其中的使用。（用时 3Day）**

1. 添加rabbitmq的起步依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-amqp</artifactId>
</dependency>
```



2. 添加配置

```yaml
spring:
  rabbitmq:
    host: 123.56.220.247
    port: 5672
    virtual-host: /myVM
    username: guest
    password: guest
```



3. 创建配置类，绑定队列和交换机

```java
@Configuration
public class RabbitmqConfig {

    /**
     * 交换机名称
     */
    public static final String EXCHANGE_NAME = "test_topic_exchange";

    /**
     * 队列名称
     */
    public static final String QUEUE_NAME = "test_queue";

    /**
     * 声明交换机
     */
    @Bean("testTopicExchange")
    public Exchange testTopicExchange() {
        return ExchangeBuilder.topicExchange(EXCHANGE_NAME).durable(true).build();
    }

    /**
     * 声明队列
     */
    @Bean("testQueue")
    public Queue testQueue() {
        return QueueBuilder.durable(QUEUE_NAME).build();
    }

    /**
     * 绑定队列和交换机
     */
    @Bean
    public Binding bindQueueExchange(@Qualifier("testQueue") Queue queue, @Qualifier("testTopicExchange") Exchange exchange) {
        return BindingBuilder.bind(queue).to(exchange).with("test.#").noargs();
    }
}
```



4. 编写生产者测试类

```java
@SpringBootTest
@RunWith(SpringRunner.class)
@Component
public class RabbitmqTest {

    /**
     * 注入rabbitTemplate
     */
    @Autowired
    private RabbitTemplate rabbitTemplate;

    /**
     * 生产者发送消息
     */
    @Test
    public void producer() {
        rabbitTemplate.convertAndSend(RabbitmqConfig.EXCHANGE_NAME, "test.rabbitmq", "测试springboot整合rabbitmq：测试1");
        rabbitTemplate.convertAndSend(RabbitmqConfig.EXCHANGE_NAME, "test.springboot", "测试springboot整合rabbitmq：测试2");
        rabbitTemplate.convertAndSend(RabbitmqConfig.EXCHANGE_NAME, "springboot.test", "测试springboot整合rabbitmq：测试3");
    }
}
```

- 已发送到队列中

![image-20220116161458341](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201161614508.png)



5. 编写消费者监听类接受消息

```java
@Component
public class ConsumerListener {
    /**
     * 监听某个队列的消息
     */
    @RabbitListener(queues = RabbitmqConfig.QUEUE_NAME)
    public void listen(Message message){
        System.out.println("消费者接受到的消息为：" + new String(message.getBody()));
    }
}
```

- 消费者接受到消息

![image-20220116162003117](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201161620197.png)



> **九、本地安装Kafka，熟悉Kafka的相关概念，在SpringBoot项目中，自己设计场景，熟悉其中的使用。（用时 3Day）**

1. 引入Kafka依赖

```xml
<dependency>
    <groupId>org.springframework.kafka</groupId>
    <artifactId>spring-kafka</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.kafka</groupId>
    <artifactId>spring-kafka-test</artifactId>
</dependency>
```



2. 定义配置文件

```yaml
spring:
  kafka:
    #生产者
    producer:
      #发送失败重试次数
      retries: 0
      #生产者批次投递大小  16384(16k)默认
      batch-size: 16384
      #生产缓冲区大小 默认值 (32M)
      buffer-memory: 33554432
      #配置序列化
      key-serializer: org.apache.kafka.common.serialization.StringSerializer
      value-serializer: org.apache.kafka.common.serialization.StringSerializer
    #kafka代理地址
    bootstrap-servers: 101.35.158.207:9092
    #消费者
    consumer:
      #组iD
      group-id: kafka-spring
      #反序列化
      key-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      value-deserializer: org.apache.kafka.common.serialization.StringDeserializer
      #配置手动提交
      enable-auto-commit: false
    listener:
      ack-mode: manual

```



3. 编写生产者服务类和测试类

```java
//生产者服务类
@Service
public class KafkaProviderServer {

    @Autowired
    private KafkaTemplate kafkaTemplate;
    public void sendMsg() {
        for (int i = 1; i <= 50; i++) {
            kafkaTemplate.send("kafka-springboot","kafka-test", "hello springboot~kafka");
        }
    }
}
//生产者测试类
@RunWith(SpringRunner.class)
@SpringBootTest(classes = KafkaProviderServer.class)
public class KafkaProviderTest {

    @Autowired
    private KafkaProviderServer providerServer;
    @Test
    public void sendTest(){
        providerServer.sendMsg();
    }
}
```



4. 消费者

```java
@Service
@Slf4j
public class KafkaCustomerServer {
    @KafkaListener(topics = "kafka-springboot")
    public void listen(ConsumerRecord record, Acknowledgment acknowledgment){
        log.info("消息的key："+record.key());
        log.info("收到的消息为：" + record.value().toString());
        acknowledgment.acknowledge();
    }
}
```

> **十、本地搭建Vue环境，用ElementUI开发和上面web项目对应的增删改查界面。 （用时3Day）**

1. 安装vue-cli

```shell
npm install -g vue-cli
```

![image-20220119145607487](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201191456678.png)



2. 搭建前端环境

```shell
vue init webpack vue-test
cd vue-test
npm install
npm run dev
```

![image-20220119150213961](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201191502103.png)

![image-20220119150454612](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201191504953.png)



- 浏览器输入`http://localhost:8080`访问

![image-20220119150621720](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201191506078.png)



2. 使用`vscode`打开项目

![image-20220119151055217](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201191510311.png)



3. 在项目根目录安装axios和element ui

```shell
npm install axios
npm i element-ui -S
```

4. 跨域配置

```vue
dev: {
    // Paths
    assetsSubDirectory: 'static',
    assetsPublicPath: '/',
    proxyTable: {
      '/': {
        target:'http://localhost:8081',
        changeOrigin:true, 
        pathRewrite:{  
          '^/': ''  
        }
      }
    },
}
```

5. 编写界面

```vue
<template>
  <div>
      <el-form :inline="true" class="demo-form-inline">
          <el-form-item>
            <el-input
              v-model="search"
              class="search_name"
              size="mini"
              placeholder="输入姓名查询">
            </el-input>
          </el-form-item>
          <el-form-item>
            <el-button
              type="text"
              @click="onSearch()"
              class="el-icon-search">查询
            </el-button>
          </el-form-item>
          <el-form-item>
            <el-button
              class="el-icon-circle-plus-outline"
              type="text"
              @click="dialogVisible = true">添加
            </el-button>
          </el-form-item>
      </el-form>
      <el-table
        :data="tableData"
        highlight-current-row
        border
        style="width: 100%">
        <el-table-column
          label="编号">
          <template slot-scope="scope">
            <span>{{ scope.row.id }}</span>
          </template>
        </el-table-column>
        
        <el-table-column
          label="姓名">
          <template slot-scope="scope">
            <el-popover trigger="hover" placement="right">
              <p>编号: {{ scope.row.id }}</p>
              <p>姓名: {{ scope.row.name}}</p>
              <p>年龄：{{ scope.row.age }}</p>
              <div slot="reference" class="name-wrapper">
                <el-button type="text">{{ scope.row.name }}</el-button>
              </div>
            </el-popover>
          </template>
        </el-table-column>
        <el-table-column
          label="年龄">
          <template slot-scope="scope">
            <span>{{ scope.row.age }}</span>
          </template>
        </el-table-column>
        <el-table-column
          label="操作"
          fixed="right"
          width="200">
          <template slot-scope="scope">
            <el-button
              size="mini"
              icon="el-icon-edit"
              @click="handleEdit(scope.$index, scope.row)">编辑
            </el-button>
            <el-button
              size="mini"
              type="danger"
              icon="el-icon-delete"
              @click="handleDelete(scope.$index, scope.row)">删除
            </el-button>
          </template>
        </el-table-column>
      </el-table>

      <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="70px" class="demo-ruleForm" size="medium">
        <el-dialog
          title="添加"
          :append-to-body='true'
          :visible.sync="dialogVisible"
          width="80%"
          :before-close="handleClose">
          <el-input type="hidden" v-model="ruleForm.id"/>
          <el-form-item label="姓名" prop="name">
            <el-input v-model="ruleForm.name"></el-input>
          </el-form-item>
          <el-form-item label="年龄" prop="age">
            <el-input v-model="ruleForm.age"></el-input>
          </el-form-item>

          <span slot="footer" class="dialog-footer">
            <el-button @click="cancel()" size="medium">取 消</el-button>
            <el-button @click="addUser()" type="primary" size="medium">确 定</el-button>
          </span>
        </el-dialog>
      </el-form>

    <el-form :model="ruleForm" :rules="rules" ref="ruleForm" label-width="70px" class="demo-ruleForm" size="medium">
      <el-dialog
        title="编辑"
        :append-to-body='true'
        :visible.sync="dialogUpdate"
        width="80%"
        :before-close="handleClose">
        <el-input type="hidden" v-model="ruleForm.id"/>
        <el-form-item label="姓名" prop="name">
          <el-input v-model="ruleForm.name"></el-input>
        </el-form-item>
        <el-form-item label="年龄" prop="age">
          <el-input v-model="ruleForm.age"></el-input>
        </el-form-item>

        <span slot="footer" class="dialog-footer">
            <el-button @click="cancel()" size="medium">取 消</el-button>
            <el-button @click="updateUser()" type="primary" size="medium">确 定</el-button>
          </span>
      </el-dialog>
    </el-form>
      <br>
      <div class="pages">
        <el-pagination
          background
          :disabled = "disablePage"
          :current-page.sync="currentPage"
          small
          layout="prev, pager, next"
          :page-size="pageSize"
          :total="total"
          @current-change="handleCurrentChange">
        </el-pagination>
      </div>
  </div>
</template>

<script>
    export default {
        data() {
            return {
                ruleForm: {
                    id: '',
                    name: '',
                    age: '',
                },
                rules: {
                    name: [
                        { required: true, message: '请输入姓名', trigger: 'blur' },
                    ],
                    age: [
                        { required: true, message: '请输入年龄', trigger: 'blur' },
                    ],
                },
                tableData: [],
                search: '',
                dialogVisible: false,
                dialogUpdate: false,
                pageSize: 5,
                currentPage: 1,
                total: 0,
                disablePage: false
            }
        },
        methods: {
            handleEdit(index, row) {
                this.dialogUpdate = true;
                this.ruleForm = Object.assign({}, row); //这句是关键！！！
            },
            handleDelete(index, row) {
                console.log(index, row);
                this.$confirm('删除操作, 是否继续?', '提示', {
                    confirmButtonText: '确定',
                    cancelButtonText: '取消',
                    type: 'warning'
                }).then(() => {
                    let postData = this.qs.stringify({
                        id: row.id,
                    });
                    this.axios({
                        method: 'post',
                        url:'/delete',
                        data:postData
                    }).then(response =>
                    {
                        this.getPages();
                        this.currentPage = 1;
                        this.axios.post('/page').then(response =>
                        {
                            this.tableData = response.data;
                        }).catch(error =>
                        {
                            console.log(error);
                        });
                        this.$message({
                            type: 'success',
                            message: '删除成功!'
                        });
                        console.log(response);
                    }).catch(error =>
                    {
                        console.log(error);
                    });

                }).catch(() => {
                    this.$message({
                        type: 'info',
                        message: '已取消删除'
                    });
                });
            },
            handleClose(done) {
                this.$confirm('确认关闭？')
                    .then(_ => {
                        done();
                    })
                    .catch(_ => {});
            },
            handleCurrentChange() {
                console.log(`当前页: ${this.currentPage}`);
                let postData = this.qs.stringify({
                    page: this.currentPage
                });
                this.axios({
                    method: 'post',
                    url:'/page',
                    data:postData
                }).then(response =>
                {
                    this.tableData = response.data;
                }).catch(error =>
                {
                    console.log(error);
                });
            },
            cancel() {
                this.dialogUpdate = false;
                this.dialogVisible = false;
                this.emptyUserData();
            },
            emptyUserData(){
                this.ruleForm = {
                    name: '',
                    age: '',
                    id: ''
                }
            },
            addUser() {
                let postData = this.qs.stringify({
                  name: this.ruleForm.name,
                  age: this.ruleForm.age
                    
                });
                this.axios({
                    method: 'post',
                    url:'/insert',
                    data:postData
                }).then(response =>
                {
                    this.axios.post('/page').then(response =>
                    {
                        this.tableData = response.data;
                        this.currentPage = 1;
                        this.$message({
                            type: 'success',
                            message: '已添加!'
                        });
                    }).catch(error =>
                    {
                        console.log(error);
                    });
                    this.getPages();
                    this.dialogVisible = false
                    console.log(response);
                }).catch(error =>
                {
                    console.log(error);
                });
            },
            updateUser() {
                let postData = this.qs.stringify({
                    id: this.ruleForm.id,
                    age: this.ruleForm.age,
                    name: this.ruleForm.name,
                   
                });
                this.axios({
                    method: 'post',
                    url:'/update',
                    data:postData
                }).then(response =>
                {
                    this.handleCurrentChange();
                    this.cancel();
                    this.$message({
                        type: 'success',
                        message: '更新成功!'
                    });
                    console.log(response);
                }).catch(error =>
                {
                    this.$message({
                        type: 'success',
                        message: '更新失败!'
                    });
                    console.log(error);
                });
            },
            onSearch() {
                let postData = this.qs.stringify({
                    name: this.search
                });
                this.axios({
                    method: 'post',
                    url: '/ListByName',
                    data: postData
                }).then(response =>
                {
                    this.tableData = response.data;
                    this.disablePage = true;
                }).catch(error =>
                {
                    console.log(error);
                });
            },
            getPages() {
                this.axios.post('/rows').then(response =>
                {
                    this.total = response.data;
                }).catch(error =>
                {
                    console.log(error);
                });
            },
            refreshData() {
                location.reload();
            }
        },
        created() {
            /*this.axios.get('static/user.json').then(response =>
            {
                this.tableData = response.data.tableData;
                this.total = response.data.tableData.length;
                // console.log(JSON.parse(JSON.stringify(response.data))['tableData'])
            });*/
            this.axios.post('/page').then(response =>
            {
                this.tableData = response.data;
            }).catch(error =>
            {
                console.log(error);
            });

            this.axios.post('/rows').then(response =>
            {
                this.total = response.data;
            }).catch(error =>
            {
                console.log(error);
            });

        },
    }
</script>
<style scoped>
  .search_name{
    width: 200px;
  }
  .pages{
    margin: 0px;
    padding: 0px;
    text-align: right;
  }
</style>
```



6. Dao层

```java
@Mapper
public interface UserMapper {

    User findById(int id);

    List<User> queryPage(Integer startRows);

    int getRowCount();

    User insert(User user);

    int delete(int id);

    int update(User user);

    List<User> findByName(String name);

    List<User> listUser();
}
```



7. Mapper文件

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.yxg.mapper.UserMapper">
    <resultMap id="result" type="com.yxg.pojo.User">
        <result property="id" column="id"/>
        <result property="name" column="name"/>
        <result property="age" column="age"/>
    </resultMap>
    <select id="findById" parameterType="int" resultType="com.yxg.pojo.User">
        select *
        from user
        where id = #{id}
    </select>
    <select id="queryPage" resultMap="result" parameterType="Integer">
        select *
        from user
        order by id desc
        limit #{startRows},5
    </select>

    <select id="getRowCount" resultType="Integer">
        select count(*)
        from user
    </select>

    <select id="listUser" resultMap="result">
        SELECT *
        FROM user
    </select>

    <select id="findByName" resultMap="result" parameterType="String">
        SELECT *
        FROM user
        where name like concat(concat('%', #{name}), '%')
        order by id desc
    </select>


    <insert id="insert" parameterType="com.yxg.pojo.User">
        INSERT INTO user
            (name, age)
        VALUES (#{name},
                #{age})
    </insert>

    <delete id="delete" parameterType="int">
        delete
        from user
        where id = #{id}
    </delete>

    <update id="update" parameterType="com.yxg.pojo.User">
        update user
        set user.name=#{name},
            user.age=#{age}
        where user.id = #{id}
    </update>
</mapper>
```



8. Service层

- Service接口

```java
public interface UserService {
    User findById(int id);

    List<User> queryPage(Integer startRows);

    int getRowCount();

    List<User> findByName(String name);

    User insert(User user);

    List<User> listUser();

    int update(User user);

    int delete(int id);
}
```

- Service接口实现类

```java
@Service
public class UserServiceImpl implements UserService {
    @Autowired
    private UserMapper userMapper;

    @Override
    public User findById(int id) {
        return userMapper.findById(id);
    }

    @Override
    public List<User> queryPage(Integer startRows) {
        return userMapper.queryPage(startRows);
    }

    @Override
    public int getRowCount() {
        return userMapper.getRowCount();
    }

    @Override
    public List<User> findByName(String name) {
        return userMapper.findByName(name);
    }

    @Override
    public User insert(User user) {
        return userMapper.insert(user);
    }

    @Override
    public List<User> listUser() {
        return userMapper.listUser();
    }

    @Override
    public int update(User user) {
        return userMapper.update(user);
    }

    @Override
    public int delete(int id) {
        return userMapper.delete(id);
    }
}

```



9. Controller层

```java
@RestController
@Slf4j
public class UserController {

    //private final Logger logger = LoggerFactory.getLogger(UserController.class);

    @Autowired
    private UserService userService;

    @GetMapping("/user/{id}")
    public User findById(@PathVariable("id") int id) {
        User user = userService.findById(id);
        log.info("logTest,User:{}", user);
        return user;
    }

    @RequestMapping("/page")
    @ResponseBody
    public List<User> page(Integer page) {
        int pageNow = page == null ? 1 : page;
        int pageSize = 5;
        int startRows = pageSize * (pageNow - 1);
        return userService.queryPage(startRows);
    }

    @RequestMapping("/rows")
    @ResponseBody
    public int rows() {
        return userService.getRowCount();
    }

    @RequestMapping(value = "/delete", method = RequestMethod.POST)
    public Integer delete(Integer id) {
        return userService.delete(id);
    }

    @RequestMapping(value = "/update", method = RequestMethod.POST)
    @ResponseBody
    public String update(User user) {
        int result = userService.update(user);
        if (result >= 1) {
            return "修改成功";
        } else {
            return "修改失败";
        }
    }

    @RequestMapping(value = "/insert", method = RequestMethod.POST)
    @ResponseBody
    public User insert(User user) {
        return userService.insert(user);
    }

    @RequestMapping("/ListUser")
    @ResponseBody
    public List<User> listUser() {
        return userService.listUser();
    }

    @RequestMapping("/ListByName")
    @ResponseBody
    public List<User> findByName(String name) {
        return userService.findByName(name);
    }
}
```



10. 启动测试

![image-20220119172547059](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201191725152.png)

- 查询

![image-20220119172519288](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201191725375.png)

- 增加

![image-20220119172918119](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201191729195.png)

![image-20220119172710220](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201191727318.png)

- 修改

![image-20220119173046244](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201191730321.png)

![image-20220119173052576](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201191730660.png)

- 删除

![image-20220119172849949](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201191728061.png)

![image-20220119172854114](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201191728194.png)
