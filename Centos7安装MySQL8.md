# Centos7安装MySQL8

1. 下载rpm包

   ```shell
   wget https://cdn.mysql.com/archives/mysql-8.0/mysql-8.0.16-2.el7.x86_64.rpm-bundle.tar
   ```

2. 解压rpm包

   ```shell
   tar -xvf mysql-8.0.16-2.el7.x86_64.rpm-bundle.tar
   ```

   解压后包含8个文件

   ```shell
   mysql-community-libs-8.0.16-2.el7.x86_64.rpm
   
   mysql-community-embedded-compat-8.0.16-2.el7.x86_64.rpm
   
   mysql-community-devel-8.0.16-2.el7.x86_64.rpm
   
   mysql-community-server-8.0.16-2.el7.x86_64.rpm
   
   mysql-community-libs-compat-8.0.16-2.el7.x86_64.rpm
   
   mysql-community-client-8.0.16-2.el7.x86_64.rpm
   
   mysql-community-common-8.0.16-2.el7.x86_64.rpm
   
   mysql-community-test-8.0.16-2.el7.x86_64.rpm
   ```

3. 检测centos系统中自带的mariadb数据库

   ```shell
   rpm -qa | grep -i mariadb
   ```

   如果存在，显示结果：

   ```shell
   mariadb-libs-5.5.65-1.el7.x86_64
   ```

   去除依赖

   ```shell
   rpm -ev  --nodeps mariadb-libs-5.5.65-1.el7.x86_64
   ```

4. 安装，安装顺序按`common->libs->client->server`的顺序安装

   ```shell
   rpm -ivh mysql-community-common-8.0.16-2.el7.x86_64.rpm
   
   rpm -ivh mysql-community-libs-8.0.16-2.el7.x86_64.rpm
   
   rpm -ivh mysql-community-client-8.0.16-2.el7.x86_64.rpm
   
   rpm -ivh mysql-community-server-8.0.16-2.el7.x86_64.rpm
   ```

5. 启动与修改密码

   ```shell
    systemctl start mysqld # 启动
    
    systemctl status mysqld # 查看状态
   ```

   ![image-20220303141134373](https://yxgspace.oss-cn-beijing.aliyuncs.com/img/image-20220303141134373.png)

​	表示安装成功，启动正常

6. 查看初始随机密码

   ```shell
    cat /var/log/mysqld.log | grep password
   ```

   显示以下内容：

   ```shell
   2020-09-02T05:30:06.739311Z 5 [Note] [MY-010454] [Server] A temporary password is generated for root@localhost: tC0;+kB?BqCg
   ```

7. 使用root角色登录

   ```shell
   mysql -u root -p
   ```

   粘贴初始密码后复制

8. 修改密码，按照MySQL8.0默认的密码组件，此时的密码要求是必须包含数字，大小写字母，特殊字符，且长度不低于8位，否则会提示密码不符合规则。

   ```shell
   ALTER user 'root'@'localhost' IDENTIFIED BY 'Root.123456';
   ```

9. 开放远程登录权限

   ```shell
   use mysql;
   
   select host,user from user;
   
   update user set host='%' where user ='root';
   
   flush privileges; # 刷新权限
   ```

10. 开放防火墙端口

    ```shell
    systemctl start firewalld # 开启防火墙
    
    firewall-cmd --query-port=3306/tcp # 查询是否开放3306端口
    
    firewall-cmd --zone=public --add-port=3306/tcp --permanent # 开放3306端口
    
    firewall-cmd --reload # 重新加载防火墙
    ```

11. 远程连接

    如果在阿里云或者腾讯云中安装mysql，使用navicat或者sqlyong连接不上，到阿里云或腾讯云控制台开放2206端口