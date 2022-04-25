# Centoos7安装kafka

> 因为Kafka依赖与zookeeper，而zookeeper又需要jdk环境，所以先安装jdk，在安装zookeeper，最后安装Kafka

1. 安装jdk1.8

- 卸载openjdk,`有就删掉，没有忽略此步骤`

```shell
java –version
rpm -qa | grep java
rpm -e --nodeps java-1.7.0-openjdk-1.7.0.45-2.4.3.3.el6.x86_64
rpm -e --nodeps java-1.6.0-openjdk-1.6.0.0-1.66.1.13.0.el6.x86_64
```

- 下载jdk1.8[官网地址](https://www.oracle.com/java/technologies/downloads/#java8)

![image-20220118181229003](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201181812272.png)

- 上传至Centos系统，一般安装到/user/local目录(远程工具都可以上传)
- 创建java文件夹，解压到/usr/local/java目录下

```shell
mkdir /usr/local/java
tar -xvf jdk-8u311-linux-x64.tar.gz /usr/local/java
```

- 配置jdk的环境变量

```shell
vim /etc/profile

#在末尾行添加
# set java environment
JAVA_HOME=/usr/local/java/jdk1.8.0_311
CLASSPATH=.:$JAVA_HOME/lib.tools.jar
PATH=$JAVA_HOME/bin:$PATH
export JAVA_HOME CLASSPATH PATH
#保存退出

source /etc/profile  #使更改的配置立即生效
```

- 检查安装结果

```shell
java -version
```

![image-20220118182518044](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201181825090.png)



2. 安装Kafka

- [从官网获取Kafka](https://kafka.apache.org/downloads)

![image-20220118183522686](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/202201181835847.png)

- 上传至Centos
- 解压安装

```shell
tar -xvf kafka_2.11-1.1.0.tgz /usr/local/kafka
```

- 启动zookeeper

> kafka中自带了的zookeeper

```shell
cd /usr/local/kafka/
./bin/zookeeper-server-start.sh ./config/zookeeper.properties
```

- 配置Kafka

```shell
vim config/server.properties
```

增加以下配置

```shell
listeners=PLAINTEXT://0.0.0.0:9092
advertised.listeners=PLAINTEXT://101.35.158.207:9092
```

> host.name表示kafka绑定到的ip上，此处设置为0.0.0.0，保证非本机的请求也能被接受
> advertised.host.name表示kafka注册到zookeeper时，将告诉zookeeper自己的地址

- 启动Kafka

```shell
./bin/kafka-server-start.sh ./config/server.properties
```

