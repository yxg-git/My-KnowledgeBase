# Docker命令

### Docker服务相关命令
```shell
# 启动docker服务
systemctl start docker
# 停止docker服务
systemctl stop docker
# 重启docker服务
systemctl restart docker
# 查看docker状态
systemctl status docker
# 设置开机启动docker
systemctl enable docker
```
### 镜像相关命令
```shell
# 查看镜像 
docker images 
# 搜索镜像
docker search redis # 一Redis为例
# 拉取镜像
docker pull redis:5.0 # 镜像名:版本号，版本号可以到https://hub.docker.com/中查找
# 删除镜像
docker rmi 'image id' #根据id删除
docker rmi '镜像名:版本号'
docker rmi `docker images -q` # 删除所有
```

### 容器相关命令

```shell
# 查看容器
docker ps # 查看正在运行的容器
docker ps -a # 查看所有容器
# 创建容器
docker run -it --name=c1 centos:7 /bin/bash # 自动打开一个终端，退出后容器关闭
docker run -id --name=c2 redis:5.0 #在后台运行，退出后容器不关闭
# 退出容器
exit
# 进入容器
docker exec -it c2 /bin/bash
# 启动容器
docker start '容器名称'
# 停止容器
docker stop '容器名称'
# 删除容器
docker rm '容器名称' # 也可以根据id删除
docker rm `docker ps -aq` # 删除所有容器
# 查看容器信息
docker inspect '容器名称'
```
参数说明：
- -i：保持容器运行。通常与 -t 同时使用。加入it这两个参数后，容器创建后自动进入容器中，退出- 容器后，容器自动关闭。
- -t：为容器重新分配一个伪输入终端，通常与 -i 同时使用。
- -d：以守护（后台）模式运行容器。创建一个容器在后台运行，需要使用docker exec 进入容器。退出后，容器不会关闭。
- -it：创建的容器一般称为交互式容器，-id 创建的容器一般称为守护式容器
- --name：为创建的容器命名。

### docker容器数据卷

```shell
docker run ... '–v 宿主机目录(文件):容器内目录(文件)' ...
docker run -it --name=centos -v /root/data:/root/data_container centos:7 /bin/bash
```
注意事项：
  1. 目录必须是绝对路径
  2. 如果目录不存在，会自动创建
  3. 可以挂载多个数据卷

- #### 配置数据卷容器
1. 创建启动c3数据卷容器，使用 –v 参数 设置数据卷

```shell
docker run -it --name=c3 -v /volume centos:7
```
2. 创建启动 c1 c2 容器，使用 –-volumes-from 参数 设置数据卷

```shell
docker run -it --name=c1 --volumes-from c3 centos:7
docker run -it --name=c2 --volumes-from c3 centos:7
```

