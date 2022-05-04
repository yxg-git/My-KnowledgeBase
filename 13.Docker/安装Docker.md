# Docker安装

1. 更新yum包

   ```shell
   yum update
   ```

2. 安装需要的软件包

   ```shell
   yum install -y yum-utils device-mapper-persistent-data lvm2
   ```

3. 设置yum源

   ```shell
   yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
   ```

4. 安装docker，出现输入界面都按y

   ```shell
   yum install -y docker-ce
   ```

5. 查看docker版本

   ```shell
   docker -v
   ```

   