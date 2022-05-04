# Docker常用应用部署

## 一、部署MySQL

1. 搜索mysql镜像

```shell
docker search mysql
```

2. 拉取mysql镜像

```shell
docker pull mysql:5.6
```

3. 创建容器，设置端口映射、目录映射

```shell
# 在/root目录下创建mysql目录用于存储mysql数据信息
mkdir ~/mysql
cd ~/mysql
```

```shell
docker run -id \
-p 3307:3306 \
--name=docker_mysql \
-v $PWD/conf:/etc/mysql/conf.d \
-v $PWD/logs:/logs \
-v $PWD/data:/var/lib/mysql \
-e MYSQL_ROOT_PASSWORD=root \
mysql:5.6
```

- 参数说明：
  - **-p 3307:3306**：将容器的 3306 端口映射到宿主机的 3307 端口。
  - **-v $PWD/conf:/etc/mysql/conf.d**：将主机当前目录下的 conf/my.cnf 挂载到容器的 /etc/mysql/my.cnf。配置目录
  - **-v $PWD/logs:/logs**：将主机当前目录下的 logs 目录挂载到容器的 /logs。日志目录
  - **-v $PWD/data:/var/lib/mysql** ：将主机当前目录下的data目录挂载到容器的 /var/lib/mysql 。数据目录
  - **-e MYSQL_ROOT_PASSWORD=123456：**初始化 root 用户的密码。

4. 进入容器，操作mysql

```shell
docker exec -it c_mysql /bin/bash
```

5. 检查访问权限

   ```sql
   use mysql；
   select user,host from user;
   
   ## 如果不存在user权限
   update user set host='%' where user='root';
   ```

6. 修改密码

   ```sql
   ALTER USER 'root'@'localhost' IDENTIFIED BY '新密码';
   alter user 'root'@'%' identified with mysql_native_password BY '新密码';
   flush privileges;
   ```



## 二、部署Tomcat

1. 搜索tomcat镜像

```shell
docker search tomcat
```

2. 拉取tomcat镜像

```shell
docker pull tomcat
```

3. 创建容器，设置端口映射、目录映射

```shell
4. 在/root目录下创建tomcat目录用于存储tomcat数据信息
mkdir ~/tomcat
cd ~/tomcat
```

```shell
docker run -id --name=docker_tomcat \
-p 8080:8080 \
-v $PWD:/usr/local/tomcat/webapps \
tomcat 
```

- 参数说明：
  - **-p 8080:8080：**将容器的8080端口映射到主机的8080端口
  
    **-v $PWD:/usr/local/tomcat/webapps：**将主机中当前目录挂载到容器的webapps




## 三、部署Nginx

1. 搜索nginx镜像

```shell
docker search nginx
```

2. 拉取nginx镜像

```shell
docker pull nginx
```

3. 创建容器，设置端口映射、目录映射


```shell
# 在/root目录下创建nginx目录用于存储nginx数据信息
mkdir ~/nginx
cd ~/nginx
mkdir conf
cd conf
# 在~/nginx/conf/下创建nginx.conf文件,粘贴下面内容
vim nginx.conf
```
```shell

user  nginx;
worker_processes  1;

error_log  /var/log/nginx/error.log warn;
pid        /var/run/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       /etc/nginx/mime.types;
    default_type  application/octet-stream;

    log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
                      '$status $body_bytes_sent "$http_referer" '
                      '"$http_user_agent" "$http_x_forwarded_for"';

    access_log  /var/log/nginx/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    keepalive_timeout  65;

    #gzip  on;

    include /etc/nginx/conf.d/*.conf;
}


```

4. 运行容器


```shell
docker run -id --name=docker_nginx \
-p 80:80 \
-v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf \
-v $PWD/logs:/var/log/nginx \
-v $PWD/html:/usr/share/nginx/html \
nginx
```

- 参数说明：
  - **-p 80:80**：将容器的 80端口映射到宿主机的 80 端口。
  - **-v $PWD/conf/nginx.conf:/etc/nginx/nginx.conf**：将主机当前目录下的 /conf/nginx.conf 挂载到容器的 :/etc/nginx/nginx.conf。配置目录
  - **-v $PWD/logs:/var/log/nginx**：将主机当前目录下的 logs 目录挂载到容器的/var/log/nginx。日志目录

## 四、部署Redis

1. 搜索redis镜像

```shell
docker search redis
```

2. 拉取redis镜像

```shell
docker pull redis:5.0
```

3. 创建容器，设置端口映射

```shell
# 1.在根目录下创建redis目录
mkdir ~/redis/{conf,data} -p
cd redis

# 2.获取redis的默认配置模板
wget https://raw.githubusercontent.com/antirez/redis/5.0/redis.conf -O conf/redis.conf

# 3.替换编辑
sed -i 's/logfile ""/logfile "access.log"/' conf/redis.conf
sed -i 's/# requirepass foobared/requirepass 123456/' conf/redis.conf
sed -i 's/appendonly no/appendonly yes/' conf/redis.conf
sed -i 's/bind 127.0.0.1/#bind 127.0.0.1/' conf/redis.conf
sed -i 's/protected-mode yes/protected-mode no/' conf/redis.conf

# 4.创建并运行redis容器
docker run \
-d \
--name docker_redis \
-p 9736:6379 \
--privileged=true \
-v $PWD/data:/data \
-v $PWD/conf/redis.conf:/etc/redis/redis.conf \
redis:5.0 redis-server /etc/redis/redis.conf \
```

> 只要可以记得住这里端口映射随便弄，我是因为我的阿里云服务器开通6379端口一直遭到挖矿才改的。

## 五、部署rabbitmq

1. 搜索rabbitmq镜像

   ```shell
   docker search rabbitmq
   ```

2. 拉取rabbitmq镜像

   ```shell
   docker pull rabbitmq:3.9.11-management
   ```

3. 创建容器，设置端口映射

   ```shell
   docker run -dit --name docker_rabbitmq -e RABBITMQ\_DEFAULT\_USER=guest -e RABBITMQ\_DEFAULT\_PASS=guest -p 15672:15672 -p 5672:5672 rabbitmq:3.9.11-management
   ```


## 五、部署Kafka

- 拉取zookeeper镜像

```shell
docker pull wurstmeister/zookeeper
```

- 启动zookeeper容器

```shell
docker run -d --name zookeeper -p 2181:2181 -t wurstmeister/zookeeper
```

- 拉去Kafka镜像

```shell
docker pull wurstmeister/kafka
docker pull sheepkiller/kafka-manager
```

- 启动Kafka容器

```shell
docker run  -d --name kafka \
-p 9092:9092 \
-e KAFKA_BROKER_ID=0 \
-e KAFKA_ZOOKEEPER_CONNECT=192.168.43.33:2181 \
-e KAFKA_ADVERTISED_LISTENERS=PLAINTEXT://192.168.43.33:9092 \
-e KAFKA_LISTENERS=PLAINTEXT://0.0.0.0:9092 -t wurstmeister/kafka 

docker run -d --name kafka-manager \
--link zookeeper:zookeeper \
--link kafka:kafka -p 9000:9000 \
--restart=always \
--env ZK_HOSTS=zookeeper:2181 \
sheepkiller/kafka-manager
```

- 进入docker

```shell
docker exec -ti kafka /bin/bash
```





